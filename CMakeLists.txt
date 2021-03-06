cmake_minimum_required(VERSION 3.17)

project(vld-cmake)

set(CMAKE_CXX_STANDARD 17 CACHE STRING "Set to 11 or 14 to enable C++11 or C++17 builds, respectively." FORCE)
set(CMAKE_C_STANDARD 11 CACHE STRING "Set to 99 or 11 to enable C99 or C11 builds, respectively." FORCE)
set_property(GLOBAL PROPERTY CXX_STANDARD_REQUIRED ON)

get_directory_property(hasParent PARENT_DIRECTORY)
if(hasParent)
    message(STATUS "Has a parent scope.")
else()
    message(STATUS "Doesn't have a parent scope.")
	set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
	set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
	set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
endif()

message(STATUS "Operation system is ${CMAKE_SYSTEM}")

if("${CMAKE_SIZEOF_VOID_P}" EQUAL "8")
	message(STATUS "Target is 64 bits")
	set(IsX64 true)
else()
     message(STATUS "Target is 32 bits")
     set(IsX64 false)
endif()

if(WIN32)
	if(IsX64)
		message(STATUS "Now is Windows x64")
		# message(FATAL_ERROR "Windows 64-bit is not supported.")
        set(LIB_VLD_DIR "${CMAKE_CURRENT_LIST_DIR}/lib/Win64")
        set(BIN_VLD_DIR "${CMAKE_CURRENT_LIST_DIR}/bin/Win64")
	else()
		message(STATUS "Now is Windows x86")
        set(LIB_VLD_DIR "${CMAKE_CURRENT_LIST_DIR}/lib/Win32")
        set(BIN_VLD_DIR "${CMAKE_CURRENT_LIST_DIR}/bin/Win32")
	endif()
elseif(UNIX)
    message(STATUS "Unix-like OS is not supported.")
endif()

set(LIB_VLD_INC "${CMAKE_CURRENT_LIST_DIR}/include/")


if(CMAKE_BUILD_TYPE AND (CMAKE_BUILD_TYPE STREQUAL "Debug"))
    message("Debug mode:${CMAKE_C_FLAGS_DEBUG}")
	set(IsDebug true)
else()
    message("Release mode:${CMAKE_C_FLAGS_RELEASE}")
	set(IsDebug false)
endif()

add_subdirectory(src)
