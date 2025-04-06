#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

####################################
#amd-fsr2
####################################
add_subdirectory(amd-fsr2)


####################################
#basis_universal
####################################
add_subdirectory(basis_universal)

####################################
#embree
####################################
add_subdirectory(embree)

####################################
#enet
####################################
add_subdirectory(enet)

####################################
#astcenc
####################################
add_subdirectory(astcenc)

####################################
#brotli
####################################
add_subdirectory(brotli)


####################################
#clipper2
####################################
add_subdirectory(clipper2)

####################################
#cvtt
####################################
add_subdirectory(cvtt)

####################################
#etcpak
####################################
add_subdirectory(etcpak)

####################################
#freetype
####################################
add_subdirectory(freetype-2.12.0)

####################################
#glad
####################################
add_subdirectory(glad)

####################################
#glslang
####################################
add_subdirectory(glslang)


####################################
#graphite
####################################
add_subdirectory(graphite)


####################################
#harfbuzz
####################################
add_subdirectory(harfbuzz)


####################################
#icu4c
####################################
add_subdirectory(icu4c)


####################################
#libktx
####################################
add_subdirectory(libktx)

####################################
#libogg
####################################
set(OGG_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/libogg)
set(OGG_SOURCE
    ${OGG_SOURCE_DIR}/bitwise.c
    ${OGG_SOURCE_DIR}/crctable.h
    ${OGG_SOURCE_DIR}/framing.c

    )
godot_add_library(libogg thirdparty STATIC ${OGG_SOURCE})
target_include_directories(libogg PUBLIC ${OGG_SOURCE_DIR} )
target_compile_options(libogg PRIVATE -w)

####################################
#libvorbis
####################################
add_subdirectory(libvorbis)

####################################
#libtheora
####################################
add_subdirectory(libtheora)


####################################
#libwebp
####################################
add_subdirectory(libwebp)

####################################
#mbedtls
####################################
add_subdirectory(mbedtls)


####################################
#miniupnpc
####################################
add_subdirectory(miniupnpc)



####################################
#noise
####################################
#godot_add_library(noise thirdparty INTERFACE ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/noise/FastNoiseLite.h)


####################################
#jolt_physics
####################################
add_subdirectory(jolt_physics)

####################################
#libbacktrace
####################################
#add_subdirectory(libbacktrace)

####################################
#openxr
####################################
add_subdirectory(openxr)

####################################
#pcre
####################################
add_subdirectory(pcre2)



####################################
#volk
####################################
add_subdirectory(volk)

####################################
#vulkan
####################################
add_subdirectory(vulkan)

####################################
#jpeg-compressor
####################################
add_subdirectory(jpeg-compressor)

####################################
#recastnavigation
####################################
add_subdirectory(recastnavigation)


####################################
#rvo2
####################################
add_subdirectory(rvo2)

####################################
#spirv-cross
####################################
add_subdirectory(spirv-cross)

####################################
#spirv-reflect
####################################
add_subdirectory(spirv-reflect)

####################################
#thorvg
####################################
add_subdirectory(thorvg)

####################################
#tinyexr
####################################
add_subdirectory(tinyexr)

####################################
#ufbx
####################################
add_subdirectory(ufbx)

####################################
#vhacd
####################################
add_subdirectory(vhacd)

####################################
#wslay
####################################
add_subdirectory(wslay)



####################################
#xatlas
####################################
set(XATLAS_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/xatlas)
set(XATLAS_SOURCE
    ${XATLAS_SOURCE_DIR}/xatlas.cpp
    ${XATLAS_SOURCE_DIR}/xatlas.h

    )
godot_add_library(xatlas thirdparty STATIC ${XATLAS_SOURCE})
target_include_directories(xatlas PUBLIC ${XATLAS_SOURCE_DIR})
target_compile_options(xatlas PRIVATE -w)

####################################
#meshoptimizer
####################################
add_subdirectory(meshoptimizer)

####################################
#minizip
####################################
add_subdirectory(minizip)


####################################
#zlib
####################################
add_subdirectory(zlib)

####################################
#zstd
####################################
add_subdirectory(zstd)

####################################
#libpng
####################################
add_subdirectory(libpng)

####################################
#minimp3
####################################
add_subdirectory(minimp3)

####################################
#msdfgen
####################################
add_subdirectory(msdfgen)

####################################
#misc
####################################
add_subdirectory(misc)

####################################
#manifold
####################################
add_subdirectory(manifold)