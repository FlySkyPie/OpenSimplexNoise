cmake_minimum_required (VERSION 3.1)

project (OpenSimplexNoise)

option(OSN_BUILD_STATIC "BUILD STATIC LIBRARIES" ON)
option(OSN_BUILD_SHARED "BUILD SHARED LIBRARIES" ON)

if(NOT OSN_BUILD_STATIC AND NOT OSN_BUILD_SHARED)
    message(SEND_ERROR "You need to build at least one flavor of OpenSimplexNoise")
endif()

if (OSN_BUILD_STATIC AND NOT OSN_BUILD_SHARED)
    add_library(${PROJECT_NAME} STATIC
        OpenSimplexNoise/OpenSimplexNoise.cpp)
endif ()
if (NOT OSN_BUILD_STATIC AND OSN_BUILD_SHARED)
    add_library(${PROJECT_NAME} SHARED
        OpenSimplexNoise/OpenSimplexNoise.cpp)
endif ()

target_include_directories(
    ${PROJECT_NAME}
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/OpenSimplexNoise>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

install(
    DIRECTORY OpenSimplexNoise/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILES_MATCHING PATTERN "*.h"
)

install(
    TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}Targets
)

install(
    EXPORT ${PROJECT_NAME}Targets
    FILE ${PROJECT_NAME}Targets.cmake
    NAMESPACE ${PROJECT_NAME}::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})
