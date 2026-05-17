cmake_minimum_required(VERSION 3.21)

project(globed VERSION 2.1.4)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# =========================
# TLS FIX
# =========================

set(QUNET_USE_XTLS ON CACHE BOOL "" FORCE)
set(QUNET_USE_SCHANNEL ON CACHE BOOL "" FORCE)

add_definitions(
    -DQUNET_USE_XTLS
    -DQUNET_USE_SCHANNEL
)

# =========================
# GEODE SETUP
# =========================

include(cmake/CPM.cmake)

CPMAddPackage(
    NAME Geode
    GITHUB_REPOSITORY geode-sdk/geode
    GIT_TAG nightly
)

# =========================
# SOURCES
# =========================

file(GLOB_RECURSE SOURCES
    src/*.cpp
)

# =========================
# MOD
# =========================

geode_add_mod(globed
    ${SOURCES}
)

# =========================
# INCLUDES
# =========================

target_include_directories(globed PRIVATE
    include
    libs
)

# =========================
# WINDOWS
# =========================

if (WIN32)
    target_link_libraries(globed
        ws2_32
        crypt32
        secur32
        winmm
    )
endif()
