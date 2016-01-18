cmake_minimum_required(VERSION 3.0.0)
project(rtmpdump)

set(HTTP_PROXY "" CACHE STRING "HTTP proxy for package downloading.")

set(RUNTIME_PATH "${CMAKE_SOURCE_DIR}/runtime" CACHE PATH "Optional Path to look for libraries, headers and binaries.")
list(APPEND CMAKE_PREFIX_PATH "${RUNTIME_PATH}")

if(MSVC)
	add_definitions(-D_CRT_SECURE_NO_WARNINGS -D_CRT_NO_POSIX_ERROR_CODES)
	set(GETOPT_SRCS getopt.c getopt_long.c getopt.h)
endif(MSVC)

find_package(ZLIB)
find_package(OpenSSL)
include_directories(${OPENSSL_INCLUDE_DIR} librtmp .)

add_library(rtmp STATIC
	librtmp/log.c 
	librtmp/log.h 
	librtmp/rtmp.c 
	librtmp/rtmp.h
	librtmp/rtmp_sys.h
	librtmp/handshake.h
	librtmp/dh.h
	librtmp/log.h
	librtmp/amf.h
	librtmp/amf.c
	librtmp/amf.h
	librtmp/bytes.h
	librtmp/log.h
	librtmp/hashswf.c
	librtmp/http.h
	librtmp/rtmp.h
	librtmp/rtmp_sys.h
	librtmp/parseurl.c
	librtmp/rtmp.h
	librtmp/rtmp_sys.h
	librtmp/log.h)
	
add_executable(rtmpdump rtmpdump.c ${GETOPT_SRCS})
target_link_libraries(rtmpdump rtmp ws2_32 winmm gdi32 ${LIB_EAY_RELEASE} ${SSL_EAY_RELEASE} ${ZLIB_LIBRARY})
set_target_properties(rtmpdump PROPERTIES
    COMPILE_FLAGS "-DRTMPDUMP_VERSION=\\\"v2.4\\\""
)

add_executable(rtmpsrv rtmpsrv.c thread.c thread.h)
target_link_libraries(rtmpsrv rtmp ws2_32 winmm gdi32 ${LIB_EAY_RELEASE} ${SSL_EAY_RELEASE} ${ZLIB_LIBRARY})
set_target_properties(rtmpsrv PROPERTIES
    COMPILE_FLAGS "-DRTMPDUMP_VERSION=\\\"v2.4\\\""
)

add_executable(rtmpsuck rtmpsuck.c thread.c thread.h)
target_link_libraries(rtmpsuck rtmp ws2_32 winmm gdi32 ${LIB_EAY_RELEASE} ${SSL_EAY_RELEASE} ${ZLIB_LIBRARY})
set_target_properties(rtmpsuck PROPERTIES
    COMPILE_FLAGS "-DRTMPDUMP_VERSION=\\\"v2.4\\\""
)

add_executable(rtmpgw rtmpgw.c thread.c thread.h ${GETOPT_SRCS})
target_link_libraries(rtmpgw rtmp ws2_32 winmm gdi32 ${LIB_EAY_RELEASE} ${SSL_EAY_RELEASE} ${ZLIB_LIBRARY})
set_target_properties(rtmpgw PROPERTIES
    COMPILE_FLAGS "-DRTMPDUMP_VERSION=\\\"v2.4\\\""
)

install(TARGETS rtmpdump rtmpsrv rtmpsuck rtmpgw
        RUNTIME DESTINATION bin)
