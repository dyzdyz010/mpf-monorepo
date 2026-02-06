# MPF HTTP Client Library

add_library(mpf-http-client SHARED
    src/http_client.cpp
    include/mpf/http/http_client.h
)

add_library(MPF::http-client ALIAS mpf-http-client)

target_include_directories(mpf-http-client PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)

target_compile_definitions(mpf-http-client PRIVATE MPF_HTTP_CLIENT_EXPORTS)

target_link_libraries(mpf-http-client PUBLIC
    Qt6::Core
    Qt6::Network
)

# Static link CRT on MinGW to avoid cross-DLL heap issues
if(MINGW)
    target_link_options(mpf-http-client PRIVATE -static-libgcc -static-libstdc++)
endif()
