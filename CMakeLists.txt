set(lib_name shape)
#######################################
# preprocessor
if(MSVC)
  add_definitions(
    -D_CRT_SECURE_NO_WARNINGS
    )
endif()
#######################################
# library sources
set(${lib_name}_libsrcs
  dbfopen.c
  shpopen.c
  shptree.c
  shapefil.h
  )
#######################################
# library
add_library(${lib_name} STATIC ${${lib_name}_libsrcs})
