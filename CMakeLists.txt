cmake_minimum_required(VERSION 2.8.3)
project(mola_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(mola_src
  GIT_REPOSITORY git@github.com:zarmomin/mola.git
  GIT_TAG dbow-fix
  UPDATE_COMMAND ""
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
    -DCMAKE_BUILD_TYPE=Release
  BUILD_COMMAND make
  INSTALL_COMMAND make install
)

#INSTALL_COMMAND cp -a ../mola_src-build/lib/. ${CATKIN_DEVEL_PREFIX}/lib/ && cp -r ../mola_src/include ${CATKIN_DEVEL_PREFIX}/include/mola

cs_install()
cs_export(CFG_EXTRAS mola-extras.cmake)
