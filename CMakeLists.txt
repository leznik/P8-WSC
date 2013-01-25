cmake_minimum_required(VERSION 2.8)
project(P8-WSC)

ADD_DEFINITIONS(-DDYNAMIND_VERSION_CORE="0.2.0")

find_package(Git REQUIRED)

set(TOOLBOX DynaMind P8-WSC-GUI DynaMind-BasicModules P8Modules DynaMind-CityBlocks)
#set(TOOLBOX DynaMind P8-WSC-GUI DynaMind-BasicModules P8Modules DynaMind-CityBlocks Dynamind-Extensions)


foreach (tool ${TOOLBOX})
	if (EXISTS ${CMAKE_SOURCE_DIR}/${tool}/src)
	    message(STATUS "${tool} submodule already exists")
	else (EXISTS ${CMAKE_SOURCE_DIR}/${tool}/src)
	    message(STATUS "preparing ${tool} submodule")
	    execute_process(COMMAND ${GIT_EXECUTABLE} submodule init
		            WORKING_DIRECTORY ${CMAKE_SOURCE_DIR})
	    execute_process(COMMAND ${GIT_EXECUTABLE} submodule update
		            WORKING_DIRECTORY ${CMAKE_SOURCE_DIR})
	endif (EXISTS ${CMAKE_SOURCE_DIR}/${tool}/src)

endforeach(${tool}) 

SET(DYNAMIND_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/DynaMind/src/core) 
SET(DYNAMIND_INCLUDE_DIR ${DYNAMIND_INCLUDE_DIR} ${CMAKE_SOURCE_DIR}/DynaMind/src/DMToolboxes) 
SET(DYNAMIND_INCLUDE_DIR ${DYNAMIND_INCLUDE_DIR} ${CMAKE_SOURCE_DIR}/DynaMind/src/python-swig) 
SET(DYNAMIND_TOOLBOX_BUILD_DIR ${P8-WSC_BINARY_DIR}/output)
#SET(DYNAMIND_EXTENSIONS_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/DynaMind-Extensions/src)
#SET(DYNAMIND_EXTENSIONS_LIBRARY dynamind-extensions)

file(MAKE_DIRECTORY ${P8-WSC_BINARY_DIR}/output)


########TMPSECTION########
OPTION(TESTNEW "Test for new dynamind kernel" OFF)

if(TESTNEW)
    SET(DYNAMIND_EXTENSIONS_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/DynaMind-Extensions/src)
    SET(DYNAMIND_EXTENSIONS_LIBRARY dynamind-extensions)
    ADD_SUBDIRECTORY(DynaMind-Extensions)
endif(TESTNEW)
########TMPSECTION########


foreach (tool ${TOOLBOX})
	add_subdirectory(${tool})
endforeach(${tool})


