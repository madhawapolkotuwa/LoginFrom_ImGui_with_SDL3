cmake_minimum_required(VERSION 3.10)
project(LoginApp CXX)

set(CMAKE_CXX_STANDARD 17) # Use C++17
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

include(cmake/UpdateSubmodules.cmake)

# Set CMAKE_BUILD_TYPE to Debug if not specified
if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "Choose the type of build." FORCE)
    message(STATUS "CMAKE_BUILD_TYPE not specified, defaulting to Debug")
endif()

set(RUNTIME_OUTPUT_DIR "${CMAKE_BINARY_DIR}/bin")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${RUNTIME_OUTPUT_DIR})
set(DESTINATION_PATH ${RUNTIME_OUTPUT_DIR}/$<CONFIG>)


add_subdirectory(extern/SDL3)

# Specify source files
set(SOURCES
    src/main.cpp
)

set(IMGUI_SOURCES
    extern/ImGui/imgui.cpp
    extern/ImGui/imgui_draw.cpp
    extern/ImGui/imgui_tables.cpp
    extern/ImGui/imgui_widgets.cpp
    extern/ImGui/imgui_demo.cpp 
    extern/ImGui/backends/imgui_impl_sdl3.cpp
    extern/ImGui/backends/imgui_impl_sdlrenderer3.cpp
)


add_executable(LoginApp ${SOURCES} ${IMGUI_SOURCES})

target_include_directories(LoginApp PRIVATE 
    ${CMAKE_SOURCE_DIR}/extern/SDL3/include/ 
    ${CMAKE_SOURCE_DIR}/extern/ImGui/ 
    ${CMAKE_SOURCE_DIR}/extern/ImGui/backends/)

# Link SDL3 
target_link_libraries(LoginApp PRIVATE opengl32.lib SDL3::SDL3)

target_compile_definitions(LoginApp PRIVATE IMGUI_IMPL_OPENGL_LOADER_CUSTOM=1)


set_target_properties(LoginApp PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${RUNTIME_OUTPUT_DIR}
)
