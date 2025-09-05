cmake_minimum_required(VERSION 3.31)
project(ProjectName) # Replace With your Project Name

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if (WIN32 AND MSYS) # Ensure MinGW Makefiles for MSYS2
    set(CMAKE_GENERATOR "MinGW Makefiles" CACHE INTERNAL "" FORCE)
endif()

# Fetch GLFW
include(FetchContent)
FetchContent_Declare(
    glfw
    GIT_REPOSITORY https://github.com/glfw/glfw.git
    GIT_TAG 3.4
)
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(glfw)

# Fetch GLAD (for OpenGL function loading)
FetchContent_Declare(
    glad
    GIT_REPOSITORY https://github.com/Dav1dde/glad.git
    GIT_TAG v0.1.36 # stable version, can change if needed
)
FetchContent_MakeAvailable(glad)

# Find OpenGL
find_package(OpenGL REQUIRED)

# Source files
file(GLOB_RECURSE SOURCE_FILES CONFIGURE_DEPENDS
    src/*.cpp
    src/*.h
)

# Executable
add_executable(ProjectName ${SOURCE_FILES})

# Link libraries
target_link_libraries(ProjectName PRIVATE glfw glad OpenGL::GL)

# Include directories
target_include_directories(ProjectName PRIVATE
    ${OPENGL_INCLUDE_DIR}
)
