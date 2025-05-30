
find_package (Qt${QT_DEFAULT_MAJOR_VERSION} ${REQUIRED_QT_VERSION} COMPONENTS Core Widgets Network LinguistTools REQUIRED)

message(STATUS "Using Qt ${QT_DEFAULT_MAJOR_VERSION}")

set (CMAKE_AUTOMOC ON)
set (CMAKE_AUTORCC ON)
set (CMAKE_AUTOUIC ON)
set (CMAKE_INCLUDE_CURRENT_DIR ON)

# files that are used both in tests and the app
set(GUI_COMMON_SOURCE_FILES
    src/Action.cpp
    src/Hotkey.cpp
    src/KeySequence.cpp
)

set(GUI_COMMON_HEADER_FILES
    src/Action.h
    src/Hotkey.h
    src/KeySequence.h
)

set(GUI_SOURCE_FILES
    src/AboutDialog.cpp
    src/ActionDialog.cpp
    src/AddClientDialog.cpp
    src/AppConfig.cpp
    src/AppLocale.cpp
    src/BaseConfig.cpp
    src/CommandProcess.cpp
    src/DataDownloader.cpp
    src/FingerprintAcceptDialog.cpp
    src/HotkeyDialog.cpp
    src/IpcClient.cpp
    src/Ipc.cpp
    src/IpcReader.cpp
    src/KeySequenceWidget.cpp
    src/LogWindow.cpp
    src/main.cpp
    src/MainWindow.cpp
    src/NewScreenWidget.cpp
    src/QInputLeapApplication.cpp
    src/QUtility.cpp
    src/Screen.cpp
    src/ScreenSettingsDialog.cpp
    src/ScreenSetupModel.cpp
    src/ScreenSetupView.cpp
    src/ServerConfig.cpp
    src/ServerConfigDialog.cpp
    src/SettingsDialog.cpp
    src/SetupWizard.cpp
    src/SslCertificate.cpp
    src/TrashScreenWidget.cpp
    src/ZeroconfBrowser.cpp
    src/ZeroconfRegister.cpp
    src/ZeroconfServer.cpp
    src/ZeroconfService.cpp
    src/ZeroconfThread.cpp
)

set(GUI_HEADER_FILES
    src/AboutDialog.h
    src/ActionDialog.h
    src/AddClientDialog.h
    src/AppConfig.h
    src/AppConnectionState.h
    src/AppLocale.h
    src/BaseConfig.h
    src/CommandProcess.h
    src/DataDownloader.h
    src/ElevateMode.h
    src/HotkeyDialog.h
    src/IpcClient.h
    src/Ipc.h
    src/IpcReader.h
    src/KeySequenceWidget.h
    src/LogWindow.h
    src/MainWindow.h
    src/NewScreenWidget.h
    src/ProcessorArch.h
    src/QInputLeapApplication.h
    src/QUtility.h
    src/Screen.h
    src/ScreenSettingsDialog.h
    src/ScreenSetupModel.h
    src/ScreenSetupView.h
    src/ServerConfigDialog.h
    src/ServerConfig.h
    src/SettingsDialog.h
    src/SetupWizard.h
    src/SslCertificate.h
    src/TrashScreenWidget.h
    src/ZeroconfBrowser.h
    src/ZeroconfRecord.h
    src/ZeroconfRegister.h
    src/ZeroconfServer.h
    src/ZeroconfService.h
    src/ZeroconfThread.h
)

set(GUI_UI_FILES
    src/AboutDialog.ui
    src/ActionDialog.ui
    src/AddClientDialog.ui
    src/FingerprintAcceptDialog.ui
    src/HotkeyDialog.ui
    src/LogWindow.ui
    src/MainWindow.ui
    src/ScreenSettingsDialog.ui
    src/ServerConfigDialog.ui
    src/SettingsDialog.ui
    src/SetupWizard.ui
)

set(TS_FILES
    res/lang/gui_af-ZA.ts
    res/lang/gui_ar.ts
    res/lang/gui_bg-BG.ts
    res/lang/gui_ca-AD.ts
    res/lang/gui_cs-CZ.ts
    res/lang/gui_cy.ts
    res/lang/gui_da.ts
    res/lang/gui_de.ts
    res/lang/gui_es.ts
    res/lang/gui_et-EE.ts
    res/lang/gui_fi.ts
    res/lang/gui_fr.ts
    res/lang/gui_gl.ts
    res/lang/gui_grk.ts
    res/lang/gui_he.ts
    res/lang/gui_hi.ts
    res/lang/gui_hr-HR.ts
    res/lang/gui_hu-HU.ts
    res/lang/gui_id.ts
    res/lang/gui_is-IS.ts
    res/lang/gui_it.ts
    res/lang/gui_ja-JP.ts
    res/lang/gui_ko.ts
    res/lang/gui_lt.ts
    res/lang/gui_lv.ts
    res/lang/gui_mr.ts
    res/lang/gui_nl-NL.ts
    res/lang/gui_no.ts
    res/lang/gui_pes-IR.ts
    res/lang/gui_pl-PL.ts
    res/lang/gui_pt-BR.ts
    res/lang/gui_pt-PT.ts
    res/lang/gui_ro.ts
    res/lang/gui_ru.ts
    res/lang/gui_si.ts
    res/lang/gui_sk-SK.ts
    res/lang/gui_sl-SI.ts
    res/lang/gui_sq-AL.ts
    res/lang/gui_sr.ts
    res/lang/gui_sv.ts
    res/lang/gui_th-TH.ts
    res/lang/gui_tr-TR.ts
    res/lang/gui_uk.ts
    res/lang/gui_ur.ts
    res/lang/gui_vi.ts
    res/lang/gui_zh-CN.ts
    res/lang/gui_zh-TW.ts
)

