cmake_minimum_required(VERSION 3.2)

project(hl_server C CXX)

SET (USER_SERVICE "user_service")
SET (MESSAGE_SERVICE "message_service")
SET (WALL_SERVICE "wall_service")


find_package(OpenSSL)
find_package(Threads)
find_package(ZLIB)
find_package(Poco REQUIRED COMPONENTS Foundation Util Net XML JSON Crypto NetSSL)


if(NOT ${Poco_FOUND})
    message(FATAL_ERROR "Poco C++ Libraries not found.")
endif()

include_directories(${Poco_INCLUDE_DIRS})
include_directories("/usr/include/mysql")
include_directories("/usr/local/include/mysql")
link_directories("/usr/local/lib")

SET(CMAKE_CXX_STANDARD 17)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(${USER_SERVICE} main.cpp 
                                 config/config.cpp 
                                 database/database.cpp
                                 database/user.cpp
                                 database/wall.cpp
                                 database/message.cpp
                                 )


target_include_directories(${USER_SERVICE} PRIVATE "${CMAKE_BINARY_DIR}")
target_compile_options(${USER_SERVICE} PRIVATE -Wall -Wextra -pedantic -Werror )
set_target_properties(${USER_SERVICE} PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(${USER_SERVICE} PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON)

target_link_libraries(${USER_SERVICE} PRIVATE 
                             ${CMAKE_THREAD_LIBS_INIT} 
                             ${Poco_LIBRARIES}
                             "PocoData"
                             "PocoDataMySQL"
                             "mysqlclient"
                             ZLIB::ZLIB)



add_executable(${MESSAGE_SERVICE} message_main.cpp 
                             config/config.cpp 
                             database/database.cpp
                             database/user.cpp
                             database/wall.cpp
                             database/message.cpp
                             )
target_include_directories(${MESSAGE_SERVICE} PRIVATE "${CMAKE_BINARY_DIR}")
target_compile_options(${MESSAGE_SERVICE} PRIVATE -Wall -Wextra -pedantic -Werror )
set_target_properties(${MESSAGE_SERVICE} PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(${MESSAGE_SERVICE} PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON)

target_link_libraries(${MESSAGE_SERVICE} PRIVATE 
                             ${CMAKE_THREAD_LIBS_INIT} 
                             ${Poco_LIBRARIES}
                             "PocoData"
                             "PocoDataMySQL"
                             "mysqlclient"
                             ZLIB::ZLIB)

add_executable(${WALL_SERVICE} wall_main.cpp 
                             config/config.cpp 
                             database/database.cpp
                             database/user.cpp
                             database/wall.cpp
                             database/message.cpp
                             )

target_include_directories(${WALL_SERVICE} PRIVATE "${CMAKE_BINARY_DIR}")
target_compile_options(${WALL_SERVICE} PRIVATE -Wall -Wextra -pedantic -Werror )
set_target_properties(${WALL_SERVICE} PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(${WALL_SERVICE} PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON)

target_link_libraries(${WALL_SERVICE} PRIVATE 
                             ${CMAKE_THREAD_LIBS_INIT} 
                             ${Poco_LIBRARIES}
                             "PocoData"
                             "PocoDataMySQL"
                             "mysqlclient"
                             ZLIB::ZLIB)





