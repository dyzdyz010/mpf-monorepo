# MPF HTTP Client Library

add_library(mpf-http-client STATIC
    src/http_client.cpp
    include/mpf/http/http_client.h
    include/mpf/http/http_global.h
)

add_library(MPF::http-client ALIAS mpf-http-client)

target_include_directories(mpf-http-client PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)

target_link_libraries(mpf-http-client PUBLIC
    Qt6::Core
    Qt6::Network
)
