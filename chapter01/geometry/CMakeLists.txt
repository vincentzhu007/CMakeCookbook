# 生成源代码列表
list(APPEND _sources
    geometry_circle.hpp
    geometry_circle.cpp
    geometry_polygon.hpp
    geometry_polygon.cpp
    geometry_rhombus.hpp
    geometry_rhombus.cpp
    geometry_square.hpp
    geometry_square.cpp
)

if (USE_LIBRARY)
    add_library(geometry-objs OBJECT ${_sources})
    set_target_properties(geometry-objs
        PROPERTIES
        POSITION_INDEPENDENT_CODE 1
    )

    add_library(geometry-shared SHARED $<TARGET_OBJECTS:geometry-objs>)
    set_target_properties(geometry-shared
        PROPERTIES
        OUTPUT_NAME "geometry"
    )

    add_library(geometry-static STATIC $<TARGET_OBJECTS:geometry-objs>)
    set_target_properties(geometry-static
        PROPERTIES
        OUTPUT_NAME "geometry"
    )

    add_executable(computer-area computer_area.cpp)
    target_link_libraries(computer-area geometry-static)
else()
    add_executable(computer-area computer_area.cpp ${_sources})
endif()