cmake_minimum_required(VERSION 3.21)

project(globed VERSION 2.1.4)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# =========================
# Geode Bootstrap
# =========================

execute_process(
    COMMAND git clone https://github.com/geode-sdk/geode.git geode-sdk
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    RESULT_VARIABLE GEODE_CLONE_RESULT
)

include(${CMAKE_BINARY_DIR}/geode-sdk/cmake/GeodeFile.cmake)

# =========================
# TLS
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
# Mod
# =========================

geode_add_mod(globed
    ${SOURCES}
)

# =========================
# Includes
# =========================

target_include_directories(globed PRIVATE
    include
    libs
)

# =========================
# Windows
# =========================

if (WIN32)
    target_link_libraries(globed
        ws2_32
        crypt32
        secur32
        winmm
    )
endif()
