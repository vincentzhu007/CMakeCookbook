cmake_minimum_required(VERSION 3.5)

# 设置项目名
project(CMakeCookbook LANGUAGES C CXX)

# 设置默认构建类型
if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()
message(STATUS "Build type: " ${CMAKE_BUILD_TYPE})

# 打印编译器选项
message(STATUS "C flags, Debug configuration: ${CMAKE_C_FLAGS_DEBUG}")
message(STATUS "C flags, Release configuration: ${CMAKE_C_FLAGS_RELEASE}")
message(STATUS "C flags, Release configuration with Debug info: ${CMAKE_C_FLAGS_RELWITHDEBINFO}")
message(STATUS "C flags, minimal Release configuration: ${CMAKE_C_FLAGS_MINSIZEREL}")
message(STATUS "C++ flags, Debug configuration: ${CMAKE_CXX_FLAGS_DEBUG}")
message(STATUS "C++ flags, Release configuration: ${CMAKE_CXX_FLAGS_RELEASE}")
message(STATUS "C++ flags, Release configuration with Debug info: ${CMAKE_CXX_FLAGS_RELWITHDEBINFO}")
message(STATUS "C++ flags, minimal Release configuration: ${CMAKE_CXX_FLAGS_MINSIZEREL}")

# 打印编译器信息
message(STATUS "Is the C++ compiler loaded? ${CMAKE_CXX_COMPILER_LOADED}")
if (CMAKE_CXX_COMPILER_LOADED)
    message(STATUS "The C++ compiler ID is " ${CMAKE_CXX_COMPILER_ID})
    message(STATUS "Is C++ compiler from GNU? " ${CMAKE_COMPILER_IS_GNUCXX})
    message(STATUS "The C++ comiler version is " ${CMAKE_CXX_COMPILER_VERSION})
endif()

message(STATUS "Is the C compiler loaded? ${CMAKE_C_COMPILER_LOADED}")
if(CMAKE_C_COMPILER_LOADED)
    message(STATUS "The C compiler ID is: ${CMAKE_C_COMPILER_ID}")
    message(STATUS "Is the C from GNU? ${CMAKE_COMPILER_IS_GNUCC}")
    message(STATUS "The C compiler version is: ${CMAKE_C_COMPILER_VERSION}")
endif()

# 是否构建库选项
option(USE_LIBRARY "Compile source into a library" ON)
message(STATUS "Compile source into a library? ${USE_LIBRARY}")
set(BUILD_SHARED_LIBS ${USE_LIBRARY})

# 添加子目录
add_subdirectory(chapter01)