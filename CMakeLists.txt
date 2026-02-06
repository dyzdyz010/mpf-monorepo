cmake_minimum_required(VERSION 3.21)
project(mpf-monorepo VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_AUTOMOC ON)

# Qt policies
if(COMMAND qt_policy)
    qt_policy(SET QTP0001 NEW)
    qt_policy(SET QTP0004 NEW)
endif()

# Find Qt
find_package(Qt6 REQUIRED COMPONENTS Core Gui Qml Quick QuickControls2 Network)

# ============================================
# Build order: SDK -> Components -> Host -> Plugins
# ============================================

# 1. Foundation SDK (header-only, but we need to define targets)
add_subdirectory(sdk)

# 2. UI Components (provides MPF.Components QML module)
add_subdirectory(ui-components)

# 3. HTTP Client
add_subdirectory(http-client)

# 4. Host Application
add_subdirectory(host)

# 5. Plugins
add_subdirectory(plugins/orders)
add_subdirectory(plugins/rules)

# ============================================
# Output directories
# ============================================
# All outputs go to build/bin, build/plugins, build/qml
set_target_properties(mpf-host PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
)

set_target_properties(orders-plugin rules-plugin PROPERTIES
    LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/plugins
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/plugins
)

# ============================================
# Post-build: Copy plugin QML to output
# ============================================
add_custom_command(TARGET orders-plugin POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_BINARY_DIR}/plugins/orders/qml
        ${CMAKE_BINARY_DIR}/qml
    COMMENT "Copying Orders plugin QML"
)

add_custom_command(TARGET rules-plugin POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_BINARY_DIR}/plugins/rules/qml
        ${CMAKE_BINARY_DIR}/qml
    COMMENT "Copying Rules plugin QML"
)
