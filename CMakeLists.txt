cmake_minimum_required(VERSION 3.21)

project(globed)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

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
# GEODE
# =========================

find_package(Geode REQUIRED)

# =========================
# SOURCES
# =========================

file(GLOB_RECURSE SOURCES
    src/*.cpp
)

# =========================
# MOD
# =========================

geode_add_mod(${PROJECT_NAME}
    ${SOURCES}
)

# =========================
# INCLUDES
# =========================

target_include_directories(${PROJECT_NAME} PRIVATE
    include
    libs
)

# =========================
# WINDOWS
# =========================

if (WIN32)
    target_link_libraries(${PROJECT_NAME}
        ws2_32
        crypt32
        secur32
        winmm
    )
endif()
