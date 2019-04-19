# Copyright (c) 2019 The STE||AR-Group
#
# Distributed under the Boost Software License, Version 1.0. (See accompanying
# file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

# We require at least CMake V3.3.2
cmake_minimum_required(VERSION 3.3.2 FATAL_ERROR)

project(HPX.pp CXX)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

option(HPX_PP_WITH_TESTS "Include tests for pp" On)

include(HPX_AddDefinitions)
include(HPX_Option)

hpx_option(HPX_PP_WITH_COMPATIBILITY_HEADERS
  BOOL
  "Do not warn if old header files are being used"
  ON ADVANCED
  CATEGORY "Modules")

if(HPX_PP_WITH_COMPATIBILITY_HEADERS)
  hpx_add_config_define_namespace(DEFINE HPX_PP_HAVE_COMPATIBILITY_HEADERS NAMESPACE PP)
endif()

message(STATUS "pp: Configuring")

set(headers
  ${CMAKE_CURRENT_SOURCE_DIR}/include/hpx/pp/cat.hpp
  ${CMAKE_CURRENT_SOURCE_DIR}/include/hpx/pp/config.hpp
  ${CMAKE_CURRENT_SOURCE_DIR}/include/hpx/pp/expand.hpp
  ${CMAKE_CURRENT_SOURCE_DIR}/include/hpx/pp/nargs.hpp
  ${CMAKE_CURRENT_SOURCE_DIR}/include/hpx/pp/stringize.hpp
  ${CMAKE_CURRENT_SOURCE_DIR}/include/hpx/pp/strip_parens.hpp)

add_library(hpx.pp INTERFACE)

target_include_directories(hpx.pp INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>)

include(HPX_AddSourceGroup)
add_hpx_source_group(NAME hpx ROOT ${CMAKE_CURRENT_SOURCE_DIR}/include/hpx
  CLASS "Header Files" TARGETS ${headers})

install(TARGETS hpx.pp EXPORT HPXTargets
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  COMPONENT pp
)
hpx_export_targets(hpx.pp)

install(
  DIRECTORY include/hpx
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  COMPONENT pp)

add_subdirectory(tests)

write_config_defines_file(
  NAMESPACE PP
  FILENAME "${CMAKE_BINARY_DIR}/hpx/pp/config/defines.hpp")

write_config_defines_file(
  NAMESPACE PP
  FILENAME "${CMAKE_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/hpx/pp/config/defines.hpp")

message(STATUS "pp: Configuring done")
