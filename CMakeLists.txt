cmake_minimum_required(VERSION 3.0)
# Project name in here
project(ImGuiBaseProject)
message("Project name: " ${CMAKE_PROJECT_NAME})

# Setting Output Directorys
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Looks for the OpenGL Package
find_package(OpenGL REQUIRED)
# Looks for other packages for linux (Threads, x11)
if (UNIX)
    if (NOT APPLE)
        find_package(Threads REQUIRED)
        find_package(X11 REQUIRED)
    endif()
endif()

if (WIN32)
    SET (PROGRAMS-PREFIX "c:/programs")
else()
    SET (PROGRAMS-PREFIX "~/programs")
endif()

# Add all Source Files you want to include here
set(CPP_SRC_FILES 
    src/main.cpp
)

set(IMGUI_CPP_FILES
    include/imgui/include/imgui/imgui.cpp
    include/imgui/include/imgui/imgui_demo.cpp
    include/imgui/include/imgui/imgui_draw.cpp
    include/imgui/include/imgui/imgui_impl_glfw.cpp
    include/imgui/include/imgui/imgui_impl_opengl3.cpp
    include/imgui/include/imgui/imgui_tables.cpp
    include/imgui/include/imgui/imgui_widgets.cpp
)

# Sets include directories for glad GLFW and imgui
include_directories(include/glad/include include/GLFW/include include/imgui/include/ src/)

# Setting GLFW Build up
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
# Adds glad and GLFW as subdirectory
add_subdirectory(include/GLFW)
add_subdirectory(include/glad)
# adds a Executable with the Source files defined in CPP_SRC_FILES
add_executable(${CMAKE_PROJECT_NAME} ${CPP_SRC_FILES} ${IMGUI_CPP_FILES})
# Links all libarys to executable
target_link_libraries(${CMAKE_PROJECT_NAME} ${OPENGL_gl_LIBRARY} glad glfw)
if (UNIX)
    if (APPLE)
        target_link_libraries(${CMAKE_PROJECT_NAME}
            "-framework Cocoa"
            "-framework IOKit"
            "-framework CoreVideo"
            )
    else()
        target_link_libraries(${CMAKE_PROJECT_NAME}
            ${CMAKE_THREAD_LIBS_INIT}
            ${X11_LIBRARIES}
            ${CMAKE_DL_LIBS}
            )
    endif()
endif()