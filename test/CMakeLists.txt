add_executable(main main.cpp)
target_include_directories(main PUBLIC $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>)
target_link_libraries(main sympp)
