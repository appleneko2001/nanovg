cmake_minimum_required(VERSION 3.19)

project(nanovg)

set(NanoVG_Sources
	src/nanovg.c
)

set(NanoVG_Sources_GLES
	src/nanovg_gl.h
	src/nanovg_gl_utils.h
)

set(NanoVG_Source_Exposer
	src/nanovg_usage.c
)
set(NanoVG_Link_GLES
	-lGLESv2
	${EXTRA_LIBS}
)

set(NanoVG_Option_Definitions)

# NanoVG OpenGLES Backend (Static)
add_library(NanoVG_GLES_Static STATIC
	${NanoVG_Sources}
	${NanoVG_Sources_GLES}
)
target_link_libraries(NanoVG_GLES_Static ${NanoVG_Link_GLES})
set_target_properties(NanoVG_GLES_Static PROPERTIES LINKER_LANGUAGE C)

option(BUILD_SHARED_STB_LIBRARIES "Enable to allow build shared library with stb_image embedded" ON)

# NanoVG with OpenGLES 3 (Shared library)
add_library(NanoVG_GLES3 SHARED
	${NanoVG_Sources}
	${NanoVG_Sources_GLES}
	${NanoVG_Source_Exposer}
)
target_link_libraries(NanoVG_GLES3 ${NanoVG_Link_GLES})

if (NOT BUILD_SHARED_STB_LIBRARIES)
	list(APPEND NanoVG_Option_Definitions -DNVG_NO_STB)
endif()

target_compile_definitions(NanoVG_GLES3 PUBLIC
	-DNANOVG_GLES3_IMPLEMENTATION=1
	-DNANOVG_GLES3=1
	${NanoVG_Option_Definitions}
)

# NanoVG with OpenGLES 2 (Shared library)
add_library(NanoVG_GLES2 SHARED
	${NanoVG_Sources}
	${NanoVG_Sources_GLES}
	${NanoVG_Source_Exposer}
)
target_link_libraries(NanoVG_GLES2 ${NanoVG_Link_GLES})

if (NOT BUILD_SHARED_STB_LIBRARIES)
	list(APPEND NanoVG_Option_Definitions -DNVG_NO_STB)
endif()

target_compile_definitions(NanoVG_GLES2 PUBLIC
	-DNANOVG_GLES2_IMPLEMENTATION=1
	-DNANOVG_GLES2=1
	${NanoVG_Option_Definitions}
)

# Examples
add_subdirectory(example)