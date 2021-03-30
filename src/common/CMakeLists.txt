add_library(common
    imgui/imgui_impl_glfw.cpp
    imgui/imgui_impl_opengl3.cpp
    imgui/imgui_impl_glfw.h
    imgui/imgui_impl_opengl3.h
    application.hpp
    application.cpp
    shader.hpp
    shader.cpp
    mesh.hpp
    mesh.cpp
    data.hpp
    data.cpp
    texture.hpp
    texture.cpp
    gltf.hpp
    gltf.cpp
    framebuffer.hpp
    framebuffer.cpp
    renderer.hpp
    renderer.cpp
    utils.hpp
    utils.cpp
)

# Macros for building header-only libraries
target_compile_definitions(common PRIVATE
    TINYGLTF_IMPLEMENTATION
    STB_IMAGE_WRITE_STATIC
    STB_IMAGE_WRITE_IMPLEMENTATION
    STB_IMAGE_STATIC
    STB_IMAGE_IMPLEMENTATION
)

conan_target_link_libraries(common PUBLIC glfw glew_s stb imgui glm tinygltf)

# linkage to Windows IMM library is necessary for Dear Imgui
if(WIN32)
    target_link_libraries(common imm32 version winmm)
endif()

target_compile_features(common PUBLIC cxx_std_17)
configure_file(config.in.h ${CMAKE_CURRENT_BINARY_DIR}/include/config.h)
target_include_directories(common PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/include)
