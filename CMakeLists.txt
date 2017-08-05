cmake_minimum_required (VERSION 2.8.11)

######## Project settings ########
project(CServer)
set(LICENSE "GPL")

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread" )

file(GLOB SOURCES "src/*.cpp")

add_executable(cserver ${SOURCES})

# setup directory where we should look for cmake files
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/")

set(Boost_USE_STATIC_LIBS ON)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME OFF)

FIND_PACKAGE(Boost COMPONENTS system REQUIRED)

target_include_directories(cserver PRIVATE ${Boost_INCLUDE_DIRS})
target_link_libraries(cserver ${Boost_LIBRARIES})
