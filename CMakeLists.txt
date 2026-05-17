cmake_minimum_required(VERSION 3.21)

project(Globed VERSION 2.1.4)

# =========================
# C++
# =========================

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# =========================
# Windows / MSVC
# =========================

if (MSVC)
    set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>DLL")

    add_compile_options(
        /W3
        /EHsc
        /utf-8
        /MP
    )
else()
    add_compile_options(
        -Wall
        -Wextra
        -Wno-unused-parameter
    )
endif()

# =========================
# QUNET TLS FIX
# =========================

set(QUNET_USE_XTLS ON CACHE BOOL "" FORCE)
set(QUNET_USE_SCHANNEL ON CACHE BOOL "" FORCE)

add_definitions(
    -DQUNET_USE_XTLS
    -DQUNET_USE_SCHANNEL
)

# =========================
# Sources
# =========================

file(GLOB_RECURSE SOURCES
    src/*.cpp
)

# =========================
# Library
# =========================

add_library(${PROJECT_NAME} SHARED ${SOURCES})

# =========================
# Includes
# =========================

target_include_directories(${PROJECT_NAME} PRIVATE
    include
    libs
)

# =========================
# Windows libs
# =========================

if (WIN32)
    target_link_libraries(${PROJECT_NAME}
        ws2_32
        winmm
        crypt32
        secur32
    )
endif()

# =========================
# Output
# =========================

set_target_properties(${PROJECT_NAME} PROPERTIES
    OUTPUT_NAME "globed"
)
