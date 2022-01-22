cmake_minimum_required(VERSION 3.5)

project(CMakeCookbook LANGUAGES CXX)

set(USE_LIBRARY OFF)

message("Compile source into a library? ${USE_LIBRARY}")

set(BUILD_SHARED_LIBS OFF)

add_subdirectory(chapter01)