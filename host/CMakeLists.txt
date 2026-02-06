# MPF Host Application

add_executable(mpf-host
    # Main
    src/main.cpp
    src/application.cpp
    
    # Core
    src/service_registry.cpp
    src/logger.cpp
    src/plugin_metadata.cpp
    
    # Services
    src/plugin_manager.cpp
    src/plugin_loader.cpp
    src/navigation_service.cpp
    src/settings_service.cpp
    src/theme_service.cpp
    src/menu_service.cpp
    src/event_bus_service.cpp
    src/qml_context.cpp
    
    # Headers
    include/application.h
    include/cross_dll_safety.h
    include/service_registry.h
    include/logger.h
    include/plugin_metadata.h
    include/plugin_manager.h
    include/plugin_loader.h
    include/navigation_service.h
    include/settings_service.h
    include/theme_service.h
    include/menu_service.h
    include/event_bus_service.h
    include/qml_context.h
)

target_include_directories(mpf-host PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(mpf-host PRIVATE
    Qt6::Core
    Qt6::Gui
    Qt6::Qml
    Qt6::Quick
    Qt6::QuickControls2
    MPF::sdk
    MPF::ui-components
)

# Static link CRT on MinGW to avoid cross-DLL heap issues
if(MINGW)
    target_link_options(mpf-host PRIVATE -static-libgcc -static-libstdc++)
endif()

# QML files
set(HOST_QML_FILES
    qml/Main.qml
    qml/SideMenu.qml
    qml/MenuItemCustom.qml
    qml/ErrorDialog.qml
)

set(HOST_RESOURCES
    qml/images/logo.svg
)

foreach(file ${HOST_QML_FILES} ${HOST_RESOURCES})
    string(REGEX REPLACE "^qml/" "" alias "${file}")
    set_source_files_properties(${file} PROPERTIES QT_RESOURCE_ALIAS ${alias})
endforeach()

qt_add_qml_module(mpf-host
    URI MPF.Host
    VERSION 1.0
    RESOURCE_PREFIX /
    QML_FILES ${HOST_QML_FILES}
    RESOURCES ${HOST_RESOURCES}
    OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/qml/MPF/Host
)

# Generate simplified sdk_paths.h for monorepo build
file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/include/mpf/sdk_paths.h
"#pragma once
// Monorepo build - SDK paths are not used
#define MPF_SDK_HAS_QML_PATH 0
#define MPF_PREFIX \"\"
#define MPF_QML_PATH \"\"
")

target_include_directories(mpf-host PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}/include
)

# Generate version header
file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/include/mpf/version.h
"#pragma once
#define MPF_VERSION_MAJOR 1
#define MPF_VERSION_MINOR 0
#define MPF_VERSION_PATCH 0
#define MPF_VERSION_STRING \"1.0.0-monorepo\"
")
