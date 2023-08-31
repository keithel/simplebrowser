cmake_minimum_required(VERSION 3.16)
project(simplebrowser LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

if(NOT DEFINED INSTALL_EXAMPLESDIR)
  set(INSTALL_EXAMPLESDIR "examples")
endif()

set(INSTALL_EXAMPLEDIR "${INSTALL_EXAMPLESDIR}/webenginewidgets/simplebrowser")

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} COMPONENTS Core)
find_package(Qt${QT_VERSION_MAJOR} COMPONENTS Gui)
find_package(Qt${QT_VERSION_MAJOR} COMPONENTS WebEngineWidgets)

set(PROJECT_SOURCES
    browser.cpp browser.h
    browserwindow.cpp browserwindow.h
    certificateerrordialog.ui
    downloadmanagerwidget.cpp downloadmanagerwidget.h downloadmanagerwidget.ui
    downloadwidget.cpp downloadwidget.h downloadwidget.ui
    main.cpp
    passworddialog.ui
    tabwidget.cpp tabwidget.h
    webpage.cpp webpage.h
    webpopupwindow.cpp webpopupwindow.h
    webview.cpp webview.h
)
if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(simplebrowser
        ${PROJECT_SOURCES}
    )
else()
    add_executable(simplebrowser
        ${PROJECT_SOURCES}
        qrc_simplebrowser.cpp
        qrc_simplebrowser1.cpp
        simplebrowser.qrc
        simplebrowser1.qrc
    )
endif()
set_target_properties(simplebrowser PROPERTIES
    WIN32_EXECUTABLE TRUE
    MACOSX_BUNDLE TRUE
)
target_link_libraries(simplebrowser PUBLIC
    Qt::Core
    Qt::Gui
    Qt::WebEngineWidgets
)


# Resources:
set(simplebrowser_resource_files
    "data/AppLogoColor.png"
    "data/ninja.png"
)

set(simplebrowser1_resource_files
    "data/3rdparty/dialog-error.png"
    "data/3rdparty/edit-clear.png"
    "data/3rdparty/go-bottom.png"
    "data/3rdparty/go-next.png"
    "data/3rdparty/go-previous.png"
    "data/3rdparty/process-stop.png"
    "data/3rdparty/text-html.png"
    "data/3rdparty/view-refresh.png"
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_resources(simplebrowser "simplebrowser"
        PREFIX
            "/"
        BASE
            "data"
        FILES
            ${simplebrowser_resource_files}
    )

    qt_add_resources(simplebrowser "simplebrowser1"
        PREFIX
            "/"
        BASE
            "data/3rdparty"
        FILES
            ${simplebrowser1_resource_files}
    )
else()
    qt_add_resources(simplebrowser "simplebrowser"
        FILES
            simplebrowser.qrc
            ${simplebrowser_resource_files}
    )

    qt_add_resources(simplebrowser "simplebrowser1"
        FILES
            simplebrowser1.qrc
            ${simplebrowser1_resource_files}
    )
endif()

install(TARGETS simplebrowser
    RUNTIME DESTINATION "${INSTALL_EXAMPLEDIR}"
    BUNDLE DESTINATION "${INSTALL_EXAMPLEDIR}"
    LIBRARY DESTINATION "${INSTALL_EXAMPLEDIR}"
)
