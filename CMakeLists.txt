cmake_minimum_required(VERSION 3.10)
project(boostdemo)

add_executable(boostdemo ${PROJECT_SOURCE_DIR}/main.cpp)

set( Boost_Bootstrap_Command ${CMAKE_BINARY_DIR}/boost/src/boost/bootstrap.sh && cp ${CMAKE_BINARY_DIR}/boost/src/boost-build/b2 ${CMAKE_BINARY_DIR}/boost/src/boost/)
set( Boost_b2_Command cd ${CMAKE_BINARY_DIR}/boost/src/boost && ./b2 )

include(ExternalProject)
ExternalProject_Add(
  boost-external
  GIT_REPOSITORY https://github.com/boostorg/boost.git
  GIT_TAG boost-1.79.0
  PREFIX boost
  DOWNLOAD_DIR boost/src
  CONFIGURE_COMMAND ${Boost_Bootstrap_Command}
  BUILD_COMMAND  ${Boost_b2_Command} install
    --without-python
    --without-mpi
    --disable-icu
    --prefix=${CMAKE_BINARY_DIR}/INSTALL
    --threading=single,multi
    --link=shared
    --variant=release
    -j12
  INSTALL_COMMAND ""
)

set(Boost_LIBRARY_DIR ${CMAKE_BINARY_DIR}/INSTALL/lib/boost/ )
set(Boost_INCLUDE_DIR ${CMAKE_BINARY_DIR}/INSTALL/include/ )

add_dependencies(boostdemo boost-external)
include_directories(boostdemo ${Boost_INCLUDE_DIR})
target_link_libraries(boostdemo ${Boost_LIBRARY_DIR})
