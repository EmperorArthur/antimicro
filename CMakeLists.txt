# Debian Wheezy has a package for version 2.8.9 so
# that will be the minimum supported version.
cmake_minimum_required(VERSION 2.8.9)
project(AntiMicro)

if(WIN32)
    # Cause cmake to fail if Qt location is not specified.
    if(NOT CMAKE_PREFIX_PATH)
        message(FATAL_ERROR "Please set CMAKE_PREFIX_PATH to the Qt installation directory. Exiting.")
    endif(NOT CMAKE_PREFIX_PATH)

    # CMP0020: Automatically link Qt executables to qtmain target on Windows.
    cmake_policy(SET CMP0020 NEW)
endif(WIN32)

# The version number.
set(ANTIMICRO_MAJOR_VERSION 2)
set(ANTIMICRO_MINOR_VERSION 3)
set(ANTIMICRO_PATCH_VERSION 2)

option(USE_SDL_2 "Use SDL 2 libraries rather than SDL 1" ON)

# Use pkg-config to find SDL library.
if(UNIX)
    find_package(PkgConfig REQUIRED)
    include(FindPkgConfig)

    if(USE_SDL_2)
        pkg_check_modules(SDL2 REQUIRED sdl2)
    elseif(NOT USE_SDL_2)
        pkg_check_modules(SDL REQUIRED sdl)
    endif(USE_SDL_2)
endif(UNIX)

set(antimicro_SOURCES src/main.cpp
    src/mainwindow.cpp
    src/joybuttonwidget.cpp
    src/joystick.cpp
    src/joybutton.cpp
    src/event.cpp
    src/inputdaemon.cpp
    src/joyaxis.cpp
    src/joyaxiswidget.cpp
    src/joydpad.cpp
    src/joydpadbutton.cpp
    src/axiseditdialog.cpp
    src/xmlconfigreader.cpp
    src/xmlconfigwriter.cpp
    src/joytabwidget.cpp
    src/axisvaluebox.cpp
    src/advancebuttondialog.cpp
    src/simplekeygrabberbutton.cpp
    src/joybuttonslot.cpp
    src/joyaxisbutton.cpp
    src/aboutdialog.cpp
    src/setjoystick.cpp
    src/sdleventreader.cpp
    src/setaxisthrottledialog.cpp
    src/keyboard/virtualkeypushbutton.cpp
    src/keyboard/virtualkeyboardmousewidget.cpp
    src/keyboard/virtualmousepushbutton.cpp
    src/buttoneditdialog.cpp
    src/commandlineutility.cpp
    src/joycontrolstick.cpp
    src/joycontrolstickbutton.cpp
    src/joycontrolstickeditdialog.cpp
    src/joycontrolstickpushbutton.cpp
    src/joycontrolstickbuttonpushbutton.cpp
    src/joycontrolstickstatusbox.cpp
    src/advancestickassignmentdialog.cpp
    src/dpadpushbutton.cpp
    src/dpadeditdialog.cpp
    src/vdpad.cpp
    src/joydpadbuttonwidget.cpp
    src/quicksetdialog.cpp
    src/mousehelper.cpp
    src/mousesettingsdialog.cpp
    src/mousedialog/mousecontrolsticksettingsdialog.cpp
    src/mousedialog/mouseaxissettingsdialog.cpp
    src/mousedialog/mousebuttonsettingsdialog.cpp
    src/mousedialog/mousedpadsettingsdialog.cpp
    src/joytabwidgetcontainer.cpp
    src/mousedialog/springmoderegionpreview.cpp
    src/joystickstatuswindow.cpp
    src/joybuttonstatusbox.cpp
    src/qtkeymapperbase.cpp
    src/flashbuttonwidget.cpp
    src/xmlconfigmigration.cpp
    src/qkeydisplaydialog.cpp
    src/antkeymapper.cpp
    src/inputdevice.cpp
    src/mainsettingsdialog.cpp
    src/gamecontroller/gamecontrollertriggerbutton.cpp
    src/setnamesdialog.cpp
    src/autoprofilewatcher.cpp
    src/autoprofileinfo.cpp
    src/addeditautoprofiledialog.cpp
    src/editalldefaultautoprofiledialog.cpp
    src/common.cpp
    src/localantimicroserver.cpp
    src/extraprofilesettingsdialog.cpp
)

# Platform dependent files.
if(UNIX)
    LIST(APPEND antimicro_SOURCES src/x11info.cpp
        src/qtx11keymapper.cpp)
