Building from Source
====================

This page explains how you can build aergo from source.

If you are only interested in running the aergo tools, it is easier to use pre-built binaries or Docker images instead, as explained in `Quickstart <../running-node/quickstart.html>`_.

Linux
-----

1. Install dependencies
   ::

        apk update
        apk add git build-base go libgcc

2. Install aergo
   ::

        go get -d github.com/aergoio/aergo
        cd ${GOPATH}/src/github.com/aergoio/aergo
        git submodule init && git submodule update
        make

3. Set environment
   ::
      export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:${GOPATH}/src/github.com/aergoio/aergo/libtool/lib

macOS
-----

1. If you haven't already, install `homebrew <https://brew.sh/>`_.
2. Install dependencies
   ::

        brew install go
        brew install cmake  # optional
        brew install protobuf  # optional

3. Install aergo
   ::

        go get -d github.com/aergoio/aergo
        cd `go env GOPATH`/src/github.com/aergoio/aergo
        git submodule init
        git submodule update
        make

4. Set environment
   ::
      export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:${GOPATH}/src/github.com/aergoio/aergo/libtool/lib

5. Run server
   ::

        ./bin/aergosvr

Windows
-------

Building on Windows is currently not supported.

Generating protobuf files
-------------------------

If you changed the protobuf files, you need to re-compile the type definitions.

1. Install protoc-gen-go
   ::

        GIT_TAG="v1.1.0" # set version of protoc-gen-go to 1.1.0, on which aergo v0.8.x depends.
        go get -u github.com/golang/protobuf/protoc-gen-go
        git -C "$(go env GOPATH)"/src/github.com/golang/protobuf checkout $GIT_TAG
        go install github.com/golang/protobuf/protoc-gen-go

2. Generate type definitions from protobuf files
   ::

        make protoc
