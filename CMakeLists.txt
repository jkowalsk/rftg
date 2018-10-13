cmake_minimum_required(VERSION 3.0)
project(rftg C)

set(PROJECT_NAME "rftg")

#message("CMAKE_MODULE_PATH: ${CMAKE_MODULE_PATH}")
#list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake/")
#message("CMAKE_MODULE_PATH: ${CMAKE_MODULE_PATH}")

# manage option
option(ENABLE_SERVER "enable multiplayer server" OFF)

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

set(DATA_INSTALL_DIR
    ${CMAKE_INSTALL_PREFIX}/share/${PACKAGE}/
)
set(RFTGDIR "${DATA_INSTALL_DIR}")

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

# check for gtk2
find_package(GTK2 2.16.0 REQUIRED gtk)
include_directories(${GTK2_INCLUDE_DIRS})

# sources set definitions
set(HEADERS
    client.h
    comm.h
    net.h
    rftg.h
)

set(RFTG_SRC 
    engine.c
    init.c
    ai.c
    loadsave.c
    gui.c
    net.c
    client.c
    comm.c
)

set(LEARNER_SRC
    engine.c 
    init.c 
    ai.c 
    learner.c 
    net.c 
)

set(DUMPNET_SRC 
    net.c 
    dumpnet.c
)

set(SERVER_SRC
    server.c 
    engine.c 
    init.c 
    ai.c 
    loadsave.c 
    net.c 
    comm.c
)

set(AI_CLIENT_SRC
    ai_client.c 
    engine.c 
    init.c 
    ai.c 
    net.c
    comm.c 
)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -DRFTGDIR=\\\"${RFTGDIR}\\\" ")

if(ENABLE_SERVER)
  find_package(SQLITE3 REQUIRED)

endif(ENABLE_SERVER)

add_executable(${PACKAGE} ${HEADERS} ${RFTG_SRC})
target_link_libraries(${PACKAGE} ${GTK2_LIBRARIES} -lm)

# Data file management
add_subdirectory(network)


set(DATA 
    cards.txt 
    campaign.txt 
    images.data
)

file(COPY ${DATA} DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

## for installing target
install(TARGETS ${PACKAGE}
        DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
        PERMISSIONS OWNER_READ OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
)

install(FILES do_train
        DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
        PERMISSIONS OWNER_READ OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
)

install(FILES ${DATA}
    DESTINATION ${DATA_INSTALL_DIR}
    PERMISSIONS OWNER_READ GROUP_READ WORLD_READ
)
