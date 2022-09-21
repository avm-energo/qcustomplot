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
  Qt${QT_VERSION_MAJOR}
  COMPONENTS Core Widgets PrintSupport REQUIRED
  )

# Old Qt does not provide QT_VERSION_MAJOR
if (NOT QT_VERSION_MAJOR)
    string(SUBSTRING ${QT_VERSION} 0 1 QT_VERSION_MAJOR)
endif()

message(STATUS "Qt version: ${QT_VERSION_MAJOR}")

set ( QCUSTOMPLOT_SOURCES
    qcustomplot.cpp
    qcustomplot.h
)

set(GLOBAL_HEADERS
    qcustomplot.h
    )

add_library(${PROJECT_NAME} STATIC ${QCUSTOMPLOT_SOURCES})

target_link_libraries( ${PROJECT_NAME} PUBLIC
	Qt${QT_VERSION_MAJOR}::Core
	Qt${QT_VERSION_MAJOR}::Widgets
    Qt${QT_VERSION_MAJOR}::PrintSupport
)

# Define target sources
file(GLOB_RECURSE files_cpp *.cpp)
file(GLOB_RECURSE files_h *.h)

target_sources(${PROJECT_NAME} PRIVATE
  ${files_cpp} ${files_h}
)

target_include_directories( ${PROJECT_NAME} PRIVATE )

install(TARGETS
	${PROJECT_NAME}
	ARCHIVE DESTINATION lib
	LIBRARY DESTINATION lib
	RUNTIME DESTINATION bin
    )

install(FILES
  ${GLOBAL_HEADERS}
  DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}")
