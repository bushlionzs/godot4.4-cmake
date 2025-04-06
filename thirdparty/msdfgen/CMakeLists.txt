#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(MSDFGEN_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/msdfgen)

file(GLOB  MSDFGEN_MODULES_SOURCE 
    "${MSDFGEN_SOURCE_DIR}/core/*.cpp"
	"${MSDFGEN_SOURCE_DIR}/core/*.hpp"

	)


list(REMOVE_ITEM MSDFGEN_MODULES_SOURCE "${MSDFGEN_SOURCE_DIR}/core/export-svg.cpp")
list(REMOVE_ITEM MSDFGEN_MODULES_SOURCE "${MSDFGEN_SOURCE_DIR}/core/save-bmp.cpp")
list(REMOVE_ITEM MSDFGEN_MODULES_SOURCE "${MSDFGEN_SOURCE_DIR}/core/save-fl32.cpp.cpp")
list(REMOVE_ITEM MSDFGEN_MODULES_SOURCE "${MSDFGEN_SOURCE_DIR}/core/save-rgba.cpp")
list(REMOVE_ITEM MSDFGEN_MODULES_SOURCE "${MSDFGEN_SOURCE_DIR}/core/save-tiff.cpp")

foreach(file ${MSDFGEN_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(msdfgen thirdparty STATIC ${MSDFGEN_MODULES_SOURCE})
target_include_directories(msdfgen PUBLIC ${MSDFGEN_SOURCE_DIR} ${MSDFGEN_SOURCE_DIR}/core)
#target_link_libraries(thorvg zlib libwebp png) 
