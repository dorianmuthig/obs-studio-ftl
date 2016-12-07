project(libftl)

find_package(Libcurl REQUIRED)

include_directories(${OBS_JANSSON_INCLUDE_DIRS})
include_directories(${LIBCURL_INCLUDE_DIRS})

set(FTLSDK_SOURCES
	ftl-sdk/libftl/hmac/hmac.c
	ftl-sdk/libftl/hmac/sha2.c
	ftl-sdk/libftl/ftl-sdk.c
	ftl-sdk/libftl/handshake.c
	ftl-sdk/libftl/ingest.c
	ftl-sdk/libftl/ftl_helpers.c
	ftl-sdk/libftl/media.c
	ftl-sdk/libftl/gettimeofday/gettimeofday.c
	ftl-sdk/libftl/logging.c)
	
if (WIN32)
  set(FTLSDK_PLATFORM_SOURCES 
		ftl-sdk/libftl/win32/socket.c
		ftl-sdk/libftl/gettimeofday/gettimeofday.c
		ftl-sdk/libftl/win32/threads.c
		ftl-sdk/libftl/win32/threads.h)

  include_directories(ftl-sdk/libftl/win32)
else()
  set(FTLSDK_PLATFORM_SOURCES ftl-sdk/libftl/posix/socket.c
  							  ftl-sdk/libftl/posix/threads.c
							  ftl-sdk/libftl/posix/threads.h)
  include_directories(ftl-sdk/libftl/posix)
endif()	

set(FTLSDK_HEADERS
	ftl-sdk/libftl/hmac/hmac.h
	ftl-sdk/libftl/hmac/sha2.h
	ftl-sdk/libftl/ftl.h
	ftl-sdk/libftl/ftl_private.h)
	
if (WIN32)
  set(FTLSDK_PLATFORM_HEADERS 
		ftl-sdk/libftl/gettimeofday/gettimeofday.h)
endif()		

add_library(libftl SHARED 
	${FTLSDK_SOURCES}
	${FTLSDK_HEADERS}
	${FTLSDK_PLATFORM_SOURCES}
	${FTLSDK_PLATFORM_HEADERS})

set_target_properties(libftl PROPERTIES
        OUTPUT_NAME ftl
        VERSION "0"
        SOVERSION "0")

	
if(WIN32)
  target_link_libraries(libftl ws2_32 ${LIBCURL_LIBRARIES} ${OBS_JANSSON_IMPORT})
else()
  target_link_libraries(libftl ${LIBCURL_LIBRARIES} ${OBS_JANSSON_IMPORT})
endif()
	
target_include_directories(libftl
	PUBLIC
		"$<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/plugins/libftl>"
		"$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/include>")
		
file (COPY ${CMAKE_CURRENT_SOURCE_DIR}/ftl-sdk/libftl/ftl.h
           DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/include)

include_directories (${CMAKE_CURRENT_BINARY_DIR}/include)
include_directories (${CMAKE_CURRENT_SOURCE_DIR}/ftl-sdk/libftl/)
#set(LIBFTL_INCLUDE_DIRS ${CMAKE_CURRENT_BINARY_DIR}/include)

install_obs_plugin_with_data(libftl data)

