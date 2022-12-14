cmake_minimum_required(VERSION 3.14)

project(qcustomplot LANGUAGES CXX)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

option(QCUSTOMPLOT_STATIC "QCustomPlot static build" ON)

find_package(
  Qt${QT_VERSION_MAJOR}
  COMPONENTS Core Widgets PrintSupport
  REQUIRED)

set(QCUSTOMPLOT_VERSION_MAJOR 2)
set(QCUSTOMPLOT_VERSION_MINOR 1)
set(QCUSTOMPLOT_VERSION_RELEASE 0)

set(QCUSTOMPLOT_SOURCES
  qcustomplot.cpp
  qcustomplot.h)

set(GLOBAL_HEADERS qcustomplot.h)

if (QCUSTOMPLOT_STATIC)
    add_library(${PROJECT_NAME} STATIC ${QCUSTOMPLOT_SOURCES})
    target_compile_definitions(${PROJECT_NAME} PUBLIC -DHAVE_STATIC_BUILD)
    target_link_libraries(${PROJECT_NAME} PRIVATE
      Qt${QT_VERSION_MAJOR}::Core
      Qt${QT_VERSION_MAJOR}::Widgets
      Qt${QT_VERSION_MAJOR}::PrintSupport)
else()
    add_library(${PROJECT_NAME} SHARED  ${QCUSTOMPLOT_SOURCES})
    target_compile_definitions(${PROJECT_NAME} PUBLIC -DQCUSTOMPLOT_COMPILE_LIBRARY)
    target_link_libraries(${PROJECT_NAME} PUBLIC
      Qt${QT_VERSION_MAJOR}::Core
      Qt${QT_VERSION_MAJOR}::Widgets
      Qt${QT_VERSION_MAJOR}::PrintSupport)
endif()
