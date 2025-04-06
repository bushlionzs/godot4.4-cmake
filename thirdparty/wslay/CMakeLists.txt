#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(WSLAY_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/wslay)

file(GLOB  WSLAY_MODULES_SOURCE "${WSLAY_SOURCE_DIR}/*.c" "${WSLAY_SOURCE_DIR}/*.h")
godot_add_library(wslay thirdparty STATIC ${WSLAY_MODULES_SOURCE})
target_include_directories(wslay PUBLIC ${WSLAY_SOURCE_DIR})
