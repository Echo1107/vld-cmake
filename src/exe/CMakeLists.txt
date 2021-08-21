project(vld-cmake-exe C CXX)

file(GLOB_RECURSE src
    ${CMAKE_CURRENT_LIST_DIR}/*.h
    ${CMAKE_CURRENT_LIST_DIR}/*.c
    ${CMAKE_CURRENT_LIST_DIR}/*.cc
    ${CMAKE_CURRENT_LIST_DIR}/*.cpp
)

add_executable(vld-cmake-exe ${src})

target_link_libraries(vld-cmake-exe
    PUBLIC vld-wrapper
)
