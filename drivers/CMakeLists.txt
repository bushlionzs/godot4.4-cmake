#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

add_definitions(-DVULKAN_ENABLED)
add_definitions(-DTOOLS_ENABLED)
add_definitions(-DDEBUG_ENABLED)
add_definitions(-DUSE_VOLK)
add_definitions(-DVK_USE_PLATFORM_WIN32_KHR)
add_definitions(-DVMA_STATIC_VULKAN_FUNCTIONS=1)


set(GODOT_DRIVERS_BASE_DIR ${GODOT_ENGINE_ROOT_DIRECTORY}/drivers)

file(GLOB  GODOT_DRIVERS_SOURCE "${GODOT_DRIVERS_BASE_DIR}/*.cpp" "${GODOT_DRIVERS_BASE_DIR}/*.h")

file(GLOB  ALSA_GODOT_DRIVERS_SOURCE "${GODOT_DRIVERS_BASE_DIR}/alsa/*.cpp" "${GODOT_DRIVERS_BASE_DIR}/alsa/*.h")
file(GLOB  ALSAMIDI_GODOT_DRIVERS_SOURCE "${GODOT_DRIVERS_BASE_DIR}/alsamidi/*.cpp" "${GODOT_DRIVERS_BASE_DIR}/alsamidi/*.h")
file(GLOB  PNG_GODOT_DRIVERS_SOURCE "${GODOT_DRIVERS_BASE_DIR}/png/*.cpp" "${GODOT_DRIVERS_BASE_DIR}/png/*.h")
file(GLOB  PULSEAUDIO_GODOT_DRIVERS_SOURCE "${GODOT_DRIVERS_BASE_DIR}/pulseaudio/*.cpp" "${GODOT_DRIVERS_BASE_DIR}/pulseaudio/*.h")
file(GLOB  UNIX_GODOT_DRIVERS_SOURCE "${GODOT_DRIVERS_BASE_DIR}/unix/*.cpp" "${GODOT_DRIVERS_BASE_DIR}/unix/*.h")
file(GLOB  WINDOWS_GODOT_DRIVERS_SOURCE "${GODOT_DRIVERS_BASE_DIR}/windows/*.cpp" "${GODOT_DRIVERS_BASE_DIR}/windows/*.h")
file(GLOB  VULKAN_GODOT_DRIVERS_SOURCE "${GODOT_DRIVERS_BASE_DIR}/vulkan/*.cpp" "${GODOT_DRIVERS_BASE_DIR}/vulkan/*.h")
file(GLOB  WINMIDI_GODOT_DRIVERS_SOURCE "${GODOT_DRIVERS_BASE_DIR}/winmidi/*.cpp" "${GODOT_DRIVERS_BASE_DIR}/winmidi/*.h")
file(GLOB  WASAPI_GODOT_DRIVERS_SOURCE "${GODOT_DRIVERS_BASE_DIR}/wasapi/*.cpp" "${GODOT_DRIVERS_BASE_DIR}/wasapi/*.h")


list(APPEND GODOT_DRIVERS_SOURCE ${ALSA_GODOT_DRIVERS_SOURCE})
list(APPEND GODOT_DRIVERS_SOURCE ${ALSAMIDI_GODOT_DRIVERS_SOURCE})
list(APPEND GODOT_DRIVERS_SOURCE ${PNG_GODOT_DRIVERS_SOURCE})
list(APPEND GODOT_DRIVERS_SOURCE ${PULSEAUDIO_GODOT_DRIVERS_SOURCE})
list(APPEND GODOT_DRIVERS_SOURCE ${UNIX_GODOT_DRIVERS_SOURCE})
list(APPEND GODOT_DRIVERS_SOURCE ${WINDOWS_GODOT_DRIVERS_SOURCE})
list(APPEND GODOT_DRIVERS_SOURCE ${VULKAN_GODOT_DRIVERS_SOURCE})
list(APPEND GODOT_DRIVERS_SOURCE ${WINMIDI_GODOT_DRIVERS_SOURCE})
list(APPEND GODOT_DRIVERS_SOURCE ${WASAPI_GODOT_DRIVERS_SOURCE})

add_library(godot-driver STATIC ${GODOT_DRIVERS_SOURCE})
target_include_directories(godot-driver PUBLIC 
${GODOT_ENGINE_ROOT_DIRECTORY} 
${GODOT_SOURCE_ROOT_PLATFORM_DIR}/windows
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/vulkan/include
${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/vulkan
)

target_link_libraries(godot-driver PUBLIC
clipper2
mbedtls
zlib
zstd
libpng
volk
)
