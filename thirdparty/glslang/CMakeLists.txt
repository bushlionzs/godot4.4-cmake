#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(GLSLANG_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/glslang)

file(GLOB  GLSLANG_MODULES_SOURCE 
    "${GLSLANG_SOURCE_DIR}/glslang/GenericCodeGen/*.cpp"
	"${GLSLANG_SOURCE_DIR}/glslang/GenericCodeGen/*.h"
	"${GLSLANG_SOURCE_DIR}/glslang/MachineIndependent/*.cpp"
	"${GLSLANG_SOURCE_DIR}/glslang/MachineIndependent/*.h"
	"${GLSLANG_SOURCE_DIR}/glslang/MachineIndependent/preprocessor/*.cpp"
	"${GLSLANG_SOURCE_DIR}/glslang/MachineIndependent/preprocessor/*.h"
	"${GLSLANG_SOURCE_DIR}/glslang/OSDependent/Windows/*.cpp"
	"${GLSLANG_SOURCE_DIR}/glslang/OSDependent/Windows/*.h"
	"${GLSLANG_SOURCE_DIR}/glslang/ResourceLimits/*.cpp"
	"${GLSLANG_SOURCE_DIR}/glslang/ResourceLimits/*.h"
	"${GLSLANG_SOURCE_DIR}/SPIRV/*.cpp"
	"${GLSLANG_SOURCE_DIR}/SPIRV/*.h"
	)

foreach(file ${GLSLANG_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(glslang thirdparty STATIC ${GLSLANG_MODULES_SOURCE})
target_include_directories(glslang PRIVATE 
    ${GLSLANG_SOURCE_DIR}

	)
target_include_directories(glslang PUBLIC 
    ${GLSLANG_SOURCE_DIR}/glslang/include
	${GLSLANG_SOURCE_DIR}/glslang/Public
	${GLSLANG_SOURCE_DIR}
	)
#target_link_libraries(thorvg zlib libwebp png) 
