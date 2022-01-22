list(APPEND _sources
    Message.hpp
    Message.cpp
)

# 条件编译
if (USE_LIBRARY)
    # 创建对象库
    add_library(message-obj
        OBJECT
        ${_sources}
    )

    # 在一些老的编译器中需要显式设置
    set_target_properties(message-obj
        PROPERTIES
        POSITION_INDEPENDENT_CODE 1
    )

    # 使用对象库创建动态库
    # 引用对象库的语法是生成器表达式$<TARGET_OBJECTS:message-obj>

    add_library(message-shared
        SHARED
        $<TARGET_OBJECTS:message-obj>
    )

    set_target_properties(message-shared
        PROPERTIES
        OUTPUT_NAME "message"
    )

    # 使用对象库创建静态库
    add_library(message-static
        STATIC
        $<TARGET_OBJECTS:message-obj>
    )

    set_target_properties(message-static
        PROPERTIES
        OUTPUT_NAME "message"
    )

    add_executable(hello_world hello_world.cpp)
    target_link_libraries(hello_world message-static)

else()
    add_executable(hello_world hello_world.cpp ${_sources})

endif()