cmake_minimum_required(VERSION 3.11)
project(raster)

# --------------------------------------------------------------------------------

# OpenGL
find_package(OpenGL REQUIRED)

# OpenGL Mathematics
# https://github.com/g-truc/glm
set(GLM_INSTALL_ENABLE OFF)
add_subdirectory(glm)

# GLFW
# https://github.com/glfw/glfw
set(GLFW_BUILD_EXAMPLES OFF)
set(GLFW_BUILD_TESTS OFF)
set(GLFW_BUILD_DOCS OFF)
set(GLFW_INSTALL OFF)
add_subdirectory(glfw)

# tinyobjloader
# https://github.com/syoyo/tinyobjloader
add_subdirectory(tinyobjloader)

# --------------------------------------------------------------------------------

set(_IMGUI_DIR "${CMAKE_SOURCE_DIR}/imgui")
add_library(libraster STATIC
    "${_IMGUI_DIR}/imgui.cpp"
    "${_IMGUI_DIR}/imgui_draw.cpp"
    "${_IMGUI_DIR}/imgui_demo.cpp"
    "${_IMGUI_DIR}/examples/imgui_impl_glfw.cpp"
    "${_IMGUI_DIR}/examples/imgui_impl_opengl3.cpp"
    "${_IMGUI_DIR}/examples/libs/gl3w/GL/gl3w.c")
target_link_libraries(libraster PUBLIC glm glfw tinyobjloader ${OPENGL_LIBRARIES})
target_include_directories(libraster PUBLIC
    "${CMAKE_SOURCE_DIR}"
    "${_IMGUI_DIR}"
    "${_IMGUI_DIR}/examples"
    "${_IMGUI_DIR}/examples/libs/gl3w")
target_compile_features(libraster PUBLIC cxx_std_17)
target_compile_options(libraster PUBLIC "$<$<CXX_COMPILER_ID:MSVC>:/EHsc>")

# --------------------------------------------------------------------------------

add_executable(raster01_window "${CMAKE_SOURCE_DIR}/src/raster01_window.cpp")
target_link_libraries(raster01_window PRIVATE libraster)
add_executable(raster02_vertex_processing "${CMAKE_SOURCE_DIR}/src/raster02_vertex_processing.cpp")
target_link_libraries(raster02_vertex_processing PRIVATE libraster)
add_executable(raster03_clipping "${CMAKE_SOURCE_DIR}/src/raster03_clipping.cpp")
target_link_libraries(raster03_clipping PRIVATE libraster)
add_executable(raster04_rasterization "${CMAKE_SOURCE_DIR}/src/raster04_rasterization.cpp")
target_link_libraries(raster04_rasterization PRIVATE libraster)
add_executable(raster05_shadow "${CMAKE_SOURCE_DIR}/src/raster05_shadow.cpp")
target_link_libraries(raster05_shadow PRIVATE libraster)
