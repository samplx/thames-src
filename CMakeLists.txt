# set minimum cmake version
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

# project name and language
project(thames LANGUAGES C)

add_executable(thames
    src/_version.h
    src/_version.c
    src/appinfo.h
    src/cmdline.c
    src/errcheck.c
    src/extensions.c
    src/isis.c
    src/isisfile.c
    src/isisload.c
    src/thames.c
    src/utils.c
    src/z80.c
    src/cbops.h
    src/cmdline.h
    ${CMAKE_CURRENT_BINARY_DIR}/config.h
    src/edops.h
    src/errcheck.h
    src/extensions.h
    src/isis.h
    src/isisfile.h
    src/isisload.h
    src/thames.h
    src/z80.h
    src/z80ops.h
    )

target_compile_options(thames
    PRIVATE
    "-D_CRT_SECURE_NO_WARNINGS"
    "-D_CRT_NONSTDC_NO_WARNINGS"
    )

target_include_directories(thames PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)
add_custom_target(
getversion
COMMAND perl ${thames_SOURCE_DIR}/Scripts/getVersion.pl -W
BYPRODUCTS ${thames_SOURCE_DIR}/src/_version.h
WORKING_DIRECTORY ${thames_SOURCE_DIR}/src
)

add_dependencies(thames getversion)

include(CheckIncludeFiles)
CHECK_INCLUDE_FILES(sys/types.h HAVE_SYS_TYPES_H)
CHECK_INCLUDE_FILES(sys/stat.h HAVE_SYS_STAT_H)
CHECK_INCLUDE_FILES(fcntl.h HAVE_FNCTL_H)
CHECK_INCLUDE_FILES(unistd.h HAVE_UNISTD_H)
CHECK_INCLUDE_FILES(errno.h HAVE_ERRNO_H)
CHECK_INCLUDE_FILES(limits.h HAVE_LIMITS_H)
CHECK_INCLUDE_FILES(conio.h HAVE_CONIO_H)
CHECK_INCLUDE_FILES(io.h HAVE_IO_H)
include_directories(${CMAKE_CURRENT_BINARY_DIR} ${thames_SOURCE_DIR})
configure_file(config.in ${CMAKE_CURRENT_BINARY_DIR}/config.h @ONLY)


