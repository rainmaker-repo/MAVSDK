cmake_minimum_required(VERSION 3.13)

project(external-mavlink)
include(ExternalProject)

list(APPEND CMAKE_ARGS
    "-DCMAKE_INSTALL_PREFIX:PATH=${CMAKE_INSTALL_PREFIX}"
    "-DCMAKE_TOOLCHAIN_FILE:PATH=${CMAKE_TOOLCHAIN_FILE}"
    "-DMAVLINK_DIALECT=${MAVLINK_DIALECT}"
    )

set(MAVLINK_GIT_HASH 9d87a209f43d1c096741b9da6a86ca41c44615a6)

if(NOT DEFINED MAVLINK_GIT_REMOTE_URL)
    set(MAVLINK_GIT_REMOTE_URL "git@github.com:mavlink/mavlink.git")
endif()

if(IOS)
    ExternalProject_add(
        mavlink
        GIT_REPOSITORY ${MAVLINK_GIT_REMOTE_URL}
        GIT_TAG ${MAVLINK_GIT_HASH}
        PREFIX mavlink
        PATCH_COMMAND git checkout . && git apply ${PROJECT_SOURCE_DIR}/mavlink.patch
        CMAKE_ARGS "${CMAKE_ARGS}"
        GIT_CONFIG core.sshCommand="ssh -i /home/rainmaker/.ssh/id_github -o StrictHostKeyChecking=no"
    )
else()
    ExternalProject_add(
        mavlink
        GIT_REPOSITORY ${MAVLINK_GIT_REMOTE_URL}
        GIT_TAG ${MAVLINK_GIT_HASH}
        PREFIX mavlink
        PATCH_COMMAND git checkout . && git apply ${PROJECT_SOURCE_DIR}/mavlink.patch
        CMAKE_ARGS "${CMAKE_ARGS}"
        GIT_CONFIG core.sshCommand="ssh -i /home/rainmaker/.ssh/id_github -o StrictHostKeyChecking=no"
    )
endif()
