#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(AMD-FSR2_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/amd-fsr2)

file(GLOB  AMD-FSR2_MODULES_SOURCE 
    "${AMD-FSR2_SOURCE_DIR}/*.cpp"
	"${AMD-FSR2_SOURCE_DIR}/*.h"
	)

foreach(file ${AMD-FSR2_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(amd-fsr2 thirdparty STATIC ${AMD-FSR2_MODULES_SOURCE})

target_include_directories(amd-fsr2 PUBLIC 
    ${AMD-FSR2_SOURCE_DIR}
	)
#target_link_libraries(etcpak zlib) 