elseif(WIN32)
    LIST(APPEND antimicro_SOURCES src/wininfo.cpp
        src/qtwinkeymapper.cpp
        src/winappprofiletimerdialog.cpp)
endif(UNIX)

# Files that require SDL 2 support.
if(USE_SDL_2)
    LIST(APPEND antimicro_SOURCES src/gamecontroller/gamecontroller.cpp
            src/gamecontroller/gamecontrollerdpad.cpp
            src/gamecontroller/gamecontrollerset.cpp
            src/gamecontroller/gamecontrollertrigger.cpp
            src/gamecontrollermappingdialog.cpp
            src/gamecontrollerexample.cpp
    )
endif(USE_SDL_2)


set(antimicro_HEADERS src/mainwindow.h
    src/joybuttonwidget.h
    src/joystick.h
    src/joybutton.h
    src/inputdaemon.h
    src/joyaxis.h
    src/joyaxiswidget.h
    src/joydpad.h
    src/joydpadbutton.h
    src/axiseditdialog.h
    src/xmlconfigreader.h
    src/xmlconfigwriter.h
    src/joytabwidget.h
    src/axisvaluebox.h
    src/advancebuttondialog.h
    src/simplekeygrabberbutton.h
    src/joybuttonslot.h
    src/joyaxisbutton.h
    src/aboutdialog.h
    src/setjoystick.h
    src/sdleventreader.h
    src/setaxisthrottledialog.h
    src/keyboard/virtualkeypushbutton.h
    src/keyboard/virtualkeyboardmousewidget.h
    src/keyboard/virtualmousepushbutton.h
    src/buttoneditdialog.h
    src/commandlineutility.h
    src/joycontrolstick.h
    src/joycontrolstickbutton.h
    src/joycontrolstickeditdialog.h
    src/joycontrolstickpushbutton.h
    src/joycontrolstickbuttonpushbutton.h
    src/joycontrolstickstatusbox.h
    src/advancestickassignmentdialog.h
    src/dpadpushbutton.h
    src/dpadeditdialog.h
    src/vdpad.h
    src/joydpadbuttonwidget.h
    src/quicksetdialog.h
    src/mousehelper.h
    src/mousesettingsdialog.h
    src/mousedialog/mousecontrolsticksettingsdialog.h
    src/mousedialog/mouseaxissettingsdialog.h
    src/mousedialog/mousebuttonsettingsdialog.h
    src/mousedialog/mousedpadsettingsdialog.h
    src/joytabwidgetcontainer.h
    src/mousedialog/springmoderegionpreview.h
    src/joystickstatuswindow.h
    src/joybuttonstatusbox.h
    src/qtkeymapperbase.h
    src/flashbuttonwidget.h
    src/xmlconfigmigration.h
    src/qkeydisplaydialog.h
    src/antkeymapper.h
    src/inputdevice.h
    src/mainsettingsdialog.h
    src/gamecontroller/gamecontrollertriggerbutton.h
    src/setnamesdialog.h
    src/autoprofilewatcher.h
    src/autoprofileinfo.h
    src/addeditautoprofiledialog.h
    src/editalldefaultautoprofiledialog.h
    src/localantimicroserver.h
    src/extraprofilesettingsdialog.h
)

# Platform dependent files.
if(UNIX)
    LIST(APPEND antimicro_HEADERS src/x11info.h
    src/qtx11keymapper.h)
elseif(WIN32)
    LIST(APPEND antimicro_HEADERS src/wininfo.h
        src/qtwinkeymapper.h
        src/winappprofiletimerdialog.h)
endif(UNIX)

# Files that require SDL 2 support.
if(USE_SDL_2)
    LIST(APPEND antimicro_HEADERS src/gamecontroller/gamecontroller.h
        src/gamecontroller/gamecontrollerdpad.h
        src/gamecontroller/gamecontrollerset.h
        src/gamecontroller/gamecontrollertrigger.h
        src/gamecontrollermappingdialog.h
        src/gamecontrollerexample.h
    )
endif(USE_SDL_2)


set(antimicro_FORMS src/mainwindow.ui
    src/axiseditdialog.ui
    src/advancebuttondialog.ui
    src/aboutdialog.ui
    src/setaxisthrottledialog.ui
    src/buttoneditdialog.ui
    src/joycontrolstickeditdialog.ui
    src/advancestickassignmentdialog.ui
    src/dpadeditdialog.ui
    src/quicksetdialog.ui
    src/mousesettingsdialog.ui
    src/joystickstatuswindow.ui
    src/qkeydisplaydialog.ui
    src/gamecontrollermappingdialog.ui
    src/mainsettingsdialog.ui
    src/setnamesdialog.ui
    src/addeditautoprofiledialog.ui
    src/editalldefaultautoprofiledialog.ui
    src/winappprofiletimerdialog.ui
    src/extraprofilesettingsdialog.ui
)

