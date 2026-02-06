# MPF UI Components - QML Component Library

set(QML_FILES
    qml/MPFCard.qml
    qml/MPFButton.qml
    qml/MPFIconButton.qml
    qml/StatusBadge.qml
    qml/MPFDialog.qml
    qml/MPFTextField.qml
    qml/MPFLoadingIndicator.qml
)

# Auto-flatten paths
foreach(file ${QML_FILES})
    string(REGEX REPLACE "^qml/" "" alias "${file}")
    set_source_files_properties(${file} PROPERTIES QT_RESOURCE_ALIAS ${alias})
endforeach()

qt_add_qml_module(mpf-ui-components
    URI MPF.Components
    VERSION 1.0
    RESOURCE_PREFIX /
    SOURCES
        src/ui_components_global.h
        src/color_helper.h
        src/color_helper.cpp
        src/input_validator.h
        src/input_validator.cpp
    QML_FILES ${QML_FILES}
    OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/qml/MPF/Components
)

add_library(MPF::ui-components ALIAS mpf-ui-components)

target_include_directories(mpf-ui-components PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
)

target_link_libraries(mpf-ui-components PUBLIC
    Qt6::Core
    Qt6::Gui
    Qt6::Qml
    Qt6::Quick
)
