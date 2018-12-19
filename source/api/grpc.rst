Using gRPC
==========

gRPC is a standard for RPC APIs using protobuffer messages for data exchange.
The main advantages are typed messages and efficiency: data is packed tightly and sent over HTTP2.

If you want to use the gRPC API directly, please refer to the `official documentation <https://grpc.io/>`_ to get started.
You can find the protobuf definitions in [aergoio/aergo-protobuf](https://github.com/aergoio/aergo-protobuf).

Alternatively, you can use one of the `SDKs <../sdks/index.html>`_ which wrap the same functionality in language-specific styles.