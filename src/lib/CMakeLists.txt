cmake_minimum_required(VERSION 3.16)

project(vld-cmake-lib C CXX)

file(GLOB_RECURSE src
    ${CMAKE_CURRENT_LIST_DIR}/*.h
    ${CMAKE_CURRENT_LIST_DIR}/*.hpp

    ${CMAKE_CURRENT_LIST_DIR}/*.c
	${CMAKE_CURRENT_LIST_DIR}/*.cc
	${CMAKE_CURRENT_LIST_DIR}/*.cpp
)

file(GLOB_RECURSE public_headers
    ${CMAKE_CURRENT_LIST_DIR}/include/*.h
    ${CMAKE_CURRENT_LIST_DIR}/include/*.hpp
)

include_directories(
    ${CMAKE_CURRENT_LIST_DIR}/include
)

file(GLOB_RECURSE VLD_BINS
    ${BIN_VLD_DIR}/*
)

foreach(res IN ITEMS ${VLD_BINS})
	message(STATUS "Copying ${res} to ${CMAKE_BINARY_DIR} ...")
	get_filename_component(res_file_name ${res} NAME)
	CONFIGURE_FILE(${res} ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${res_file_name} COPYONLY)
endforeach()

file(GLOB_RECURSE VLD_LIBS
    ${LIB_VLD_DIR}/*.lib
)

foreach(res IN ITEMS ${VLD_LIBS})
	get_filename_component(res_file_name ${res} NAME)
	message(STATUS "Adding prebuilt static library: ${res_file_name} ...")
    add_library("${res_file_name}" STATIC IMPORTED)
    set_property(TARGET "${res_file_name}" PROPERTY IMPORTED_LOCATION "${res}")
endforeach()

add_library(vld-wrapper
    STATIC ${src}
)

foreach(res IN ITEMS ${VLD_LIBS})
	get_filename_component(res_file_name ${res} NAME)
    target_link_libraries(vld-wrapper PUBLIC "${res_file_name}")
endforeach()

target_include_directories(vld-wrapper
    PUBLIC "${LIB_VLD_INC}/"
    PUBLIC "${CMAKE_CURRENT_LIST_DIR}/include/"
)
