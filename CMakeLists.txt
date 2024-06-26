cmake_minimum_required(VERSION 3.28)

SET(PROJECT_NAME "demo")
SET(PROJECT_NAME_UPPER "")
SET(PROJECT_NAME_LOWER "")

string(TOUPPER ${PROJECT_NAME} PROJECT_NAME_UPPER)
string(TOLOWER ${PROJECT_NAME} PROJECT_NAME_LOWER)

project(${PROJECT_NAME})

message(STATUS "Start generation of project with name: ${PROJECT_NAME}")

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    add_definitions(-D${PROJECT_NAME_UPPER}_PLATFORM_X64)
    message(STATUS "64 bit platform - adding definition: ${PROJECT_NAME_UPPER}_PLATFORM_X64")
elseif(CMAKE_SIZEOF_VOID_P EQUAL 4)
    add_definitions(-D${PROJECT_NAME_UPPER}_PLATFORM_X86)
    message(STATUS "32 bit platform - adding definition: ${PROJECT_NAME_UPPER}_PLATFORM_X86")
else()
	message( FATAL_ERROR "Unknown CPU architecture, CMAKE will exit." )
endif()

if(MSVC)
	# Remove the always defined _WINDOWS flag
	# CPP
	string (REPLACE "/D_WINDOWS" "" CMAKE_CUSTOM_CPP_FLAGS ${CMAKE_CXX_FLAGS})
	string (REPLACE "/DWIN32" "" CMAKE_CUSTOM_CPP_FLAGS ${CMAKE_CUSTOM_CPP_FLAGS}) 
	SET(CMAKE_CXX_FLAGS "${CMAKE_CUSTOM_CPP_FLAGS}" CACHE STRING "" FORCE)
	# C
	string (REPLACE "/D_WINDOWS" "" CMAKE_CUSTOM_C_FLAGS ${CMAKE_C_FLAGS})
	string (REPLACE "/DWIN32" "" CMAKE_CUSTOM_C_FLAGS ${CMAKE_CUSTOM_C_FLAGS})
	SET(CMAKE_C_FLAGS "${CMAKE_CUSTOM_C_FLAGS}" CACHE STRING "" FORCE)
endif()

# Use C++ 17 as a standard
SET(CMAKE_CXX_STANDARD 17)
# Debug postfix
SET(CMAKE_DEBUG_POSTFIX _d)
# Enable folder structure in the IDE
SET_PROPERTY(GLOBAL PROPERTY USE_FOLDERS ON)

add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/source/${PROJECT_NAME_LOWER})