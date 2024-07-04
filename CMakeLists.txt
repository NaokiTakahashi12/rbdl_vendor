cmake_minimum_required(VERSION 3.8)
project(rbdl_vendor
  VERSION 0.0.0
  LANGUAGES CXX
)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)

find_package(Eigen3 REQUIRED)
find_package(RBDL QUIET)

if(NOT RBDL_FOUND)
  set(rbdl_version 3.2.1)
  set(rbdl_install_dir "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-install")
  set(extra_cmake_args)
  list(APPEND extra_cmake_args "-D CMAKE_BUILD_TYPE=Release")
  list(APPEND extra_cmake_args "-D CMAKE_INSTALL_PREFIX=${rbdl_install_dir}")
  list(APPEND extra_cmake_args "-D CMAKE_POSITION_INDEPENDENT_CODE=ON")
  list(APPEND extra_cmake_args "-D RBDL_BUILD_ADDON_URDFREADER=ON")
  list(APPEND extra_cmake_args "-D RBDL_BUILD_ADDON_GEOMETRY=ON")
  if(DEFINED CMAKE_TOOLCHAIN_FILE)
    message(FATAL_ERROR "Not support CMAKE_TOOLCHAIN_FILE: ${CMAKE_TOOLCHAIN_FILE}")
  else()
    list(APPEND extra_cmake_args "-D CMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}")
  endif()
  include(ExternalProject)
  externalproject_add(rbdl_${rbdl_version}
    GIT_REPOSITORY https://github.com/rbdl/rbdl.git
    GIT_TAG v${rbdl_version}
    CMAKE_ARGS ${extra_cmake_args}
  )
  install(
    DIRECTORY ${rbdl_install_dir}/
    DESTINATION ${CMAKE_INSTALL_PREFIX}
    USE_SOURCE_PERMISSIONS
  )
endif()

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
