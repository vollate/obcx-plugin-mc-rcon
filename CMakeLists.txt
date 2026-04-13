cmake_minimum_required(VERSION 3.20)
project(obcx-plugin-mc-rcon LANGUAGES CXX C)

# When built standalone, find the installed SDK
# When built via FetchContent from OBCX, obcx_core is already available
if(NOT TARGET obcx_core)
    find_package(obcx-sdk REQUIRED)
endif()

include(OBCXPlugin)

# Plugin-specific dependency
find_package(tomlplusplus CONFIG REQUIRED)

# MC RCON uses C code
set_source_files_properties(src/mcrcon.c PROPERTIES LANGUAGE C)

obcx_add_plugin(mc_rcon
    SOURCES
        src/mc_rcon_plugin.cpp
        src/mc_rcon_plugin.hpp
        src/rcon_client.cpp
        src/rcon_client.hpp
        src/config.cpp
        src/config.hpp
        src/mcrcon.c
        src/mcrcon.h
    DEPS
        tomlplusplus::tomlplusplus)

# C standard for the mcrcon.c file
set_target_properties(mc_rcon_plugin PROPERTIES
    C_STANDARD 11
    C_STANDARD_REQUIRED ON)
