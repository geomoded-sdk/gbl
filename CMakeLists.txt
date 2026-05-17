cmake_minimum_required(VERSION 3.21)

project(globed VERSION 2.1.4)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# =========================
# Geode
# =========================

execute_process(
    COMMAND git clone https://github.com/geode-sdk/geode.git geode-sdk
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
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
# Setup Mod
# =========================

setup_geode_mod(${PROJECT_NAME})

# =========================
# Includes
# =========================

target_include_directories(${PROJECT_NAME} PRIVATE
    include
    libs
)

# =========================
# Windows
# =========================

if (WIN32)
    target_link_libraries(${PROJECT_NAME}
        ws2_32
        crypt32
        secur32
        winmm
    )
endif()
