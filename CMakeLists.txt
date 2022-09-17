cmake_minimum_required(VERSION 3.12)
project(shapelib)
include(GNUInstallDirs)
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
add_library(${lib_name} STATIC ${${lib_name}_libsrcs})
if(MSVC)
  target_compile_definitions(${lib_name} PRIVATE -D_CRT_SECURE_NO_WARNINGS)
endif()
target_include_directories(${lib_name} PUBLIC $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
#######################################
set(targetsFile ${PROJECT_NAME}-targets)
install(TARGETS ${lib_name} EXPORT ${targetsFile}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  )
install(FILES shapefil.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${lib_name})
if(DEFINED XP_NAMESPACE)
  set(nameSpace NAMESPACE ${XP_NAMESPACE}::)
endif()
if(NOT DEFINED XP_INSTALL_CMAKEDIR)
  set(XP_INSTALL_CMAKEDIR ${CMAKE_INSTALL_DATADIR}/cmake)
endif()
install(EXPORT ${targetsFile} DESTINATION ${XP_INSTALL_CMAKEDIR} ${nameSpace})
