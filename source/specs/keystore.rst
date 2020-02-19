Keystore
========

.. note::

    If you or your users are storing significant amounts of values in an Aergo account, be advised that the only really
    secure way of storing private keys is using dedicated, offline hardware like a hardware wallet.
    This article is not a operations security guide. Please do your own research about private key security.

Securing private keys is the responsibility of client software, but Aergo has a recommended storage format for increased portability.
This storage format is also used internally when storing accounts in aergocli or aergosvr.

This so-called Keystore format defines a file format (json) and encryption scheme.

File format
-----------

Filename: :code:`{address}__keystore.txt`

.. code:: json

    {
      "aergo_address": "Amdxxxx",
      "ks_version": "1",
      "kdf": {
        "algorithm": "algorithm_name",
        "params": {
          "some_param": "some_value"
        },
        "mac": "mesasge_authentication_code"
      },
      "cipher": {
        "algorithm": "algorithm_name",
        "params": {
          "some_param": "some_value"
        },
        "ciphertext": "dxxxx",
      }
    }

**Rules**

- Version field is for choosing cipher algorithm. If version updates, cipher algorithm would be updated (also format can change).
- Every file has its own algorithm and parameters per version. If any of the parameters break, an error should be thrown.
- File name is '{address}__keystore.txt' or '{alias}__keystore.txt'
- File name is used to identify keystore file. Duplicates are not allowed.
- For the alias file, you can have same keystore file representing same account using different alias. That's fine.
- Address must be a valid base58-check encoded address. Alias must be form of :code:`[a-zA-Z0-9]+`

Version 1 encryption scheme
---------------------------

Example implementations: `Javascript <https://github.com/aergoio/herajs/blob/385b93d186569456280e235e3bd3a4f595057c50/packages/crypto/src/keystore.ts>`__

**Encrypt**

.. code::

    encryptionKey = Scrypt(rawPassword, scryptParams)
    cipherText = AES_CTR.encrypt(rawPrivateKey, encryptionKey[0:16], nonce)
    mac = Sha256(encryptionKey[16:32] + cipherText)
    keystore = {
      "aergo_address": "Amdxxxx",
      "ks_version": "1",
      "kdf": {
        "algorithm": "scrypt",
        "params": scryptParams,
        "mac": mac
      },
      "cipher": {
        "algorithm": "aes-128-ctr",
        "params": {
          "iv": nonce
        },
        "ciphertext": cipherText
      }
    }

**Decrypt**

.. code::

    encryptionKey = Scrypt(rawPassword, keystore.kdf.params)
    mac = Sha256(encryptionKey[16:32] + json.cipher.ciphertext)
    if keystore.kdf.mac != mac {
      throw "invalid mac"
    }
    rawPrivateKey = AES_CTR.decrypt(json.cipher.ciphertext, encryptionKey[0:16], json.cipher.params.iv)

**Recommended parameters**

.. code:: javascript

    {
        kdfAlgorithm: 'scrypt',
        cipherAlgorithm: 'aes-128-ctr',
        kdfParams: {
            dklen: 32,
            n: 1 << 18,
            p: 1,
            r: 8,
        },
    }