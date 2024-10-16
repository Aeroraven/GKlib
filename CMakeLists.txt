cmake_minimum_required(VERSION 2.8)
project(GKlib C)

option(BUILD_SHARED_LIBS "Build shared libraries (.dll/.so) instead of static ones (.lib/.a)" OFF)

# if gcc or mingw32 / mingww64, ignore maybe-uninitialized warning
if(CMAKE_COMPILER_IS_GNUCC)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-maybe-uninitialized")
endif(CMAKE_COMPILER_IS_GNUCC)
get_filename_component(abs "." ABSOLUTE)
set(GKLIB_PATH ${abs})
unset(abs)
include(GKlibSystem.cmake)

include_directories(".")
if(MSVC)
  include_directories("win32")
  file(GLOB win32_sources RELATIVE "win32" "*.c")
else(MSVC)
  set(win32_sources, "")
endif(MSVC)

# set project path
set(GKLIB_PATH ${CMAKE_BINARY_DIR}/../)

# set output dir
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${GKLIB_PATH}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${GKLIB_PATH}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${GKLIB_PATH}/bin)

add_library(GKlib ${GKlib_sources} ${win32_sources})

if(UNIX)
  target_link_libraries(GKlib m)
endif(UNIX)

include_directories("test")
#add_subdirectory("test")

install(TARGETS GKlib
  ARCHIVE DESTINATION lib/${LINSTALL_PATH}
  LIBRARY DESTINATION lib/${LINSTALL_PATH})
install(FILES ${GKlib_includes} DESTINATION include/${HINSTALL_PATH})