set(antimicro_RESOURCES src/resources.qrc)
if(WIN32)
    # Add Windows specific resource file used for application
    # icons.
    LIST(APPEND antimicro_RESOURCES src/resources_windows.qrc)
endif(WIN32)

set(USE_QT5 OFF)
set(USE_QT4 OFF)

if(UNIX)
    # Check if Qt4 was specified using an environment variable
    # or by specifying -DQT_QMAKE_EXECUTABLE.
    # Otherwise, check for Qt libraries.
    if ("$ENV{QT_SELECT}" EQUAL 4)
        set(USE_QT4 ON)
    elseif(QT_QMAKE_EXECUTABLE MATCHES ".*/qmake-qt4")
        set(USE_QT4 ON)
    elseif("$ENV{QT_SELECT}" EQUAL 5)
        set(USE_QT5 ON)
    elseif(QT_QMAKE_EXECUTABLE MATCHES ".*/qmake-qt5")
        set(USE_QT5 ON)
    else()
        find_package(Qt5Core QUIET)
        if(Qt5Core_FOUND)
            set(USE_QT5 ON)
        else()
            find_package(Qt4 QUIET)
            if(QT_FOUND)
                set(USE_QT4 ON)
            else()
                message(FATAL_ERROR "No Qt libraries could be found.")
            endif(QT_FOUND)
        endif(Qt5Core_FOUND)
    endif("$ENV{QT_SELECT}" EQUAL 4)
elseif(WIN32)
    # Use Qt5 on Windows.
    set(USE_QT5 ON)
endif(UNIX)

if(USE_QT5)
    message("Compiling with Qt5 support")
elseif(USE_QT4)
    message("Compiling with Qt4 support")
else()
    message(FATAL_ERROR "No Qt version was specified.")
endif(USE_QT5)

if(USE_SDL_2)
    add_definitions(-DUSE_SDL_2)
endif(USE_SDL_2)

if (UNIX)
    if (USE_QT5)
        # Find includes in corresponding build directories
        #set(CMAKE_INCLUDE_CURRENT_DIR ON)
        # Instruct CMake to run moc automatically when needed.
        set(CMAKE_AUTOMOC ON)

        find_package(Qt5Widgets REQUIRED)
        find_package(Qt5Core REQUIRED)
        find_package(Qt5Gui REQUIRED)
        find_package(Qt5Network REQUIRED)
        find_package(Qt5LinguistTools REQUIRED)

        QT5_WRAP_CPP(antimicro_HEADERS_MOC ${antimicro_HEADERS})
        QT5_WRAP_UI(antimicro_FORMS_HEADERS ${antimicro_FORMS})
        QT5_ADD_RESOURCES(antimicro_RESOURCES_RCC ${antimicro_RESOURCES})
        add_subdirectory("share/antimicro/translations")

        include_directories(${Qt5Widgets_INCLUDE_DIRS})
        add_definitions(${Qt5Widgets_DEFINITIONS})
        include_directories(${Qt5Core_INCLUDE_DIRS})
        add_definitions(${Qt5Core_DEFINITIONS})
        include_directories(${Qt5Gui_INCLUDE_DIRS})
        add_definitions(${Qt5Gui_DEFINITIONS})
        include_directories(${Qt5Network_INCLUDE_DIRS})
        add_definitions(${Qt5Network_DEFINITIONS})

        set(CMAKE_POSITION_INDEPENDENT_CODE ON)

        # Add compiler flags for building executables (-fPIE)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${Qt5Widgets_EXECUTABLE_COMPILE_FLAGS}")

        LIST(APPEND LIBS ${Qt5Widgets_LIBRARIES}
            ${Qt5Core_LIBRARIES}
            ${Qt5Gui_LIBRARIES}
            ${Qt5Network_LIBRARIES}
        )
    else()
        find_package(Qt4 REQUIRED)
        set(QT_USE_QTNETWORK TRUE)

        QT4_WRAP_CPP(antimicro_HEADERS_MOC ${antimicro_HEADERS})
        QT4_WRAP_UI(antimicro_FORMS_HEADERS ${antimicro_FORMS})
        QT4_ADD_RESOURCES(antimicro_RESOURCES_RCC ${antimicro_RESOURCES})

        add_subdirectory("share/antimicro/translations")

        include(${QT_USE_FILE})
        add_definitions(${QT_DEFINITIONS})
        list(APPEND LIBS ${QT_LIBRARIES})
    endif(USE_QT5)

