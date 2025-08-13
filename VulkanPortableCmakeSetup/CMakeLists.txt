cmake_minimum_required(VERSION 3.31)
project(ProjectName) # Replace With your Project Name

set(CMAKE_CXX_STANDARD 20) # Targeting Cpp 20 
set(CMAKE_CXX_STANDARD_REQUIRED ON)


if (WIN32 AND MSYS) # Ensure MinGW Makefiles for MSYS2
    set(CMAKE_GENERATOR "MinGW Makefiles" CACHE INTERNAL "" FORCE)
endif()


include(FetchContent)
FetchContent_Declare(
        glfw
        GIT_REPOSITORY https://github.com/glfw/glfw.git
        GIT_TAG 3.4 # Optionally pin to a specific target glfw version
)
# ensure GLFW is built with Vulkan support
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(GLFW_VULKAN_STATIC ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(glfw)

# Find Vulkan
find_package(Vulkan REQUIRED)

# listing all project files under src direcectory
file(GLOB_RECURSE SOURCE_FILES CONFIGURE_DEPENDS
        src/*.cpp
        src/*.h
)

# define executable.... replace ProjectName  With your Project Name
add_executable(ProjectName ${SOURCE_FILES})

# link libraries and include directories... replace ProjectName  With your Project Name
target_link_libraries(ProjectName PRIVATE glfw Vulkan::Vulkan)
target_include_directories(ProjectName PRIVATE ${Vulkan_INCLUDE_DIRS})

# optional: ensuring Vulkan SDK is available on the system
if (NOT Vulkan_FOUND)
    message(FATAL_ERROR "Vulkan SDK not found. Please install it from: https://www.lunarg.com/vulkan-sdk/")
endif()
