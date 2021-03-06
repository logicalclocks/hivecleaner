project (hiveCleaner)

cmake_minimum_required (VERSION 2.8)

set(HIVECLEANER_VERSION_MAJOR 0)
set(HIVECLEANER_VERSION_MINOR 2)
set(HIVECLEANER_VERSION_BUILD 0)

set (CMAKE_CXX_STANDARD 11)

configure_file(Version.h.in ${CMAKE_SOURCE_DIR}/include/Version.h)

set(Boost_USE_STATIC_LIBS ON)

find_package(Boost 1.53 REQUIRED COMPONENTS system date_time program_options thread)
find_package(CURL 7.29.0)

include_directories(${Boost_INCLUDE_DIRS})
include_directories(${CURL_INCLUDE_DIRS})
include_directories(${CMAKE_SOURCE_DIR}/include)

if(NOT NDB_DIR)
  set(NDB_DIR "/srv/hops/mysql")
  message(STATUS "NDB_DIR was not defined. Default: ${NDB_DIR} ")
endif()

include_directories(${NDB_DIR}/include ${NDB_DIR}/include/storage/ndb ${NDB_DIR}/include/storage/ndb/ndbapi ${NDB_DIR}/include/storage/ndb/mgmapi)
link_directories(${NDB_DIR}/lib)

SET( CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -DBOOST_SPIRIT_USE_PHOENIX_V3 -Wall -Wextra -Wformat-security  -Woverloaded-virtual -Wno-unused-variable -Wno-unused-parameter -Wno-unused-but-set-variable -Wno-strict-aliasing -Wno-switch -O3 -DDBUG_OFF" )

file(GLOB SOURCE ${CMAKE_SOURCE_DIR}/src/*.cpp)

add_executable(hiveCleaner ${SOURCE})

target_link_libraries(hiveCleaner ${Boost_LIBRARIES} ${CURL_LIBRARIES} ndbclient pthread)
