#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
set(GODOT_SOURCE_ROOT_EDITOR_DIR ${GODOT_ENGINE_ROOT_DIRECTORY}/editor)

add_definitions(-DTOOLS_ENABLED)
add_definitions(-DDEBUG_ENABLED)

file(GLOB  EDITOR_SOURCE "${GODOT_SOURCE_ROOT_EDITOR_DIR}/*.cpp" "${GODOT_SOURCE_ROOT_EDITOR_DIR}/*.h")
file(GLOB  DEBUGGER_EDITOR_SOURCE 
    "${GODOT_SOURCE_ROOT_EDITOR_DIR}/debugger/*.cpp" 
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/debugger/*.h"
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/debugger/debug_adapter/*.cpp" 
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/debugger/debug_adapter/*.h"
	)
file(GLOB  EXPORT_EDITOR_SOURCE "${GODOT_SOURCE_ROOT_EDITOR_DIR}/export/*.cpp" "${GODOT_SOURCE_ROOT_EDITOR_DIR}/export/*.h")
file(GLOB  GUI_EDITOR_SOURCE "${GODOT_SOURCE_ROOT_EDITOR_DIR}/gui/*.cpp" "${GODOT_SOURCE_ROOT_EDITOR_DIR}/gui/*.h")
file(GLOB  IMPORT_EDITOR_SOURCE 
    "${GODOT_SOURCE_ROOT_EDITOR_DIR}/import/*.cpp" 
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/import/*.h"
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/import/3d/*.cpp" 
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/import/3d/*.h"
	)
file(GLOB  PLUGINS_EDITOR_SOURCE 
    "${GODOT_SOURCE_ROOT_EDITOR_DIR}/plugins/*.cpp" 
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/plugins/*.h"
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/plugins/gizmos/*.cpp" 
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/plugins/gizmos/*.h"
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/plugins/shader/*.cpp" 
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/plugins/shader/*.h"
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/plugins/tiles/*.cpp" 
	"${GODOT_SOURCE_ROOT_EDITOR_DIR}/plugins/tiles/*.h"
	)

file(GLOB  PROJECT_MANAGER_EDITOR_SOURCE "${GODOT_SOURCE_ROOT_EDITOR_DIR}/project_manager/*.cpp" "${GODOT_SOURCE_ROOT_EDITOR_DIR}/project_manager/*.h")
file(GLOB  THEMES_EDITOR_SOURCE "${GODOT_SOURCE_ROOT_EDITOR_DIR}/themes/*.cpp" "${GODOT_SOURCE_ROOT_EDITOR_DIR}/themes/*.h")


list(APPEND EDITOR_SOURCE ${DEBUGGER_EDITOR_SOURCE})
list(APPEND EDITOR_SOURCE ${EXPORT_EDITOR_SOURCE})
list(APPEND EDITOR_SOURCE ${GUI_EDITOR_SOURCE})
list(APPEND EDITOR_SOURCE ${IMPORT_EDITOR_SOURCE})
list(APPEND EDITOR_SOURCE ${PLUGINS_EDITOR_SOURCE})
list(APPEND EDITOR_SOURCE ${PROJECT_MANAGER_EDITOR_SOURCE})
list(APPEND EDITOR_SOURCE ${THEMES_EDITOR_SOURCE})

add_library(godot-editor STATIC ${EDITOR_SOURCE})

target_include_directories(godot-editor PUBLIC 
${GODOT_ENGINE_ROOT_DIRECTORY} 
${GODOT_SOURCE_ROOT_PLATFORM_DIR}/windows
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/clipper2/include
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/mbedtls/include
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/zlib
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/zstd
)


