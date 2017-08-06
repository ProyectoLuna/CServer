cmake_minimum_required (VERSION 2.8.11)

######## Project settings ########
project(CServer)
set(LICENSE "GPL")

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread" )

file(GLOB SOURCES "src/*.cpp")

add_executable(cserver ${SOURCES})

# setup directory where we should look for cmake files
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/")

# SQLite3

find_package (sqlite3 REQUIRED)
if (SQLITE3_FOUND)
  include_directories(${SQLITE3_INCLUDE_DIRS})
endif (SQLITE3_FOUND)

# OpenSSL

find_package(OpenSSL REQUIRED)
include_directories(${OPENSSL_INCLUDE_DIR})

# Boost

set(Boost_USE_STATIC_LIBS ${USE_STATIC_BOOST})
set(Boost_USE_MULTITHREADED ON)

unset(Boost_INCLUDE_DIR CACHE)
unset(Boost_LIBRARY_DIRS CACHE)

find_package(Boost 1.63.0 REQUIRED COMPONENTS system)

if(USE_STATIC_BOOST)
   message(STATUS "Linking against boost static libraries")
else(USE_STATIC_BOOST)
   message(STATUS "Linking against boost dynamic libraries")
endif(USE_STATIC_BOOST)

if (Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIR})
    link_directories(${Boost_LIBRARY_DIR})

endif()

target_link_libraries(cserver LINK_PUBLIC ${SQLITE3_LIBRARIES} ${OPENSSL_LIBRARIES} ${Boost_LIBRARIES})
