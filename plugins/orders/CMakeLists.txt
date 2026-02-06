# Orders Plugin

add_library(orders-plugin SHARED
    src/orders_plugin.cpp
    include/orders_plugin.h
    src/orders_service.cpp
    include/orders_service.h
    src/order_model.cpp
    include/order_model.h
)

target_include_directories(orders-plugin PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(orders-plugin PRIVATE
    Qt6::Core
    Qt6::Gui
    Qt6::Qml
    Qt6::Quick
    Qt6::Network
    MPF::sdk
    MPF::http-client
    # NOTE: ui-components is linked by host, not plugin
)

# Static link CRT on MinGW to avoid cross-DLL heap issues
if(MINGW)
    target_link_options(orders-plugin PRIVATE -static-libgcc -static-libstdc++)
endif()

# QML files
set(PLUGIN_QML_FILES
    qml/OrdersPage.qml
    qml/OrderCard.qml
    qml/CreateOrderDialog.qml
)

foreach(file ${PLUGIN_QML_FILES})
    string(REGEX REPLACE "^qml/" "" alias "${file}")
    set_source_files_properties(${file} PROPERTIES QT_RESOURCE_ALIAS ${alias})
endforeach()

qt_add_qml_module(orders-plugin
    URI YourCo.Orders
    VERSION 1.0
    RESOURCE_PREFIX /
    QML_FILES ${PLUGIN_QML_FILES}
    OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/qml/YourCo/Orders
    NO_PLUGIN
)
