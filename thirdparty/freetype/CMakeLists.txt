#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
add_definitions(-DFT2_BUILD_LIBRARY)
add_definitions(-DFT_CONFIG_OPTION_SYSTEM_ZLIB)
add_definitions(-D_WIN32)


set(FREETYPE_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/freetype)

file(GLOB  FREETYPE_MODULES_SOURCE 
    "${FREETYPE_SOURCE_DIR}/src/autofit/autofit.c"
	"${FREETYPE_SOURCE_DIR}/src/base/ftbase.c"
	"${FREETYPE_SOURCE_DIR}/src/base/ftbbox.c"
	"${FREETYPE_SOURCE_DIR}/src/bdf/ftbdf.c"
	"${FREETYPE_SOURCE_DIR}/src/bzip2/ftbzip2.c"
	"${FREETYPE_SOURCE_DIR}/src/cache/ftcache.c"
	"${FREETYPE_SOURCE_DIR}/src/cff/cff.c"
	"${FREETYPE_SOURCE_DIR}/src/cid/ftcid.c"
	"${FREETYPE_SOURCE_DIR}/src/gxvalid/*.c"
	"${FREETYPE_SOURCE_DIR}/src/gxvalid/*.h"
	"${FREETYPE_SOURCE_DIR}/src/gzip/*.c"
	"${FREETYPE_SOURCE_DIR}/src/gzip/*.h"
	"${FREETYPE_SOURCE_DIR}/src/lzw/*.c"
	"${FREETYPE_SOURCE_DIR}/src/lzw/*.h"
	"${FREETYPE_SOURCE_DIR}/src/otvalid/*.c"
	"${FREETYPE_SOURCE_DIR}/src/otvalid/*.h"
	"${FREETYPE_SOURCE_DIR}/src/pcf/*.c"
	"${FREETYPE_SOURCE_DIR}/src/pcf/*.h"
	"${FREETYPE_SOURCE_DIR}/src/pfr/*.c"
	"${FREETYPE_SOURCE_DIR}/src/pfr/*.h"
	"${FREETYPE_SOURCE_DIR}/src/psaux/*.c"
	"${FREETYPE_SOURCE_DIR}/src/psaux/*.h"
	"${FREETYPE_SOURCE_DIR}/src/pshinter/*.c"
	"${FREETYPE_SOURCE_DIR}/src/pshinter/*.h"
	"${FREETYPE_SOURCE_DIR}/src/psnames/*.c"
	"${FREETYPE_SOURCE_DIR}/src/psnames/*.h"
	"${FREETYPE_SOURCE_DIR}/src/raster/*.c"
	"${FREETYPE_SOURCE_DIR}/src/raster/*.h"
	"${FREETYPE_SOURCE_DIR}/src/sdf/*.c"
	"${FREETYPE_SOURCE_DIR}/src/sdf/*.h"
	"${FREETYPE_SOURCE_DIR}/src/sfnt/*.c"
	"${FREETYPE_SOURCE_DIR}/src/sfnt/*.h"
	"${FREETYPE_SOURCE_DIR}/src/smooth/*.c"
	"${FREETYPE_SOURCE_DIR}/src/smooth/*.h"
	"${FREETYPE_SOURCE_DIR}/src/svg/*.c"
	"${FREETYPE_SOURCE_DIR}/src/svg/*.h"
	"${FREETYPE_SOURCE_DIR}/src/truetype/*.c"
	"${FREETYPE_SOURCE_DIR}/src/truetype/*.h"
	"${FREETYPE_SOURCE_DIR}/src/type1/*.c"
	"${FREETYPE_SOURCE_DIR}/src/type1/*.h"
	"${FREETYPE_SOURCE_DIR}/src/type42/*.c"
	"${FREETYPE_SOURCE_DIR}/src/type42/*.h"
	"${FREETYPE_SOURCE_DIR}/src/winfonts/*.c"
	"${FREETYPE_SOURCE_DIR}/src/winfonts/*.h"
	)



foreach(file ${FREETYPE_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(freetype thirdparty STATIC ${FREETYPE_MODULES_SOURCE})

target_include_directories(freetype PUBLIC 
    ${FREETYPE_SOURCE_DIR}/include
	)
target_link_libraries(freetype zlib) 
