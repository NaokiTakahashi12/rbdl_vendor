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
find_package(ament_cmake_vendor_package REQUIRED)

find_package(Eigen3 REQUIRED)
find_package(RBDL QUIET)

ament_vendor(rbdl_vendor
  SATISFIED ${RBDL_FOUND}
  VCS_URL https://github.com/rbdl/rbdl.git
  VCS_VERSION v3.2.1
  CMAKE_ARGS
    -DRBDL_BUILD_ADDON_URDFREADER:BOOL=ON
    -DRBDL_BUILD_ADDON_GEOMETRY:BOOL=ON
    -DCMAKE_POSITION_INDEPENDENT_CODE:BOOL=ON
)

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
