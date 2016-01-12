

add_executable(example1 example1.cpp)
target_link_libraries(example1 ${INTEROP_LIB})

add_executable(example2 example2.cpp)
target_link_libraries(example2 ${INTEROP_LIB})

if(NOT ENABLE_EXAMPLES)
    set_target_properties(example1 PROPERTIES EXCLUDE_FROM_ALL 1 EXCLUDE_FROM_DEFAULT_BUILD 1)
    set_target_properties(example2 PROPERTIES EXCLUDE_FROM_ALL 1 EXCLUDE_FROM_DEFAULT_BUILD 1)
endif()

if(NOT ENABLE_STATIC)
    add_custom_command(TARGET example1 POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy_if_different $<TARGET_FILE_DIR:${INTEROP_LIB}> ${CMAKE_CURRENT_BINARY_DIR})
    add_custom_command(TARGET example2 POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy_if_different $<TARGET_FILE_DIR:${INTEROP_LIB}> ${CMAKE_CURRENT_BINARY_DIR})
endif()

add_subdirectory("csharp")