elseif(WIN32)
    # Find includes in corresponding build directories
    #set(CMAKE_INCLUDE_CURRENT_DIR ON)
    # Instruct CMake to run moc automatically when needed.
    set(CMAKE_AUTOMOC ON)

    find_package(Qt5Widgets REQUIRED)
    find_package(Qt5Core REQUIRED)
    find_package(Qt5Gui REQUIRED)
    find_package(Qt5Network REQUIRED)
    find_package(Qt5LinguistTools REQUIRED)

    QT5_WRAP_CPP(antimicro_HEADERS_MOC ${antimicro_HEADERS})
    QT5_WRAP_UI(antimicro_FORMS_HEADERS ${antimicro_FORMS})
    QT5_ADD_RESOURCES(antimicro_RESOURCES_RCC ${antimicro_RESOURCES})
    add_subdirectory("share/antimicro/translations")

    include_directories(${Qt5Widgets_INCLUDE_DIRS})
    add_definitions(${Qt5Widgets_DEFINITIONS})
    include_directories(${Qt5Core_INCLUDE_DIRS})
    add_definitions(${Qt5Core_DEFINITIONS})
    include_directories(${Qt5Gui_INCLUDE_DIRS})
    add_definitions(${Qt5Gui_DEFINITIONS})
    include_directories(${Qt5Network_INCLUDE_DIRS})
    add_definitions(${Qt5Network_DEFINITIONS})

    set(CMAKE_POSITION_INDEPENDENT_CODE ON)

    # Add compiler flags for building executables (-fPIE)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${Qt5Widgets_EXECUTABLE_COMPILE_FLAGS}")

    LIST(APPEND LIBS ${Qt5Widgets_LIBRARIES}
        ${Qt5Core_LIBRARIES}
        ${Qt5Gui_LIBRARIES}
        ${Qt5Network_LIBRARIES}
    )
endif(UNIX)

if(UNIX)
    LIST(APPEND LIBS X11 Xtst)
    if(USE_SDL_2)
        list(APPEND LIBS ${SDL2_LIBRARIES})
    else()
        list(APPEND LIBS ${SDL_LIBRARIES})
    endif(USE_SDL_2)
elseif (WIN32)
    if(USE_SDL_2)
        # Perform extra voodoo to get proper library paths and include
        # proper headers.
        find_library(SDL2_LIBRARY SDL2 "${PROJECT_SOURCE_DIR}/SDL2-2.0.3/i686-w64-mingw32/lib")
        list(APPEND LIBS ${SDL2_LIBRARY})
        include_directories("${PROJECT_SOURCE_DIR}/SDL2-2.0.3/i686-w64-mingw32/include")
        add_definitions(-DUNICODE -D_UNICODE)
    endif(USE_SDL_2)

    list(APPEND LIBS "psapi")
endif (UNIX)

include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories("${PROJECT_SOURCE_DIR}/src")

if(USE_QT5)
    if(UNIX)
        add_executable(antimicro ${antimicro_SOURCES}
            ${antimicro_FORMS_HEADERS}
            ${antimicro_RESOURCES_RCC}
            src/antimicro.rc
        )
    elseif(WIN32)
        # The WIN32 is required to specify a GUI application.
        add_executable(antimicro WIN32 ${antimicro_SOURCES}
            ${antimicro_FORMS_HEADERS}
            ${antimicro_RESOURCES_RCC}
            src/antimicro.rc
        )
    endif(UNIX)
else()
    add_executable(antimicro ${antimicro_SOURCES}
        ${antimicro_HEADERS_MOC}
        ${antimicro_FORMS_HEADERS}
        ${antimicro_RESOURCES_RCC}
    )
endif(USE_QT5)

# Add link libraries.
#message(${LIBS})
target_link_libraries(antimicro ${LIBS})

# Specify out directory for final executable.
if(UNIX)
	install(TARGETS antimicro RUNTIME DESTINATION "bin")
elseif(WIN32)
	install(TARGETS antimicro RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX})
endif(UNIX)

if(UNIX)
    install(FILES other/antimicro.desktop DESTINATION "share/applications")
