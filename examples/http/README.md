# OpenTelemetry C++  Example

## HTTP

This is a simple example that demonstrates tracing an HTTP request from client
to server. The example shows several aspects of tracing such as:

* Using the `TracerProvider`
* Using the `GlobalPropagator`
* Span Attributes
* Span Events
* Using the GRPC exporter, sending to Honeycomb
* W3C Trace Context Propagation

### Running the example

1. The example uses HTTP server and client provided as part of this repo:
    * [HTTP
      Client](https://github.com/open-telemetry/opentelemetry-cpp/tree/main/ext/include/opentelemetry/ext/http/client)
    * [HTTP
      Server](https://github.com/open-telemetry/opentelemetry-cpp/tree/main/ext/include/opentelemetry/ext/http/server)

2. Build and Deploy the opentelementry-cpp as described in
   [INSTALL.md](../../INSTALL.md)

   Make sure your system has `protobuf` and `grpc` installed as described in
   their repositories.

   For example, with the APT package manager you can install protobuf this way.

   ```console
   $ apt-get install libprotobuf-dev protobuf-compiler
   ```

   gRPC requires a bit more work so follow [their BUILDING.md guide](https://github.com/grpc/grpc/blob/master/BUILDING.md).

   ```console
   $ apt-get install build-essential autoconf libtool pkg-config cmake
   $ git clone -b 1.50.1 https://github.com/grpc/grpc
   $ cd grpc
   $ git submodule update --init
   $ mkdir -p cmake/build
   $ cd cmake/build
   $ cmake ../..
   $ make
   $ make install
   ```

3. Start the server from the `examples/http` directory

   ```console
   $ export OTEL_EXPORTER_OTLP_ENDPOINT=api.honeycomb.io:443
   $ export OTEL_EXPORTER_OTLP_PROTOCOL=grpc
   $ export OTEL_RESOURCE_ATTRIBUTES=service.name=cpp-http-server
   $ export OTEL_EXPORTER_OTLP_HEADERS="x-honeycomb-team=your-api-key"
   $ export OTEL_EXPORTER_OTLP_SSL_ENABLE=true
   $ http_server 8800
    Server is running..Press ctrl-c to exit...
   ```

4. In a separate terminal window, run the client to make a single request:

   ```console
   $ export OTEL_EXPORTER_OTLP_ENDPOINT=api.honeycomb.io:443
   $ export OTEL_EXPORTER_OTLP_PROTOCOL=grpc
   $ export OTEL_RESOURCE_ATTRIBUTES=service.name=cpp-http-client
   $ export OTEL_EXPORTER_OTLP_HEADERS="x-honeycomb-team=your-api-key"
   $ export OTEL_EXPORTER_OTLP_SSL_ENABLE=true
   $ ./http_client 8800
   ...
   ...
   ```

5. You should see the spans show up in Honeycomb within a few seconds.
   This example creates a client side span, server side span, and span
   event for each time the client application is run.

## Troubleshooting

If you do not see spans showing up, you may see an error in the console.
If you do not see an error, enable debugging in the GRPC library:

   ```console
   $ export GRPC_VERBOSITY=debug
   $ export GPRC_TRACE=http
   ```

For additional debugging information, you can set `GRPC_TRACE=all`.
