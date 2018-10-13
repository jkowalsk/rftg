cmake_minimum_required(VERSION 2.8)
project(rftg)
add_subdirectory(network)

message("Main cmake file")

# configure part: generate config.h
set(PACKAGE "rftg")
set(VERSION "0.9.4")
set(PACKAGE_BUGREPORT "keldon@keldon.net")
set(PACKAGE_URL "")

set(PACKAGE_NAME "${PACKAGE}")
set(PACKAGE_TARNAME "${PACKAGE}")
set(PACKAGE_VERSION "${VERSION}")
set(PACKAGE_STRING  "${PACKAGE} ${VERSION}")

INCLUDE (CheckIncludeFiles)
INCLUDE (CheckLibraryExists)
INCLUDE (CheckFunctionExists)

CHECK_INCLUDE_FILES(inttypes.h HAVE_INTTYPES_H)
CHECK_LIBRARY_EXISTS(m exp "" HAVE_LIBM)
CHECK_INCLUDE_FILES(memory.h HAVE_MEMORY_H)
CHECK_FUNCTION_EXISTS(memset HAVE_MEMSET)
CHECK_INCLUDE_FILES(stdint.h HAVE_STDINT_H)
CHECK_INCLUDE_FILES(stdio.h HAVE_STDIO_H)
CHECK_INCLUDE_FILES(stdlib.h HAVE_STDLIB_H)
CHECK_INCLUDE_FILES(strings.h HAVE_STRINGS_H)
CHECK_INCLUDE_FILES(string.h HAVE_STRING_H)
CHECK_INCLUDE_FILES(sys/stat.h HAVE_SYS_STAT_H)
CHECK_INCLUDE_FILES(sys/types.h HAVE_SYS_TYPES_H)
CHECK_INCLUDE_FILES(unistd.h HAVE_UNISTD_H)
CHECK_INCLUDE_FILES("stdlib.h;stdarg.h;string.h;float.h" STDC_HEADERS)

CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/cmake-config.h.in ${CMAKE_CURRENT_BINARY_DIR}/config.h)