cmake_minimum_required(VERSION 2.8.3)
project(mola_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple(ALL DEPS REQUIRED)

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(mola_src
  GIT_REPOSITORY git@github.com:zarmomin/mola.git
  GIT_TAG master
  UPDATE_COMMAND ""
  PATCH_COMMAND patch -p1 < ${CMAKE_CURRENT_SOURCE_DIR}/patches/mola-base.patch &&
  patch -p1 < ${CMAKE_CURRENT_SOURCE_DIR}/patches/mola-fe-lidar.patch &&
  patch -p1 < ${CMAKE_CURRENT_SOURCE_DIR}/patches/mola-kernel.patch &&
  patch -p1 < ${CMAKE_CURRENT_SOURCE_DIR}/patches/mola-launcher.patch &&
  patch -p1 < ${CMAKE_CURRENT_SOURCE_DIR}/patches/mola-lidar-segmentation.patch &&
  patch -p1 < ${CMAKE_CURRENT_SOURCE_DIR}/patches/mola-slam-gtsam.patch
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
    -DCMAKE_BUILD_TYPE=Release
  BUILD_COMMAND make
  INSTALL_COMMAND make install
)

cs_install()
cs_export(CFG_EXTRAS mola-extras.cmake)
