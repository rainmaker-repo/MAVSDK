cmake_minimum_required(VERSION 3.13)
project(external-mavlink)
include(ExternalProject)

# Set the specific Git commit hash
set(MAVLINK_GIT_HASH b4add5292992cc43bd471b431b425338787982be)

# Set the path to the SSH key placed by Ansible
set(SSH_KEY_PATH "/home/rainmaker/.ssh/id_github")

# Set the GIT_SSH_COMMAND to use the specific key
set(GIT_SSH_COMMAND "ssh -i ${SSH_KEY_PATH} -o StrictHostKeyChecking=no")

ExternalProject_add(
    mavlink
    GIT_REPOSITORY git@github.com:rainmaker-repo/rm-mavlink-quad.git
    GIT_TAG ${MAVLINK_GIT_HASH}
    PREFIX mavlink
    PATCH_COMMAND git checkout . && git apply ${PROJECT_SOURCE_DIR}/mavlink.patch
    CMAKE_ARGS "${CMAKE_ARGS}"
    GIT_CONFIG "core.sshCommand=${GIT_SSH_COMMAND}"
)
