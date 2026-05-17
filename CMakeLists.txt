cmake_minimum_required(VERSION 3.21)

project(globed VERSION 2.1.4)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# =========================
# Geode SDK
# =========================

include(cmake/GeodeFile.cmake)

# =========================
# TLS Fix
# =========================

set(QUNET_USE_XTLS ON CACHE BOOL "" FORCE)
set(QUNET_USE_SCHANNEL ON CACHE BOOL "" FORCE)

add_definitions(
    -DQUNET_USE_XTLS
    -DQUNET_USE_SCHANNEL
)

# =========================
# Mod
# =========================

geode_add_mod(globed
    src/main.cpp
)

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
        crypt32
        secur32
        winmm
    )
endif()
