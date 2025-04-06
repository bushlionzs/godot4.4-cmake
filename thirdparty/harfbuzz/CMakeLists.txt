#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
add_definitions(-DHAVE_FREETYPE)
add_definitions(-DHAVE_ICU)
set(HARFBUZZ_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/harfbuzz)

file(GLOB  HARFBUZZ_MODULES_SOURCE 
    "${HARFBUZZ_SOURCE_DIR}/src/*.cc"
	"${HARFBUZZ_SOURCE_DIR}/src/*.h"
	"${HARFBUZZ_SOURCE_DIR}/src/graph/*.cc"
	"${HARFBUZZ_SOURCE_DIR}/src/graph/*.h"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/*.cc"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/*.h"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/Color/*.cc"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/Color/*.h"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/glyf/*.cc"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/glyf/*.h"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/Layout/*.cc"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/Layout/*.h"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/name/*.cc"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/name/*.h"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/Var/*.cc"
	"${HARFBUZZ_SOURCE_DIR}/src/OT/Var/*.h"
	)

foreach(file ${HARFBUZZ_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(harfbuzz thirdparty STATIC ${HARFBUZZ_MODULES_SOURCE})
target_include_directories(harfbuzz PRIVATE 
    ${HARFBUZZ_SOURCE_DIR}

	)
target_include_directories(harfbuzz PUBLIC ${HARFBUZZ_SOURCE_DIR}/src)
target_link_libraries(harfbuzz freetype icu4c) 
