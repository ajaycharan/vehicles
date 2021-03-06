cmake_minimum_required(VERSION 2.6)
#SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -ansi")
#SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99")
#SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pedantic")

SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2")

SET(json-c_sources
	arraylist.c
	debug.c
	json_object.c
	json_tokener.c
	json_util.c
	linkhash.c
	printbuf.c
	JSON_checker.c
	json_more_utils.c
)

ADD_LIBRARY(json-c STATIC ${json-c_sources})

#ADD_EXECUTABLE(test1 test1.c)
ADD_EXECUTABLE(test2 test2.c)

#TARGET_LINK_LIBRARIES(test1 json-c)
TARGET_LINK_LIBRARIES(test2 json-c)


FILE(GLOB json_headers "*.h")
INSTALL(FILES ${json_headers} DESTINATION include/json-c)


INCLUDE(CheckIncludeFile)

MACRO(MY_CHECK_INCLUDE_FILE file VAR)
	CHECK_INCLUDE_FILE(${file} ${file}-exists)
	IF(${${file}-exists})
		SET(${VAR} 1)
	ELSE()
		SET(${VAR} 0)
	ENDIF(${${file}-exists})
ENDMACRO(MY_CHECK_INCLUDE_FILE file VAR)

MY_CHECK_INCLUDE_FILE(fcntl.h HAVE_FCNTL_H)
MY_CHECK_INCLUDE_FILE(inttypes.h HAVE_INTTYPES_H)
MY_CHECK_INCLUDE_FILE(limits.h HAVE_LIMITS_H)
MY_CHECK_INCLUDE_FILE(memory.h HAVE_MEMORY_H)
MY_CHECK_INCLUDE_FILE(stdarg.h HAVE_STDARG_H)
MY_CHECK_INCLUDE_FILE(stdint.h HAVE_STDINT_H)
MY_CHECK_INCLUDE_FILE(stdlib.h HAVE_STDLIB_H)
MY_CHECK_INCLUDE_FILE(strings.h HAVE_STRINGS_H)
MY_CHECK_INCLUDE_FILE(string.h HAVE_STRING_H)
MY_CHECK_INCLUDE_FILE(syslog.h HAVE_SYSLOG_H)
MY_CHECK_INCLUDE_FILE(sys/param.h HAVE_SYS_PARAM_H)
MY_CHECK_INCLUDE_FILE(sys/types.h HAVE_SYS_TYPES_H)
MY_CHECK_INCLUDE_FILE(sys/stat.h HAVE_SYS_STAT_H)
MY_CHECK_INCLUDE_FILE(unistd.h HAVE_UNISTD_H)

MACRO(MY_CHECK_FUNCTION_EXISTS function VAR)
	CHECK_FUNCTION_EXISTS(${function} ${function}-exists)
	IF(${${function}-exists})
		# MESSAGE(STATUS "Exists ${function} = ${${function}-exists}")
		SET(${VAR} 1)
	ELSE()
		# MESSAGE(STATUS "NOT Exists ${function} = ${${function}-exists}")
		SET(${VAR} 0)
	ENDIF(${${function}-exists})
ENDMACRO(MY_CHECK_FUNCTION_EXISTS function VAR)

INCLUDE(CheckFunctionExists)

MY_CHECK_FUNCTION_EXISTS(open HAVE_OPEN)
MY_CHECK_FUNCTION_EXISTS(strubded HAVE_STRUB)
MY_CHECK_FUNCTION_EXISTS(malloc HAVE_MALLOC)
MY_CHECK_FUNCTION_EXISTS(realloc HAVE_REALLOC)
MY_CHECK_FUNCTION_EXISTS(strerror HAVE_STRERROR)
MY_CHECK_FUNCTION_EXISTS(strncasecmp HAVE_STRNCASECMP)
MY_CHECK_FUNCTION_EXISTS(strndup HAVE_STRNDUP)
MY_CHECK_FUNCTION_EXISTS(vasprintf HAVE_VASPRINTF)
MY_CHECK_FUNCTION_EXISTS(vprintf HAVE_VPRINTF)
MY_CHECK_FUNCTION_EXISTS(vsnprintf HAVE_VSNPRINTF)
MY_CHECK_FUNCTION_EXISTS(vsyslog HAVE_VSYSLOG)

CONFIGURE_FILE(config.h.cmake ${CMAKE_CURRENT_SOURCE_DIR}/config.h)

CONFIGURE_FILE(json-c.pc.in ${CMAKE_CURRENT_SOURCE_DIR}/json-c.pc @ONLY)

INSTALL(FILES ${CMAKE_CURRENT_SOURCE_DIR}/json-c.pc
	DESTINATION lib/pkgconfig)

	INSTALL(FILES ${CMAKE_CURRENT_SOURCE_DIR}/json-c.pc
		DESTINATION lib/pkgconfig)

INSTALL(TARGETS json-c ARCHIVE DESTINATION lib)

