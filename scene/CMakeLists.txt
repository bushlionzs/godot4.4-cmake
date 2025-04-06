#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
add_definitions(-DDEBUG_ENABLED)
add_definitions(-DTOOLS_ENABLED)
#add_definitions(-DQOA_IMPLEMENTATION)

set(GODOT_SOURCE_ROOT_SCENE_DIR ${GODOT_ENGINE_ROOT_DIRECTORY}/scene)

file(GLOB  SCENE_SOURCE "${GODOT_SOURCE_ROOT_SCENE_DIR}/*.cpp" "${GODOT_SOURCE_ROOT_SCENE_DIR}/*.h")
file(GLOB  2D_SCENE_SOURCE_DIR 
    "${GODOT_SOURCE_ROOT_SCENE_DIR}/2d/*.cpp" 
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/2d/*.h"
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/2d/physics/*.cpp" 
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/2d/physics/*.h"
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/2d/physics/joints/*.cpp" 
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/2d/physics/joints/*.h"
	)
file(GLOB  3D_SCENE_SOURCE_DIR 
    "${GODOT_SOURCE_ROOT_SCENE_DIR}/3d/*.cpp" 
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/3d/*.h"
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/3d/physics/*.cpp" 
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/3d/physics/*.h"
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/3d/physics/joints/*.cpp" 
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/3d/physics/joints/*.h"
	)
file(GLOB  ANIMATION_SCENE_SOURCE_DIR "${GODOT_SOURCE_ROOT_SCENE_DIR}/animation/*.cpp" "${GODOT_SOURCE_ROOT_SCENE_DIR}/animation/*.h")
file(GLOB  AUDIO_SCENE_SOURCE_DIR "${GODOT_SOURCE_ROOT_SCENE_DIR}/audio/*.cpp" "${GODOT_SOURCE_ROOT_SCENE_DIR}/audio/*.h")
file(GLOB  DEBUGGER_SCENE_SOURCE_DIR "${GODOT_SOURCE_ROOT_SCENE_DIR}/debugger/*.cpp" "${GODOT_SOURCE_ROOT_SCENE_DIR}/debugger/*.h")
file(GLOB  GUI_SCENE_SOURCE_DIR "${GODOT_SOURCE_ROOT_SCENE_DIR}/gui/*.cpp" "${GODOT_SOURCE_ROOT_SCENE_DIR}/gui/*.h")
file(GLOB  MAIN_SCENE_SOURCE_DIR "${GODOT_SOURCE_ROOT_SCENE_DIR}/main/*.cpp" "${GODOT_SOURCE_ROOT_SCENE_DIR}/main/*.h")
file(GLOB  RESOURCES_SCENE_SOURCE_DIR
    "${GODOT_SOURCE_ROOT_SCENE_DIR}/resources/*.cpp" 
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/resources/*.h"
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/resources/2d/*.cpp" 
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/resources/2d/*.h"
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/resources/2d/skeleton/*.cpp" 
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/resources/2d/skeleton/*.h"
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/resources/3d/*.cpp" 
	"${GODOT_SOURCE_ROOT_SCENE_DIR}/resources/3d/*.h"
	)
file(GLOB  THEME_SCENE_SOURCE_DIR "${GODOT_SOURCE_ROOT_SCENE_DIR}/theme/*.cpp" "${GODOT_SOURCE_ROOT_SCENE_DIR}/theme/*.h")


list(APPEND SCENE_SOURCE ${2D_SCENE_SOURCE_DIR})
list(APPEND SCENE_SOURCE ${3D_SCENE_SOURCE_DIR})
list(APPEND SCENE_SOURCE ${ANIMATION_SCENE_SOURCE_DIR})
list(APPEND SCENE_SOURCE ${ANIMATION_SCENE_SOURCE_DIR})
list(APPEND SCENE_SOURCE ${AUDIO_SCENE_SOURCE_DIR})
list(APPEND SCENE_SOURCE ${DEBUGGER_SCENE_SOURCE_DIR})
list(APPEND SCENE_SOURCE ${GUI_SCENE_SOURCE_DIR})
list(APPEND SCENE_SOURCE ${MAIN_SCENE_SOURCE_DIR})
list(APPEND SCENE_SOURCE ${RESOURCES_SCENE_SOURCE_DIR})
list(APPEND SCENE_SOURCE ${THEME_SCENE_SOURCE_DIR})


foreach(file ${SCENE_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(godot-scene scene STATIC ${SCENE_SOURCE})
target_include_directories(godot-scene PUBLIC 
${GODOT_ENGINE_ROOT_DIRECTORY} 
${GODOT_SOURCE_ROOT_PLATFORM_DIR}/windows
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/clipper2/include
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/mbedtls/include
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/zlib
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/zstd
)
