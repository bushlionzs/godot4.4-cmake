#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(JOLT_PHYSICS_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/jolt_physics)
file(GLOB JOLT_PHYSICS_SOURCE
    "${JOLT_PHYSICS_SOURCE_DIR}/Jolt/*.cpp"
    "${JOLT_PHYSICS_SOURCE_DIR}/Jolt/*.h"
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/AABBTree/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/AABBTree/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/AABBTree/NodeCodec/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/AABBTree/NodeCodec/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/AABBTree/TriangleCodec/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/AABBTree/TriangleCodec/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Core/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Core/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Geometry/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Geometry/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Math/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Math/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/ObjectStream/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/ObjectStream/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Body/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Body/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Character/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Character/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Collision/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Collision/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Collision/BroadPhase/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Collision/BroadPhase/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Collision/Shape/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Collision/Shape/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Constraints/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Constraints/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Ragdoll/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Ragdoll/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/SoftBody/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/SoftBody/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Vehicle/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Physics/Vehicle/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Renderer/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Renderer/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Skeleton/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/Skeleton/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/TriangleGrouper/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/TriangleGrouper/*.h
	${JOLT_PHYSICS_SOURCE_DIR}/Jolt/TriangleSplitter/*.cpp
    ${JOLT_PHYSICS_SOURCE_DIR}/Jolt/TriangleSplitter/*.h
    )

message("JOLT_PHYSICS_SOURCE: ${JOLT_PHYSICS_SOURCE}")
foreach(file ${JOLT_PHYSICS_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(jolt_physics thirdparty STATIC ${JOLT_PHYSICS_SOURCE})
target_include_directories(jolt_physics PUBLIC 
    ${JOLT_PHYSICS_SOURCE_DIR})
