cmake_minimum_required(VERSION 3.0)
project(shapelib)
include(flags OPTIONAL)
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
#######################################
# install
install(TARGETS ${lib_name} EXPORT ${PROJECT_NAME}-targets
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  )
install(FILES shapefil.h DESTINATION include/shape)
install(EXPORT ${PROJECT_NAME}-targets DESTINATION lib/cmake)