set_source_files_properties(${TS_FILES} PROPERTIES OUTPUT_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/res/lang")
if (QT_DEFAULT_MAJOR_VERSION EQUAL 5)
    qt5_add_translation(QM_FILES ${TS_FILES} OPTIONS -silent)
else()
    qt_add_translation(QM_FILES ${TS_FILES} OPTIONS -silent)
endif()

# The following file includes X11 headers that later break Qt headers.
set_source_files_properties("src/DisplayIsValid.cpp" PROPERTIES SKIP_UNITY_BUILD_INCLUSION TRUE)

if (WIN32)
    set (GUI_RC_FILES res/win/InputLeap.rc)
    set_property(SOURCE res/win/InputLeap.rc PROPERTY COMPILE_FLAGS /nologo)
endif()

add_executable (input-leap WIN32
    ${GUI_COMMON_SOURCE_FILES}
    ${GUI_COMMON_HEADER_FILES}
    ${GUI_SOURCE_FILES}
    ${GUI_HEADER_FILES}
    ${GUI_UI_FILES}
    ${GUI_RC_FILES}
    res/InputLeap.qrc
)

include_directories (./src)

target_link_libraries(input-leap
    net
    arch
    base
    io
    Qt${QT_DEFAULT_MAJOR_VERSION}::Core
    Qt${QT_DEFAULT_MAJOR_VERSION}::Widgets
    Qt${QT_DEFAULT_MAJOR_VERSION}::Network
)

target_link_libraries(input-leap OpenSSL::SSL OpenSSL::Crypto)

target_compile_definitions (input-leap PRIVATE -DINPUTLEAP_VERSION_STAGE="${INPUTLEAP_VERSION_STAGE}")
target_compile_definitions (input-leap PRIVATE -DINPUTLEAP_REVISION="${INPUTLEAP_REVISION}")

if (BUILD_MSWINDOWS)
    include_directories ($ENV{BONJOUR_SDK_HOME}/Include)
    find_library (DNSSD_LIB dnssd.lib
                  HINTS ENV BONJOUR_SDK_HOME
                  PATH_SUFFIXES "Lib/x64")
    set_target_properties (input-leap PROPERTIES LINK_FLAGS "/NODEFAULTLIB:LIBCMT")
    target_link_libraries (input-leap ${DNSSD_LIB})
endif()
if (BUILD_CARBON)
    find_library(APPSERVICES_LIB ApplicationServices)
    target_link_libraries(input-leap ${APPSERVICES_LIB})
endif()
if (BUILD_XWINDOWS OR BUILD_LIBEI)
    target_link_libraries (input-leap dns_sd Threads::Threads)
endif()

target_link_libraries (input-leap common)

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    install(TARGETS input-leap DESTINATION ${INPUTLEAP_BUNDLE_BINARY_DIR})
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Linux" OR ${CMAKE_SYSTEM_NAME} MATCHES "BSD")
    install (TARGETS input-leap DESTINATION bin)
elseif(WIN32)
    set(CMAKE_INSTALL_SYSTEM_RUNTIME_DESTINATION .)
    if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
        set(CMAKE_INSTALL_DEBUG_LIBRARIES TRUE)
    endif()
    set(CMAKE_INSTALL_UCRT_LIBRARIES TRUE)
    include(InstallRequiredSystemLibraries)
    add_custom_command(
        TARGET input-leap POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E remove_directory "${CMAKE_BINARY_DIR}/qtDeploy"
        COMMAND ${QT_DEPLOY_TOOL} --no-compiler-runtime --no-system-d3d-compiler
            --dir "${CMAKE_BINARY_DIR}/qtDeploy" $<TARGET_FILE:input-leap>
    )
    install(TARGETS input-leap DESTINATION .)
    install(
        DIRECTORY ${CMAKE_BINARY_DIR}/qtDeploy/
        DESTINATION .
        FILES_MATCHING PATTERN "*.*"
    )
endif()

if(INPUTLEAP_BUILD_TESTS)
    set(GUI_TEST_SOURCE_FILES
        test/KeySequenceTests.cpp
        test/HotkeyTests.cpp
        test/main.cpp
    )

    add_executable(guiunittests
        ${GUI_TEST_SOURCE_FILES}
        ${GUI_COMMON_SOURCE_FILES}
        ${GUI_COMMON_HEADER_FILES}
    )

    add_test(NAME guiunittests
             COMMAND guiunittests
             WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})

    target_include_directories(guiunittests PUBLIC ../../ext)
    target_link_libraries(guiunittests gtest gmock Qt${QT_DEFAULT_MAJOR_VERSION}::Core Qt${QT_DEFAULT_MAJOR_VERSION}::Widgets Qt${QT_DEFAULT_MAJOR_VERSION}::Network ${libs})
endif()
