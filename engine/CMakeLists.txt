cmake_minimum_required(VERSION 3.28)
project(gdv-engine LANGUAGES CXX C)

# Variables
set(VENDOR_DIR "./vendor")
set(SRC_DIR "./src")
set(INCLUDE_DIR "./include")
set(SHADER_DIR "./shader")

# OpenGL Flags
set(OpenGL_GL_PREFERENCE GLVND)

# GLFW - Installed via: sudo dnf install -y glfw-devel
option(GLFW_BUILD_DOCS OFF)
option(GLFW_BUILD_EXAMPLES OFF)
option(GLFW_BUILD_TESTS OFF)
option(GLFW_INSTALL OFF)
find_package(glfw3 REQUIRED)

# Glad - Installed via: git submodule add git@github.com:Dav1dde/glad.git vendor/glad
add_library(glad
  STATIC
    ${VENDOR_DIR}/glad/src/glad.c
)

target_include_directories(glad
  PRIVATE
    ${VENDOR_DIR}/glad/include
)

# Project
add_executable(${PROJECT_NAME})

target_include_directories(${PROJECT_NAME}
  PRIVATE
    ${VENDOR_DIR}/glad/include
)

target_sources(${PROJECT_NAME}
  PRIVATE
    ${SRC_DIR}/main.cpp
    ${SRC_DIR}/window.cpp
)

target_link_libraries(${PROJECT_NAME}
  PRIVATE
    SDL2main 
    SDL2
    glfw
    glad
    ${CMAKE_DL_LIBS} # Needed for glad - https://stackoverflow.com/a/56842079/2394163
)

set_target_properties(${PROJECT_NAME}
  PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${PROJECT_NAME}
    COMPILE_OPTIONS -Werror
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)