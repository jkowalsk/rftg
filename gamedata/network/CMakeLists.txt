cmake_minimum_required(VERSION 2.8)
project(rftg-network)

set(NETWORK_DATA_FILES
    rftg.role.1.2.net
    rftg.eval.3.2a.net
    rftg.eval.0.4.net
    rftg.role.0.2a.net
    rftg.eval.3.5.net
    rftg.role.3.4.net
    rftg.role.1.4.net
    rftg.role.1.3.net
    rftg.role.2.2a.net
    rftg.eval.0.3.net
    rftg.role.3.2a.net
    rftg.role.0.2.net
    rftg.role.2.6.net
    rftg.role.3.2.net
    rftg.role.1.2a.net
    rftg.eval.2.5.net
    rftg.role.4.4.net
    rftg.eval.1.4.net
    rftg.eval.2.6.net
    rftg.role.3.3.net
    rftg.eval.3.2.net
    rftg.role.4.5.net
    rftg.role.3.6.net
    rftg.eval.4.3.net
    rftg.eval.4.4.net
    rftg.role.4.2.net
    rftg.eval.0.2a.net
    rftg.eval.3.4.net
    rftg.role.2.5.net
    rftg.role.4.2a.net
    rftg.eval.0.2.net
    rftg.role.0.4.net
    rftg.role.3.5.net
    rftg.eval.2.3.net
    rftg.eval.1.2a.net
    rftg.role.2.4.net
    rftg.eval.2.2a.net
    rftg.eval.1.2.net
    rftg.eval.2.4.net
    rftg.eval.3.3.net
    rftg.eval.4.5.net
    rftg.role.2.3.net
    rftg.role.4.3.net
    rftg.role.0.3.net
    rftg.eval.1.3.net
    rftg.eval.4.2a.net
    rftg.eval.3.6.net
    rftg.eval.2.2.net
    rftg.role.1.5.net
    rftg.eval.1.5.net
    rftg.role.2.2.net
    rftg.eval.4.2.net
)


file(COPY ${NETWORK_DATA_FILES} DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
install(FILES ${NETWORK_DATA_FILES}
    DESTINATION ${DATA_INSTALL_DIR}/network
    PERMISSIONS OWNER_READ GROUP_READ WORLD_READ
)