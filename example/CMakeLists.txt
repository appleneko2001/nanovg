link_libraries(NanoVG_GLES_Static)

find_package(GLEW)

find_package(glfw3)

link_libraries(GLEW::GLEW glfw)

add_definitions(-DNANOVG_GLEW)

include_directories(
        "${PROJECT_SOURCE_DIR}/src")

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR})

add_executable(example_gles3 WIN32 example_gles3.c demo.c perf.c)
target_compile_definitions(example_gles3 PUBLIC NANOVG_GLES3_IMPLEMENTATION=1)

add_executable(example_gles2 WIN32 example_gles2.c demo.c perf.c)
target_compile_definitions(example_gles2 PUBLIC NANOVG_GLES2_IMPLEMENTATION=1)
