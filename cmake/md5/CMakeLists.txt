# Generated Cmake file begin
cmake_minimum_required(VERSION 3.1)

project(MD5 C CXX)
set(MD5_VERSION 1.2-1)

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

set(BUILD_COPY_DIRECTORIES "doc" "tests")
set(des56_INCDIRS "src/")
set(des56_SOURCES "src/compat-5.2.c" "src/des56.c" "src/ldes56.c")
set(md5.core_INCDIRS "src/")
set(md5.core_SOURCES "src/compat-5.2.c" "src/md5.c" "src/md5lib.c")
set(md5_SOURCES src/md5.lua)

install(DIRECTORY ${BUILD_COPY_DIRECTORIES} DESTINATION ${INSTALL_SHARE}/MD5)

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

install(FILES ${md5_SOURCES} DESTINATION ${INSTALL_LMOD}/ RENAME md5.lua)

add_library(des56 MODULE ${des56_SOURCES})

foreach(LIBRARY ${des56_LIB_NAMES})
    find_library(des56_${LIBRARY} ${LIBRARY} ${des56_LIBDIRS})
    list(APPEND des56_LIBRARIES ${LIBRARY})
endforeach(LIBRARY)

target_include_directories(des56 PRIVATE ${des56_INCDIRS} ${LUA_INCLUDE_DIRS} ${LUA_INCLUDE_DIR})
target_compile_definitions(des56 PRIVATE ${des56_DEFINES})
target_link_libraries(des56 PRIVATE ${des56_LIBRARIES} ${LUA_LIBRARIES})
# Do not prefix "lib" before target name
set_target_properties(des56 PROPERTIES PREFIX "")
set_target_properties(des56 PROPERTIES OUTPUT_NAME des56)
install(TARGETS des56 DESTINATION ${INSTALL_CMOD}/)
add_library(md5.core MODULE ${md5.core_SOURCES})

foreach(LIBRARY ${md5.core_LIB_NAMES})
    find_library(md5.core_${LIBRARY} ${LIBRARY} ${md5.core_LIBDIRS})
    list(APPEND md5.core_LIBRARIES ${LIBRARY})
endforeach(LIBRARY)

target_include_directories(md5.core PRIVATE ${md5.core_INCDIRS} ${LUA_INCLUDE_DIRS} ${LUA_INCLUDE_DIR})
target_compile_definitions(md5.core PRIVATE ${md5.core_DEFINES})
target_link_libraries(md5.core PRIVATE ${md5.core_LIBRARIES} ${LUA_LIBRARIES})
# Do not prefix "lib" before target name
set_target_properties(md5.core PROPERTIES PREFIX "")
set_target_properties(md5.core PROPERTIES OUTPUT_NAME core)
install(TARGETS md5.core DESTINATION ${INSTALL_CMOD}/md5)

