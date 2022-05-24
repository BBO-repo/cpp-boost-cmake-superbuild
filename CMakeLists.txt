cmake_minimum_required(VERSION 3.10)
project(boostdemo)

add_executable(boostdemo ${PROJECT_SOURCE_DIR}/main.cpp)

set( Boost_Bootstrap_Command ${CMAKE_BINARY_DIR}/boost-download/src/boost-external/bootstrap.sh && cp ${CMAKE_BINARY_DIR}/boost-download/src/boost-external-build/b2 ${CMAKE_BINARY_DIR}/boost-download/src/boost-external/)
set( Boost_b2_Command cd ${CMAKE_BINARY_DIR}/boost-download/src/boost-external && ./b2 )

include(ExternalProject)
ExternalProject_Add(
  boost-external
  GIT_REPOSITORY https://github.com/boostorg/boost.git
  GIT_TAG boost-1.79.0
  PREFIX boost-download
  CONFIGURE_COMMAND ${Boost_Bootstrap_Command}
  BUILD_COMMAND  ${Boost_b2_Command} install
    --without-python
    --without-mpi
    --disable-icu
    --prefix=${CMAKE_SOURCE_DIR}/thirdparty/boost/
    --threading=single,multi
    --link=shared
    --variant=release
    -j12
  INSTALL_COMMAND ""
)

# set(Boost_LIBRARY_DIR ${CMAKE_SOURCE_DIR}/thirdparty/boost/lib/boost/ )
set(Boost_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/thirdparty/boost/include/ )

add_dependencies(boostdemo boost-external)
include_directories(boostdemo ${Boost_INCLUDE_DIR})
# target_link_libraries(boostdemo ${Boost_LIBRARY_DIR})
