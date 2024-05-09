cmake_minimum_required(VERSION 3.5)

project(yamc VERSION 1.0.0 LANGUAGES C CXX)

# Requires C++11
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

option(ENABLE_TESTING "Enable unit testing." ON)

set(LIB_NAME yamc)

add_library(${LIB_NAME} INTERFACE)
target_include_directories(${LIB_NAME} INTERFACE 
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${PROJECT_BINARY_DIR}/${LIB_NAME}ConfigVersion.cmake"
    VERSION @VERSION@
    COMPATIBILITY AnyNewerVersion
)

if (ENABLE_TESTING)
  enable_testing()
  add_subdirectory(tests)
endif()

install(TARGETS ${LIB_NAME}
    EXPORT ${LIB_NAME}Targets
    PUBLIC_HEADER DESTINATION include
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
    "${PROJECT_SOURCE_DIR}/${LIB_NAME}Config.tpl.cmake"
    "${PROJECT_BINARY_DIR}/${LIB_NAME}Config.cmake"
    INSTALL_DESTINATION "share/${LIB_NAME}"
)

install(EXPORT ${LIB_NAME}Targets DESTINATION "share/${LIB_NAME}")
install(FILES "${PROJECT_BINARY_DIR}/${LIB_NAME}ConfigVersion.cmake"
    "${PROJECT_BINARY_DIR}/${LIB_NAME}Config.cmake"
    DESTINATION "share/${LIB_NAME}")
install(DIRECTORY "${PROJECT_SOURCE_DIR}/include/" DESTINATION include
    FILES_MATCHING PATTERN "*.hpp")
