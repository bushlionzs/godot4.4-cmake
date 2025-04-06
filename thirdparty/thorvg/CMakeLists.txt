#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(THORVG_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/thorvg)

file(GLOB  THORVG_MODULES_SOURCE 
    "${THORVG_SOURCE_DIR}/src/common/*.cpp"
	"${THORVG_SOURCE_DIR}/src/common/*.h"
	"${THORVG_SOURCE_DIR}/src/loaders/svg/*.cpp"
	"${THORVG_SOURCE_DIR}/src/loaders/svg/*.h"
	"${THORVG_SOURCE_DIR}/src/loaders/raw/*.cpp"
	"${THORVG_SOURCE_DIR}/src/loaders/raw/*.h"
	"${THORVG_SOURCE_DIR}/src/loaders/jpg/*.cpp"
	"${THORVG_SOURCE_DIR}/src/loaders/jpg/*.h"
	"${THORVG_SOURCE_DIR}/src/loaders/external_webp/*.cpp"
	"${THORVG_SOURCE_DIR}/src/loaders/external_webp/*.h"
	"${THORVG_SOURCE_DIR}/src/loaders/external_png/*.cpp"
	"${THORVG_SOURCE_DIR}/src/loaders/external_png/*.h"
	"${THORVG_SOURCE_DIR}/src/renderer/*.cpp"
	"${THORVG_SOURCE_DIR}/src/renderer/*.h"
	"${THORVG_SOURCE_DIR}/src/renderer/sw_engine/*.cpp"
	"${THORVG_SOURCE_DIR}/src/renderer/sw_engine/*.h"
	)

foreach(file ${THORVG_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(thorvg thirdparty STATIC ${THORVG_MODULES_SOURCE})
target_include_directories(thorvg PRIVATE 
    ${THORVG_SOURCE_DIR}/src/common
	${THORVG_SOURCE_DIR}/src/renderer
	${THORVG_SOURCE_DIR}/src/renderer/sw_engine
	${THORVG_SOURCE_DIR}/src/loaders/svg
	${THORVG_SOURCE_DIR}/src/loaders/raw
	${THORVG_SOURCE_DIR}/src/loaders/jpg
	${THORVG_SOURCE_DIR}/src/loaders/external_webp
	${THORVG_SOURCE_DIR}/src/loaders/external_png
	)
target_include_directories(thorvg PUBLIC ${THORVG_SOURCE_DIR}/inc)
target_link_libraries(thorvg zlib libwebp libpng) 
