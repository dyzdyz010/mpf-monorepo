# MPF HTTP Client Library

add_library(mpf-http-client STATIC
    src/http_client.cpp
    include/mpf/http/http_client.h
)

add_library(MPF::http-client ALIAS mpf-http-client)

target_include_directories(mpf-http-client PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)

# Static library - PUBLIC so consumers also get this define
target_compile_definitions(mpf-http-client PUBLIC MPF_HTTP_CLIENT_STATIC)

target_link_libraries(mpf-http-client PUBLIC
    Qt6::Core
    Qt6::Network
)
