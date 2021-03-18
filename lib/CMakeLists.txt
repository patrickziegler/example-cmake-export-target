# References:
# [1] https://cmake.org/cmake/help/git-stage/guide/importing-exporting/index.html
# [2] https://gitlab.kitware.com/cmake/community/-/wikis/doc/tutorials/Exporting-and-Importing-Targets

cmake_minimum_required(VERSION 3.12)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

project(CalcLib VERSION 0.1.0)

set(TARGET_NAME Calc)

set(CMAKE_FILES_INSTALL_DIR lib/cmake/calc)

add_library(
    ${TARGET_NAME} SHARED)

target_sources(
    ${TARGET_NAME} PRIVATE
    src/operations.cpp)

target_include_directories(
    ${TARGET_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

set_target_properties(
    ${TARGET_NAME} PROPERTIES
    VERSION ${CMAKE_PROJECT_VERSION}
    SOVERSION ${CMAKE_PROJECT_VERSION_MAJOR})

# install headers
install(
    DIRECTORY include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

# install library (and flag the target for export)
install(
    TARGETS ${TARGET_NAME}
    EXPORT ${TARGET_NAME}Targets)

# install target files
install(
    EXPORT ${TARGET_NAME}Targets
    DESTINATION "${CMAKE_FILES_INSTALL_DIR}")

# create config file for enabling `find_package`
configure_package_config_file(
    cmake/Config.cmake.in
    "${CMAKE_BINARY_DIR}/${TARGET_NAME}Config.cmake"
    INSTALL_DESTINATION "${CMAKE_FILES_INSTALL_DIR}")

# create version file for enabling version check with `find_package`
write_basic_package_version_file(
    "${CMAKE_BINARY_DIR}/${TARGET_NAME}ConfigVersion.cmake"
    COMPATIBILITY SameMajorVersion)

# install config and version files
install(
    FILES "${CMAKE_BINARY_DIR}/${TARGET_NAME}Config.cmake"
          "${CMAKE_BINARY_DIR}/${TARGET_NAME}ConfigVersion.cmake"
    DESTINATION "${CMAKE_FILES_INSTALL_DIR}")

# exporting targets for `include` from build tree
export(
    EXPORT ${TARGET_NAME}Targets
    FILE "${CMAKE_BINARY_DIR}/cmake/${TARGET_NAME}Targets.cmake")
