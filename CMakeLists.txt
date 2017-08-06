cmake_minimum_required (VERSION 2.8.11)

######## Project settings ########
project(CServer)
set(LICENSE "GPL")

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread" )

file(GLOB SOURCES "src/*.cpp")

add_executable(cserver ${SOURCES})

# setup directory where we should look for cmake files
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/")

find_package(OpenSSL REQUIRED)
include_directories(${OPENSSL_INCLUDE_DIR})

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
    INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIR})
    LINK_DIRECTORIES(${Boost_LIBRARY_DIR})

endif()

target_link_libraries(cserver LINK_PUBLIC ${OPENSSL_LIBRARIES} ${Boost_LIBRARIES})
