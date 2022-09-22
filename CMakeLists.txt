project(qcustomplot)
cmake_minimum_required(VERSION 3.14)

set(QCUSTOMPLOT_VERSION_MAJOR 2)
set(QCUSTOMPLOT_VERSION_MINOR 1)
set(QCUSTOMPLOT_VERSION_RELEASE 0)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

find_package(
  Qt5
  COMPONENTS Core Widgets PrintSupport REQUIRED
  )

set ( QCUSTOMPLOT_SOURCES
    qcustomplot.cpp
    qcustomplot.h
)

set(GLOBAL_HEADERS
    qcustomplot.h
    )

if (QCUSTOMPLOT_STATIC)
    add_library(${PROJECT_NAME} STATIC  ${QCUSTOMPLOT_SOURCES})
    target_compile_definitions( ${PROJECT_NAME} PUBLIC -DHAVE_STATIC_BUILD)
else()
    add_library(${PROJECT_NAME} SHARED  ${QCUSTOMPLOT_SOURCES})
    target_compile_definitions( ${PROJECT_NAME} PUBLIC -DQCUSTOMPLOT_COMPILE_LIBRARY)
endif()

target_compile_definitions(${PROJECT_NAME} PUBLIC -DCMAKE_CONFIG)


target_link_libraries( ${PROJECT_NAME} PUBLIC
        Qt5::Core
        Qt5::Widgets
    Qt5::PrintSupport
)

target_include_directories( ${PROJECT_NAME} PRIVATE . )

install(TARGETS
        ${PROJECT_NAME}
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
    )

install(FILES
  ${GLOBAL_HEADERS}
  DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}")
