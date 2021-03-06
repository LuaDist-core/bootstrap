# Generated Cmake file begin
cmake_minimum_required(VERSION 3.1)

project(rocksolver C CXX)
set(rocksolver_VERSION 0.4.2-2)

set(ENV{LUA_DIR} ${CMAKE_INSTALL_PREFIX})
find_package(Lua REQUIRED)

## INSTALL DEFAULTS (Relative to CMAKE_INSTALL_PREFIX)
# Primary paths
set(INSTALL_BIN bin CACHE PATH "Where to install binaries to.")
set(INSTALL_LIB lib CACHE PATH "Where to install libraries to.")
set(INSTALL_ETC etc CACHE PATH "Where to store configuration files")
set(INSTALL_SHARE share CACHE PATH "Directory for shared data.")

set(INSTALL_LMOD ${INSTALL_LIB}/lua/${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR} CACHE PATH "Directory to install Lua modules.")
set(INSTALL_CMOD ${INSTALL_LIB}/lua/${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR} CACHE PATH "Directory to install Lua binary modules.")

## Platform detection
# By default use system name
set(DETECTED_PLATFORM ${CMAKE_SYSTEM_NAME})
string(TOLOWER ${DETECTED_PLATFORM} DETECTED_PLATFORM)

# Overrides for more specific platforms
if (DETECTED_PLATFORM STREQUAL "darwin")
    set(DETECTED_PLATFORM macosx)
elseif (MINGW)
    set(DETECTED_PLATFORM mingw32)
elseif (MSYS)
    set(DETECTED_PLATFORM msys)
elseif (CYGWIN)
    set(DETECTED_PLATFORM cygwin)
elseif (WIN32)
    set(DETECTED_PLATFORM win32)
elseif (UNIX)
    set(DETECTED_PLATFORM unix)
endif()

set(rocksolver.DependencySolver_SOURCES rocksolver/DependencySolver.lua)
set(rocksolver.Package_SOURCES rocksolver/Package.lua)
set(rocksolver.constraints_SOURCES rocksolver/constraints.lua)
set(rocksolver.utils_SOURCES rocksolver/utils.lua)

install(DIRECTORY ${BUILD_COPY_DIRECTORIES} DESTINATION ${INSTALL_SHARE}/rocksolver)

function(build_install KEYS DIR)
    list(REMOVE_DUPLICATES KEYS)

    foreach(KEY ${${KEYS}})
        set(BASE_NAME ${KEYS}_${KEY})
        install(FILES ${${BASE_NAME}_SRC} DESTINATION ${DIR}/${${BASE_NAME}_DST} RENAME ${${BASE_NAME}_RENAME})
    endforeach(KEY)
endfunction(build_install)

build_install(BUILD_INSTALL_lua ${INSTALL_LMOD})
build_install(BUILD_INSTALL_lib ${INSTALL_LIB})
build_install(BUILD_INSTALL_conf ${INSTALL_ETC})
build_install(BUILD_INSTALL_bin ${INSTALL_BIN})

install(FILES ${rocksolver.DependencySolver_SOURCES} DESTINATION ${INSTALL_LMOD}/rocksolver RENAME DependencySolver.lua)
install(FILES ${rocksolver.Package_SOURCES} DESTINATION ${INSTALL_LMOD}/rocksolver RENAME Package.lua)
install(FILES ${rocksolver.constraints_SOURCES} DESTINATION ${INSTALL_LMOD}/rocksolver RENAME constraints.lua)
install(FILES ${rocksolver.utils_SOURCES} DESTINATION ${INSTALL_LMOD}/rocksolver RENAME utils.lua)