endif(UNIX)

# uninstall target
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY)

add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)


# Only way to force install target to be dependent on updateqm.
install(CODE "execute_process(COMMAND ${CMAKE_BUILD_TOOL} updateqm WORKING_DIRECTORY \"${CMAKE_CURRENT_BINARY_DIR}\")")

# Use this to use some variables created here in the actual project.
# Modify the config.h.in file using the appropriate variables.
configure_file(
    "${PROJECT_SOURCE_DIR}/src/config.h.in"
    "${PROJECT_BINARY_DIR}/config.h"
)

if(WIN32)
    # Copy SDL2.dll to find installation directory.
    add_custom_target(copy_sdl_dll
        COMMAND ${CMAKE_COMMAND} -E copy "${PROJECT_SOURCE_DIR}/SDL2-2.0.3/i686-w64-mingw32/bin/SDL2.dll" ${CMAKE_CURRENT_BINARY_DIR}
    )

    # Obtain location of Qt5 DLL files and assign them to a list.
    # This list will only be used for Release builds.
    get_target_property(QTCORE_DLL_LOCATION Qt5::Core LOCATION)
    string(REPLACE "/Qt5Core.dll" "" QTCORE_DLL_LOCATION ${QTCORE_DLL_LOCATION})
    set(EXTRA_DLL_FILES "${QTCORE_DLL_LOCATION}/icudt51.dll"
        "${QTCORE_DLL_LOCATION}/icuin51.dll"
        "${QTCORE_DLL_LOCATION}/icuuc51.dll"
        "${QTCORE_DLL_LOCATION}/libgcc_s_dw2-1.dll"
        "${QTCORE_DLL_LOCATION}/libwinpthread-1.dll"
        "${QTCORE_DLL_LOCATION}/Qt5Core.dll"
        "${QTCORE_DLL_LOCATION}/Qt5Gui.dll"
        "${QTCORE_DLL_LOCATION}/Qt5Network.dll"
        "${QTCORE_DLL_LOCATION}/Qt5Widgets.dll"
        "${QTCORE_DLL_LOCATION}/libstdc++-6.dll"
        "${PROJECT_SOURCE_DIR}/SDL2-2.0.3/i686-w64-mingw32/bin/SDL2.dll"
    )

    # Target to copy Qt DLL files.
    add_custom_target(install_extra_dlls)
    foreach(CURRENT_DLL_FILE ${EXTRA_DLL_FILES})
        add_custom_command(TARGET install_extra_dlls PRE_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy ${CURRENT_DLL_FILE} ${CMAKE_INSTALL_PREFIX}
        )
    endforeach()

    # Target to copy qwindows.dll platforms file.
    add_custom_target(install_platforms_dll)
    add_custom_command(TARGET install_platforms_dll PRE_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy "${QTCORE_DLL_LOCATION}/../plugins/platforms/qwindows.dll"
        "${CMAKE_INSTALL_PREFIX}/platforms/qwindows.dll"
    )

    # Combine the two targets into a single target that will be used
    # while bundling the program.
    add_custom_target(install_dlls)
    add_dependencies(install_dlls
        install_extra_dlls
        install_platforms_dll
    )

    # Set variables needed for MSI building.
    set(MSIFOLDER "${PROJECT_SOURCE_DIR}/windows")
    set(WIXENV $ENV{WIX})
    set(WIXWXS "${MSIFOLDER}/AntiMicro.wxs")
    set(WIXOBJ "${MSIFOLDER}/antimicro.wixobj")
    set(WIXMSI "${MSIFOLDER}/antimicro.msi")

    if(NOT WIXENV)
        # Display message when WIX is not set up. # No extra target will be added.
        message("MSI package building not possible: WIX environment variable not defined.")
    else()
        # Target to build .msi installer file.
        add_custom_target(buildmsi)
        add_custom_command(TARGET buildmsi PRE_BUILD
            COMMAND "${WIXENV}\\bin\\candle.exe" ${WIXWXS} -out ${WIXOBJ} -sw1113 &&
                "${WIXENV}\\bin\\light.exe" ${WIXOBJ} -out ${WIXMSI} -sw1076 -spdb
        )
        set(WIXFILES ${WIXOBJ} ${WIXMSI})
        set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "${WIXFILES}")
    endif(NOT WIXENV)

    # Copy extra files to include in the final archive.
    install(FILES gpl.txt Changelog README.md DESTINATION ${CMAKE_INSTALL_PREFIX})
endif(WIN32)