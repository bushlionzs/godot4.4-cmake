#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
add_definitions(-DDEBUG_ENABLED)
add_definitions(-DTOOLS_ENABLED)

set(GODOT_SOURCE_ROOT_SERVERS_DIR ${GODOT_ENGINE_ROOT_DIRECTORY}/servers)

file(GLOB  GODOT_SERVER_SOURCE "${GODOT_SOURCE_ROOT_SERVERS_DIR}/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/*.h")
file(GLOB  GODOT_SERVER_AUDIO_SOURCE "${GODOT_SOURCE_ROOT_SERVERS_DIR}/audio/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/audio/*.h" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/audio/effects/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/audio/effects/*.h")
file(GLOB  GODOT_SERVER_CAMERA_SOURCE "${GODOT_SOURCE_ROOT_SERVERS_DIR}/camera/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/camera/*.h")
file(GLOB  GODOT_SERVER_DEBUGGER_SOURCE "${GODOT_SOURCE_ROOT_SERVERS_DIR}/debugger/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/debugger/*.h")
file(GLOB  GODOT_SERVER_DISPLAY_SOURCE "${GODOT_SOURCE_ROOT_SERVERS_DIR}/display/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/display/*.h")
file(GLOB  GODOT_SERVER_EXTENSIONS_SOURCE "${GODOT_SOURCE_ROOT_SERVERS_DIR}/extensions/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/extensions/*.h")
file(GLOB  GODOT_SERVER_MOVIE_WRITER_SOURCE "${GODOT_SOURCE_ROOT_SERVERS_DIR}/movie_writer/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/movie_writer/*.h")
file(GLOB  GODOT_SERVER_NAVIGATION_SOURCE "${GODOT_SOURCE_ROOT_SERVERS_DIR}/navigation/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/navigation/*.h")
file(GLOB  GODOT_SERVER_RENDERING_SOURCE 
    "${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/*.cpp" 
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/dummy/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/dummy/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/dummy/environment/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/dummy/environment/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/dummy/storage/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/dummy/storage/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/environment/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/environment/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/effects/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/effects/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/environment/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/environment/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/forward_clustered/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/forward_clustered/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/forward_mobile/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/forward_mobile/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/shaders/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/shaders/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/spirv-reflect/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/spirv-reflect/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/storage_rd/*.h"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/renderer_rd/storage_rd/*.cpp"
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/storage/*.cpp" 
	"${GODOT_SOURCE_ROOT_SERVERS_DIR}/rendering/storage/*.h"
	)
file(GLOB  GODOT_SERVER_TEXT_SOURCE "${GODOT_SOURCE_ROOT_SERVERS_DIR}/text/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/text/*.h")
file(GLOB  GODOT_SERVER_XR_SOURCE "${GODOT_SOURCE_ROOT_SERVERS_DIR}/xr/*.cpp" "${GODOT_SOURCE_ROOT_SERVERS_DIR}/xr/*.h")

list(APPEND GODOT_SERVER_SOURCE ${GODOT_SERVER_AUDIO_SOURCE})
list(APPEND GODOT_SERVER_SOURCE ${GODOT_SERVER_CAMERA_SOURCE})
list(APPEND GODOT_SERVER_SOURCE ${GODOT_SERVER_DEBUGGER_SOURCE})
list(APPEND GODOT_SERVER_SOURCE ${GODOT_SERVER_DISPLAY_SOURCE})
list(APPEND GODOT_SERVER_SOURCE ${GODOT_SERVER_EXTENSIONS_SOURCE})
list(APPEND GODOT_SERVER_SOURCE ${GODOT_SERVER_MOVIE_WRITER_SOURCE})
list(APPEND GODOT_SERVER_SOURCE ${GODOT_SERVER_NAVIGATION_SOURCE})
list(APPEND GODOT_SERVER_SOURCE ${GODOT_SERVER_RENDERING_SOURCE})
list(APPEND GODOT_SERVER_SOURCE ${GODOT_SERVER_TEXT_SOURCE})
list(APPEND GODOT_SERVER_SOURCE ${GODOT_SERVER_XR_SOURCE})

foreach(file ${GODOT_SERVER_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(godot-server servers STATIC ${GODOT_SERVER_SOURCE})
target_include_directories(godot-server PUBLIC 
${GODOT_ENGINE_ROOT_DIRECTORY} 
${GODOT_SOURCE_ROOT_PLATFORM_DIR}/windows
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/clipper2/include
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/mbedtls/include
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/zlib
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/zstd
)



