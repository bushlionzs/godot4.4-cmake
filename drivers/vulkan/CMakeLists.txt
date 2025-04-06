cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

if(${WANT_VULKAN} AND ${VULKAN_FOUND})
    set(VULKAN_DRIVER_SOURCE_DIR ${GODOT_DRIVERS_BASE_DIR}/vulkan)
    set(VULKAN_DRIVER_SOURCE
        ${VULKAN_DRIVER_SOURCE_DIR}/rendering_device_driver_vulkan.cpp
        ${VULKAN_DRIVER_SOURCE_DIR}/rendering_device_driver_vulkan.h
        ${VULKAN_DRIVER_SOURCE_DIR}/rendering_context_driver_vulkan.cpp
        ${VULKAN_DRIVER_SOURCE_DIR}/rendering_context_driver_vulkan.h
        ${VULKAN_DRIVER_SOURCE_DIR}/vulkan_hooks.h
        )
    godot_add_library(driver-vulkan drivers STATIC ${VULKAN_DRIVER_SOURCE})
	target_include_directories(driver-vulkan PUBLIC ${VULKAN_DRIVER_SOURCE_DIR} ${GODOT_COMMON_DIR})
    target_link_libraries(driver-vulkan PUBLIC godot-error godot-os vulkan)
    target_compile_definitions(driver-vulkan PUBLIC VULKAN_ENABLED=1 USE_VOLK=1)
endif()