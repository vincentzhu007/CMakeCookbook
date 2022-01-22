cmake_minimum_required(VERSION 3.5)

project(CMakeCookbook LANGUAGES CXX)

set(USE_LIBRARY NO)

message("Compile source into a library? ${USE_LIBRARY}")

set(BUILD_SHARED_LIBS NO)

add_subdirectory(chapter01)