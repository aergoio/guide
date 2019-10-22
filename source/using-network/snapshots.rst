Snapshots
=========

To quickly bootstrap a new full node, you can start syncronizing from a snapshot.

1. Download the latest snapshot from `snapshot.aergo.io <https://snapshot.aergo.io>`__.
2. Extract the archive, e.g. :code:`tar zxvf 672571_20190423.tar.gz`.
3. Start the node, pointing it to the extracted data directory: :code:`docker run -v $(pwd)/data:/aergo/data -p 7845:7845 -p 7846:7846 aergo/node`