cmake_minimum_required(VERSION 4.3)
project(shapelib)
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
if(COMMAND xpExternPackage)
  xpExternPackage(TARGETS_FILE ${targetsFile}
    LIBRARIES ${lib_name} DEFAULT_TARGETS ${lib_name}
    BASE 1.2.10 XPDIFF "intro"
    WEB "http://shapelib.maptools.org/" UPSTREAM "github.com/OSGeo/shapelib"
    DESC "reading, writing, updating ESRI Shapefiles"
    LICENSE "[MIT OR LGPL-2.0-or-later](http://shapelib.maptools.org/license.html 'MIT or LGPL License')"
    )
elseif(NOT DEFINED CMAKE_INSTALL_CMAKEDIR)
  set(CMAKE_INSTALL_CMAKEDIR ${CMAKE_INSTALL_DATADIR}/cmake)
endif()
install(TARGETS ${lib_name} EXPORT ${targetsFile}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  )
install(FILES shapefil.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${lib_name})
install(EXPORT ${targetsFile} DESTINATION ${CMAKE_INSTALL_CMAKEDIR} NAMESPACE ${PROJECT_NAME}::)
