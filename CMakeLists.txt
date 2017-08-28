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
# version
if(DEFINED SHAPELIB_VER)
  set(ver _${SHAPELIB_VER})
  set(verDir /${PROJECT_NAME}${ver})
endif()
#######################################
# library
add_library(${lib_name} STATIC ${${lib_name}_libsrcs})
set_target_properties(${lib_name} PROPERTIES OUTPUT_NAME ${lib_name}${ver})
#######################################
# install
set(targetsFile ${PROJECT_NAME}${ver}-targets)
install(TARGETS ${lib_name} EXPORT ${targetsFile}
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  )
install(FILES shapefil.h DESTINATION include${verDir}/${lib_name})
install(EXPORT ${targetsFile} DESTINATION lib/cmake)
