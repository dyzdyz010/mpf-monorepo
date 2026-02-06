# MPF Foundation SDK - Header-only library
add_library(mpf-foundation-sdk INTERFACE)
add_library(MPF::foundation-sdk ALIAS mpf-foundation-sdk)

target_include_directories(mpf-foundation-sdk INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)

target_link_libraries(mpf-foundation-sdk INTERFACE
    Qt6::Core
    Qt6::Gui
    Qt6::Qml
)
