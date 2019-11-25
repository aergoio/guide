Manually creating peer identification
=====================================

Every peer (i.e. instance of aergosvr) is identified by a peer id, derived from its public key.
If unspecified, aergosvr creates these automatically, but you may want to do it manually to control the generation of keys and make backups.
You can use the CLI to do that:

.. code:: shell

    aergocli keygen your-keyname

    # Using Docker:
    docker run --rm -v $(pwd):/tools/ aergo/tools aergocli keygen your-keyname

You then need to specify the path to the generated your-keyname.key file in the Aergo configuration:

.. code:: 

   ...
   [p2p]
   npkey = "/aergo/your-keyname.key"
   ...