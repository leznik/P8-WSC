cmake_minimum_required(VERSION 2.8)
project(P8-WSC)

ADD_DEFINITIONS(-DDYNAMIND_VERSION_CORE="0.2.0")

find_package(Git REQUIRED)

OPTION(WITH_INSTALLER "create a windows installer (NSIS)" OFF)

#set(TOOLBOX DynaMind P8-WSC-GUI DynaMind-BasicModules P8Modules DynaMind-CityBlocks)
set(TOOLBOX DynaMind P8-WSC-GUI DynaMind-BasicModules P8Modules DynaMind-CityBlocks DynaMind-Extensions UrbanBEATS)


SET(DYNAMIND_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/DynaMind/src/core) 
SET(DYNAMIND_INCLUDE_DIR ${DYNAMIND_INCLUDE_DIR} ${CMAKE_SOURCE_DIR}/DynaMind/src/DMToolboxes) 
SET(DYNAMIND_INCLUDE_DIR ${DYNAMIND_INCLUDE_DIR} ${CMAKE_SOURCE_DIR}/DynaMind/src/python-swig) 
SET(DYNAMIND_TOOLBOX_BUILD_DIR ${P8-WSC_BINARY_DIR}/output)
SET(DYNAMIND_EXTENSIONS_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/DynaMind-Extensions/src)
SET(DYNAMIND_EXTENSIONS_LIBRARY dynamind-extensions)

file(MAKE_DIRECTORY ${P8-WSC_BINARY_DIR}/output)


foreach (tool ${TOOLBOX})
	add_subdirectory(${tool})
endforeach(${tool})


if(WITH_INSTALLER)
    INCLUDE(CPackP8.txt)
    MESSAGE(STATUS "Installer enabled")
endif(WITH_INSTALLER)
