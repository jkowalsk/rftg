cmake_minimum_required(VERSION 3.0)
project(rftg C)

set(PROJECT_NAME "rftg")

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
    include/client.h
    include/comm.h
    include/net.h
    include/rftg.h
)

set(RFTG_SRC 
    src/engine.c
    src/init.c
    src/ai.c
    src/loadsave.c
    src/gui.c
    src/net.c
    src/client.c
    src/comm.c
)

set(LEARNER_SRC
    src/engine.c 
    src/init.c 
    src/ai.c 
    src/learner.c 
    src/net.c 
)

set(DUMPNET_SRC 
    src/net.c 
    src/dumpnet.c
)

set(SERVER_SRC
    src/server.c 
    src/engine.c 
    src/init.c 
    src/ai.c 
    src/loadsave.c 
    src/net.c 
    src/comm.c
)

set(AI_CLIENT_SRC
    src/ai_client.c 
    src/engine.c 
    src/init.c 
    src/ai.c 
    src/net.c
    src/comm.c 
)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -DRFTGDIR=\\\"${RFTGDIR}\\\" ")

if(ENABLE_SERVER)
  find_package(SQLITE3 REQUIRED)

endif(ENABLE_SERVER)

include_directories(include ${CMAKE_CURRENT_BINARY_DIR})

add_executable(${PACKAGE} ${HEADERS} ${RFTG_SRC})
target_link_libraries(${PACKAGE} ${GTK2_LIBRARIES} -lm)

# Data file management
set(DATA 
    gamedata/cards.txt 
    gamedata/campaign.txt 
    gamedata/images.data
)

set(NETWORK_DATA_FILES
    gamedata/network/rftg.role.1.2.net
    gamedata/network/rftg.eval.3.2a.net
    gamedata/network/rftg.eval.0.4.net
    gamedata/network/rftg.role.0.2a.net
    gamedata/network/rftg.eval.3.5.net
    gamedata/network/rftg.role.3.4.net
    gamedata/network/rftg.role.1.4.net
    gamedata/network/rftg.role.1.3.net
    gamedata/network/rftg.role.2.2a.net
    gamedata/network/rftg.eval.0.3.net
    gamedata/network/rftg.role.3.2a.net
    gamedata/network/rftg.role.0.2.net
    gamedata/network/rftg.role.2.6.net
    gamedata/network/rftg.role.3.2.net
    gamedata/network/rftg.role.1.2a.net
    gamedata/network/rftg.eval.2.5.net
    gamedata/network/rftg.role.4.4.net
    gamedata/network/rftg.eval.1.4.net
    gamedata/network/rftg.eval.2.6.net
    gamedata/network/rftg.role.3.3.net
    gamedata/network/rftg.eval.3.2.net
    gamedata/network/rftg.role.4.5.net
    gamedata/network/rftg.role.3.6.net
    gamedata/network/rftg.eval.4.3.net
    gamedata/network/rftg.eval.4.4.net
    gamedata/network/rftg.role.4.2.net
    gamedata/network/rftg.eval.0.2a.net
    gamedata/network/rftg.eval.3.4.net
    gamedata/network/rftg.role.2.5.net
    gamedata/network/rftg.role.4.2a.net
    gamedata/network/rftg.eval.0.2.net
    gamedata/network/rftg.role.0.4.net
    gamedata/network/rftg.role.3.5.net
    gamedata/network/rftg.eval.2.3.net
    gamedata/network/rftg.eval.1.2a.net
    gamedata/network/rftg.role.2.4.net
    gamedata/network/rftg.eval.2.2a.net
    gamedata/network/rftg.eval.1.2.net
    gamedata/network/rftg.eval.2.4.net
    gamedata/network/rftg.eval.3.3.net
    gamedata/network/rftg.eval.4.5.net
    gamedata/network/rftg.role.2.3.net
    gamedata/network/rftg.role.4.3.net
    gamedata/network/rftg.role.0.3.net
    gamedata/network/rftg.eval.1.3.net
    gamedata/network/rftg.eval.4.2a.net
    gamedata/network/rftg.eval.3.6.net
    gamedata/network/rftg.eval.2.2.net
    gamedata/network/rftg.role.1.5.net
    gamedata/network/rftg.eval.1.5.net
    gamedata/network/rftg.role.2.2.net
    gamedata/network/rftg.eval.4.2.net
)

file(COPY ${DATA} DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
file(COPY ${NETWORK_DATA_FILES} DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/network)

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

install(FILES ${NETWORK_DATA_FILES}
    DESTINATION ${DATA_INSTALL_DIR}/network
    PERMISSIONS OWNER_READ GROUP_READ WORLD_READ
)
