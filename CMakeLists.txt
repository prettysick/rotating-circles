cmake_minimum_required(VERSION 3.10)
project(rotating_circles C)

set(CMAKE_C_STANDARD 11)

# Force static linking for everything
set(CMAKE_EXE_LINKER_FLAGS "-static")
set(BUILD_SHARED_LIBS OFF)
set(USE_STATIC_RUNTIME ON)

# Raylib specific static options
set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(BUILD_GAMES OFF CACHE BOOL "" FORCE)
set(SUPPORT_BUSY_WAIT_LOOP ON CACHE BOOL "" FORCE)

if(WIN32)
    set(USE_MSVC_RUNTIME_LIBRARY_DLL OFF CACHE BOOL "" FORCE)
    set(OpenGL_GL_PREFERENCE "GLVND")
endif()

# Fetch and build static raylib
include(FetchContent)
FetchContent_Declare(
    raylib
    GIT_REPOSITORY https://github.com/raysan5/raylib.git
    GIT_TAG 4.5.0
)
FetchContent_MakeAvailable(raylib)

set(SOURCES 
    src/main.c
    src/linked_list.c
    src/render.c
)

add_executable(${PROJECT_NAME} ${SOURCES})
target_include_directories(${PROJECT_NAME} PRIVATE src)
target_link_libraries(${PROJECT_NAME} raylib)

if(MSVC)
    target_link_options(${PROJECT_NAME} PRIVATE /INCREMENTAL:NO /NODEFAULTLIB:MSVCRT)
endif()
