# Rules Plugin

add_library(rules-plugin SHARED
    src/rules_plugin.cpp
    include/rules_plugin.h
    src/orders_service.cpp
    include/orders_service.h
    src/order_model.cpp
    include/order_model.h
)

target_include_directories(rules-plugin PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(rules-plugin PRIVATE
    Qt6::Core
    Qt6::Gui
    Qt6::Qml
    Qt6::Quick
    MPF::foundation-sdk
    # NOTE: ui-components is linked by host, not plugin
)

# QML files
set(PLUGIN_QML_FILES
    qml/OrdersPage.qml
    qml/OrderCard.qml
    qml/CreateOrderDialog.qml
    qml/TestCard.qml
)

foreach(file ${PLUGIN_QML_FILES})
    string(REGEX REPLACE "^qml/" "" alias "${file}")
    set_source_files_properties(${file} PROPERTIES QT_RESOURCE_ALIAS ${alias})
endforeach()

qt_add_qml_module(rules-plugin
    URI Biiz.Rules
    VERSION 1.0
    RESOURCE_PREFIX /
    QML_FILES ${PLUGIN_QML_FILES}
    OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/plugins/rules/qml/Biiz/Rules
    NO_PLUGIN
)
