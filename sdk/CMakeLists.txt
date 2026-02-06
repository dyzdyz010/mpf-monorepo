# MPF SDK - Header-only interface library
add_library(mpf-sdk INTERFACE)
add_library(MPF::sdk ALIAS mpf-sdk)

target_include_directories(mpf-sdk INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)

target_link_libraries(mpf-sdk INTERFACE
    Qt6::Core
    Qt6::Gui
    Qt6::Qml
)
