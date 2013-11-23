# minimal cmakelists, just to make it build
cmake_minimum_required(VERSION 2.8.10)
project(uriparser)

option(BUILD_TESTS "Build tests (requires libcpptest)" ON)
if(BUILD_TESTS) 
	find_library(libcpptest cpptest REQUIRED) # FIXME: check for >= 1.1.0 version
endif()

option(DISABLE_UNICODE "Disable unicode char code" OFF)
if(DISABLE_UNICODE)
	add_definitions (-DURI_NO_UNICODE)
endif()

option(DISABLE_ANSI "Disable ansi char code" OFF)
if(DISABLE_ANSI)
	add_definitions (-DURI_NO_ANSI)
endif()

include_directories(
	include/)

set(Sources
	src/UriCommon.c
	src/UriCompare.c
	src/UriEscape.c
	src/UriFile.c
	src/UriIp4Base.c
	src/UriIp4.c
	src/UriNormalizeBase.c
	src/UriNormalize.c
	src/UriParseBase.c
	src/UriParse.c
	src/UriQuery.c
	src/UriRecompose.c
	src/UriResolve.c
	src/UriShorten.c)
add_library(uriparser ${Sources})

if(BUILD_TESTS)
	ENABLE_TESTING()
	add_executable(uriparser-test test/FourSuite.cpp  test/test.cpp)
	target_link_libraries(uriparser-test uriparser ${libcpptest})
	add_test(uriparser-test uriparser-test)
endif()

