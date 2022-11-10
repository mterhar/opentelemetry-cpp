find_package(CURL)

if(NOT CURL_FOUND)
  message(WARNING "Skipping http client/server example build: CURL not found")
else()
  include_directories(${CMAKE_BINARY_DIR}/generated/third_party/opentelemetry-proto
                      ${CMAKE_SOURCE_DIR}/exporters/otlp/include
                      ${CMAKE_SOURCE_DIR}/ext/include ${CMAKE_SOURCE_DIR/} )

  add_executable(http_client2 client.cc)
  add_executable(http_server2 server.cc)

  target_link_libraries(
    http_client2 ${CMAKE_THREAD_LIBS_INIT} opentelemetry_trace
    opentelemetry_http_client_curl opentelemetry_exporter_otlp_http 
    ${CURL_LIBRARIES})

  target_link_libraries(
    http_server2 ${CMAKE_THREAD_LIBS_INIT} opentelemetry_trace
    opentelemetry_http_client_curl opentelemetry_exporter_otlp_http)
endif()
