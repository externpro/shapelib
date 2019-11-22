cmake_minimum_required(VERSION 3.12)
project(shapelib)
include(flags OPTIONAL)
set(lib_name shape)
#######################################
set(${lib_name}_libsrcs
  dbfopen.c
  shpopen.c
  shptree.c
  shapefil.h
  )
#######################################
if(DEFINED SHAPELIB_VER)
  set(ver _${SHAPELIB_VER})
  set(verDir /${PROJECT_NAME}${ver})
endif()
#######################################
add_library(${lib_name} STATIC ${${lib_name}_libsrcs})
if(MSVC)
  target_compile_definitions(${lib_name} PRIVATE -D_CRT_SECURE_NO_WARNINGS)
endif()
target_include_directories(${lib_name} PUBLIC $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include${verDir}>)
set_target_properties(${lib_name} PROPERTIES OUTPUT_NAME ${lib_name}${ver})
#######################################
set(targetsFile ${PROJECT_NAME}${ver}-targets)
install(TARGETS ${lib_name} EXPORT ${targetsFile}
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  )
install(FILES shapefil.h DESTINATION include${verDir}/${lib_name})
if(DEFINED XP_NAMESPACE)
  set(nameSpace NAMESPACE ${XP_NAMESPACE}::)
endif()
install(EXPORT ${targetsFile} DESTINATION lib/cmake ${nameSpace})
