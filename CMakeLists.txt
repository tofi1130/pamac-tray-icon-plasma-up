cmake_minimum_required(VERSION 3.5)

project(pamac-tray-plasma LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt5 COMPONENTS Core Widgets)
find_package(KF5Notifications)
find_package(PkgConfig)
add_definitions(-DQT_NO_KEYWORDS)


pkg_check_modules(DEPS REQUIRED libnotify gdk-pixbuf-2.0 pamac)


add_executable(pamac-tray-plasma
  main.cpp
  pamac_tray.cpp
  pamac_tray.h
  run_guard.cpp
  run_guard.h
#  temporary while libqpamac is tied to pamac-qt
  UpdatesChecker.cpp
  UpdatesChecker.h
  resources.qrc
)
target_include_directories(pamac-tray-plasma PUBLIC ${DEPS_INCLUDE_DIRS})


target_link_libraries(pamac-tray-plasma Qt5::Core Qt5::Widgets ${DEPS_LDFLAGS} KF5::Notifications)

configure_file(pamac-tray-plasma.desktop.in pamac-tray-plasma.desktop)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/pamac-tray-plasma.desktop DESTINATION /etc/xdg/autostart/ )
install(TARGETS pamac-tray-plasma)
