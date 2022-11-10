# OpenTelemetry C++  Example

## HTTP via HTTP

This is a simple example that demonstrates tracing an HTTP request from client
to server. The example shows several aspects of tracing such as:

* Using the `TracerProvider`
* Using the `GlobalPropagator`
* Span Attributes
* Span Events
* Using the HTTP OTLP exporter, sending to Honeycomb
* W3C Trace Context Propagation

### Running the example

1. The example uses HTTP server and client provided as part of this repo:
    * [HTTP
      Client](https://github.com/open-telemetry/opentelemetry-cpp/tree/main/ext/include/opentelemetry/ext/http/client)
    * [HTTP
      Server](https://github.com/open-telemetry/opentelemetry-cpp/tree/main/ext/include/opentelemetry/ext/http/server)

2. Build and Deploy the opentelementry-cpp as described in
   [INSTALL.md](../../INSTALL.md)

   For the first `cmake` command, add some gRPC-related flags so it builds the right stuff.

   ```console
   $ cmake -DBUILD_TESTING=OFF -DWITH_OTLP=ON -DWITH_OTLP_HTTP=ON ..
   ```

   Make sure your system has `protobuf` installed.

   For example, with the APT package manager you can install protobuf this way.

   ```console
   $ apt-get install libprotobuf-dev protobuf-compiler
   ```

3. Start the server from the `examples/http` directory

   ```console
   $ export OTEL_EXPORTER_OTLP_ENDPOINT=https://api.honeycomb.io
   $ export OTEL_RESOURCE_ATTRIBUTES=service.name=cpp-http-server-http
   $ export OTEL_EXPORTER_OTLP_PROTOCOL=http
   $ export OTEL_EXPORTER_OTLP_HEADERS="x-honeycomb-team=your-api-key"
   $ export OTEL_EXPORTER_OTLP_SSL_ENABLE=true
   $ http_server 8800
    Server is running..Press ctrl-c to exit...
   ```

4. In a separate terminal window, run the client to make a single request:

   ```console
   $ export OTEL_EXPORTER_OTLP_ENDPOINT=https://api.honeycomb.io
   $ export OTEL_RESOURCE_ATTRIBUTES=service.name=cpp-http-client-http
   $ export OTEL_EXPORTER_OTLP_PROTOCOL=http
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
