set(PLATFORM_SOURCE_DIR ${GODOT_ENGINE_ROOT_DIRECTORY}/platform)
add_definitions(-DTOOLS_ENABLED)
add_definitions(-DDEBUG_ENABLED)
add_definitions(-DVULKAN_ENABLED)
add_definitions(-DRD_ENABLED)
file(GLOB  PLATFORM_SOURCE "${PLATFORM_SOURCE_DIR}/*.cpp" "${PLATFORM_SOURCE_DIR}/*.h")
file(GLOB  PLATFORM_WINDOWS_SOURCE
    "${PLATFORM_SOURCE_DIR}/windows/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/windows/*.h"
	"${PLATFORM_SOURCE_DIR}/windows/export/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/windows/export/*.h"
	)
file(GLOB  PLATFORM_ANDROID_SOURCE 
    "${PLATFORM_SOURCE_DIR}/android/api/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/android/api/*.h"
	"${PLATFORM_SOURCE_DIR}/android/export/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/android/export/*.h"
	)
file(GLOB  PLATFORM_IOS_SOURCE 
    "${PLATFORM_SOURCE_DIR}/ios/api/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/ios/api/*.h"
	"${PLATFORM_SOURCE_DIR}/ios/export/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/ios/export/*.h")
file(GLOB  PLATFORM_MACOS_SOURCE
    "${PLATFORM_SOURCE_DIR}/macos/api/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/macos/api/*.h"
	"${PLATFORM_SOURCE_DIR}/macos/export/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/macos/export/*.h"
	)
file(GLOB  PLATFORM_WEB_SOURCE
    "${PLATFORM_SOURCE_DIR}/web/api/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/web/api/*.h"
	"${PLATFORM_SOURCE_DIR}/web/export/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/web/export/*.h")
file(GLOB  PLATFORM_LINUXBSD_SOURCE 
    "${PLATFORM_SOURCE_DIR}/linuxbsd/api/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/linuxbsd/api/*.h"
	"${PLATFORM_SOURCE_DIR}/linuxbsd/export/*.cpp" 
	"${PLATFORM_SOURCE_DIR}/linuxbsd/export/*.h")

list(APPEND PLATFORM_SOURCE ${PLATFORM_WINDOWS_SOURCE})
list(APPEND PLATFORM_SOURCE ${PLATFORM_ANDROID_SOURCE})
list(APPEND PLATFORM_SOURCE ${PLATFORM_IOS_SOURCE})
list(APPEND PLATFORM_SOURCE ${PLATFORM_MACOS_SOURCE})
list(APPEND PLATFORM_SOURCE ${PLATFORM_WEB_SOURCE})
list(APPEND PLATFORM_SOURCE ${PLATFORM_LINUXBSD_SOURCE})


list(REMOVE_ITEM  PLATFORM_SOURCE "${PLATFORM_SOURCE_DIR}/windows/crash_handler_windows_signal.cpp" )

foreach(file ${PLATFORM_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

#Generate the Logo and Run icon
set(PLATFORM_WINDOWS_LOGO_SVG_SOURCE ${PLATFORM_WINDOWS_SOURCE_DIR}/logo.svg)
set(PLATFORM_WINDOWS_LOGO_SVG_OUTPUT ${CMAKE_BINARY_INCLUDE_DIR}/platform/windows/logo_svg.gen.h)
GENERATE_PLATFORM_LOGOS_AND_RUN_ICON(windows logo ${PLATFORM_WINDOWS_LOGO_SVG_SOURCE} ${PLATFORM_WINDOWS_LOGO_SVG_OUTPUT})
set(PLATFORM_WINDOWS_RUN_SVG_SOURCE ${PLATFORM_WINDOWS_SOURCE_DIR}/run_icon.svg)
set(PLATFORM_WINDOWS_RUN_SVG_OUTPUT ${CMAKE_BINARY_INCLUDE_DIR}/platform/windows/run_icon_svg.gen.h)
GENERATE_PLATFORM_LOGOS_AND_RUN_ICON(windows run_icon ${PLATFORM_WINDOWS_RUN_SVG_SOURCE} ${PLATFORM_WINDOWS_RUN_SVG_OUTPUT})

#Find the package to link and enable/disable
add_library(platforms STATIC ${PLATFORM_SOURCE} ${PLATFORM_WINDOWS_LOGO_SVG_OUTPUT} ${PLATFORM_WINDOWS_RUN_SVG_OUTPUT})
target_include_directories(platforms PRIVATE
                                             ${GODOT_ENGINE_ROOT_DIRECTORY}
                                             ${DBUS_INCLUDE_DIRS})
target_include_directories(platforms PUBLIC 
    ${PLATFORM_SOURCE_DIR}/windows)
target_link_libraries(platforms PUBLIC zlib)
# Libraries provided by Windows SDK and DirectX
target_link_libraries(platforms PUBLIC dinput8 dxguid dwrite avrt dwmapi imm32 wbemuuid)

if(${WANT_OPENGL} AND ${OPENGL_FOUND})
    target_link_libraries(platforms PUBLIC OpenGL::GL)
	target_link_libraries(platforms PUBLIC driver-gles3)
endif()

if(${WANT_VULKAN} AND ${VULKAN_FOUND})
    target_link_libraries(platforms PUBLIC vulkan glad volk)
	# We rely on Volk alone.
    #target_link_libraries (platform-windows PUBLIC ${Vulkan_LIBRARY})
endif()

