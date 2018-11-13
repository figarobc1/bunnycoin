if(DEFINED ENV{VCPKG_ROOT} AND NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  set(CMAKE_TOOLCHAIN_FILE "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake"
      CACHE STRING "")
endif()

cmake_minimum_required(VERSION 3.10)
project(bunnycoin)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

find_package(Boost COMPONENTS chrono filesystem program_options system thread REQUIRED)
find_package(Qt5Widgets CONFIG REQUIRED)
find_package(Qt5Network CONFIG REQUIRED)
find_package(Qt5LinguistTools CONFIG REQUIRED)
find_package(OpenSSL REQUIRED)

find_path(MINIUPNPC_INC NAMES miniupnpc/miniupnpc.h)
find_library(MINIUPNPC_LIB NAMES miniupnpc)
add_library(miniupnpc STATIC IMPORTED)
target_include_directories(miniupnpc INTERFACE ${MINIUPNPC_INC})
set_property(TARGET miniupnpc PROPERTY IMPORTED_LOCATION ${MINIUPNPC_LIB})

find_path(BERKELEYDB_INC NAMES db_cxx.h)
find_library(BERKELEYDB_LIB NAMES libdb48 db48)
add_library(berkeleydb STATIC IMPORTED)
target_include_directories(berkeleydb INTERFACE ${BERKELEYDB_INC})
target_compile_definitions(berkeleydb INTERFACE DB_USE_DLL)
set_property(TARGET berkeleydb PROPERTY IMPORTED_LOCATION ${BERKELEYDB_LIB})

add_subdirectory(src)
