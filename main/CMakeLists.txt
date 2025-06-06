#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

add_definitions(-DTOOLS_ENABLED)
add_definitions(-DDEBUG_ENABLED)
#add_definitions(-DSTEAMAPI_ENABLED)
set(GODOT_MAIN_BASE_DIR ${GODOT_ENGINE_ROOT_DIRECTORY}/main)

#####################################
#Main Executables
####################################
set(MAIN_EXECUTABLE_SOURCE
	${GODOT_MAIN_BASE_DIR}/main.h
	${GODOT_MAIN_BASE_DIR}/main_timer_sync.h
	${GODOT_MAIN_BASE_DIR}/performance.h
	${GODOT_MAIN_BASE_DIR}/main.cpp
	${GODOT_MAIN_BASE_DIR}/main_timer_sync.cpp
	${GODOT_MAIN_BASE_DIR}/performance.cpp
	${GODOT_MAIN_BASE_DIR}/steam_tracker.cpp
	)

set(GODOT_PLATFORM_GENERATED_REGISTERED_APIS_OUTPUT ${GODOT_ENGINE_ROOT_DIRECTORY}/platform/register_platform_apis.gen.cpp)
GENERATE_GODOT_REGISTER_PLATFORM_APIS("${GODOT_PLATFORM_GENERATED_REGISTERED_APIS_OUTPUT}")

set(GODOT_MAIN_EXECUTABLE_APP_ICON_SOURCE ${GODOT_MAIN_BASE_DIR}/app_icon.png)
set(GODOT_MAIN_EXECUTABLE_APP_ICON_OUTPUT ${GODOT_ENGINE_ROOT_DIRECTORY}/main/app_icon.gen.h)
GENERATE_MAIN_EXECUTABLE_SOURCES(make_app_icon ${GODOT_MAIN_EXECUTABLE_APP_ICON_SOURCE} ${GODOT_MAIN_EXECUTABLE_APP_ICON_OUTPUT})

set(GODOT_MAIN_EXECUTABLE_APP_SPLASH_SOURCE ${GODOT_MAIN_BASE_DIR}/splash.png)
set(GODOT_MAIN_EXECUTABLE_APP_SPLASH_OUTPUT ${GODOT_ENGINE_ROOT_DIRECTORY}/main/splash.gen.h)
GENERATE_MAIN_EXECUTABLE_SOURCES(make_app_splash ${GODOT_MAIN_EXECUTABLE_APP_SPLASH_SOURCE} ${GODOT_MAIN_EXECUTABLE_APP_SPLASH_OUTPUT})

add_executable(GodotEditor ${MAIN_EXECUTABLE_SOURCE}
 							${GODOT_MAIN_EXECUTABLE_APP_ICON_OUTPUT}
 							${GODOT_MAIN_EXECUTABLE_APP_SPLASH_OUTPUT}
 							${GODOT_PLATFORM_GENERATED_REGISTERED_APIS_OUTPUT})
target_link_libraries(GodotEditor PUBLIC
godot-driver
godot-editor
godot-modules
platforms
godot-scene
godot-server
godot-core
freetype
pcre2
basis_universal
embree
enet
astcenc
brotli
clipper2
cvtt
etcpak
glad
glslang
graphite
harfbuzz
icu4c
libktx
libogg
libtheora
libvorbis
libwebp
mbedtls
miniupnpc
jolt_physics
openxr
volk
vulkan
jpeg-compressor
recastnavigation
rvo2
spirv-cross
spirv-reflect
thorvg
tinyexr
ufbx
vhacd
wslay
xatlas
meshoptimizer
minizip
zlib
zstd
libpng
misc
manifold
amd-fsr2
Ws2_32
winmm
crypt32
Bcrypt
dbghelp
Shlwapi.lib
ntdll.lib
)
target_include_directories(GodotEditor PUBLIC ${GODOT_ENGINE_ROOT_DIRECTORY})
target_compile_definitions(GodotEditor PUBLIC NO_EDITOR_SPLASH=1)

target_include_directories(GodotEditor PUBLIC
									   ${GODOT_ENGINE_ROOT_DIRECTORY}/modules
									   ${GODOT_ENGINE_ROOT_DIRECTORY}/editor
									   ${GODOT_ENGINE_ROOT_DIRECTORY}/platform
									   )
