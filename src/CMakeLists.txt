add_subdirectory(common)

add_executable(main main.cpp material.cpp material.hpp)

target_link_libraries(main PRIVATE common)