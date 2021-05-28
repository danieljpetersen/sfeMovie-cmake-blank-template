cmake_minimum_required(VERSION 3.1)

set(PROJECT_NAME "integrate_sfemovie")
project(${PROJECT_NAME})

FILE(GLOB_RECURSE APP_SRC_FILES src/*h src/*.cpp src/*.c src/*.cc src/*.hh src/*.hpp src/*.hp)

add_executable(${PROJECT_NAME} ${APP_SRC_FILES})

# some compiler options
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}")

find_package(SFML 2.5.1 REQUIRED COMPONENTS system window graphics audio)
if (SFML_FOUND)
    include_directories(${SFML_INCLUDE_DIR})
    target_link_libraries(${PROJECT_NAME} ${SFML_LIBRARIES} ${SFML_DEPENDENCIES})
endif()

find_package(FFMPEG REQUIRED)
if (FFMPEG_FOUND)
    include_directories(${PROJECT_NAME} ${FFMPEG_INCLUDE_DIRS})
    target_link_libraries(${PROJECT_NAME} ${FFMPEG_LIBRARIES})
else()
    message("NOT FOUND")
endif()

find_package(sfeMovie REQUIRED)
if (SFEMOVIE_FOUND)
    include_directories(${PROJECT_NAME} SFEMOVIE_INCLUDE_DIR)
    target_link_libraries(${PROJECT_NAME} ${SFEMOVIE_LIB} ${SFEMOVIE_LIBRARY})
endif()

## GNU Compiler Flags
if(CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -Wno-sign-compare -Wreturn-type")
    
    target_link_libraries(${PROJECT_NAME} -lX11 -no-pie)

    IF(CMAKE_BUILD_TYPE MATCHES Release)
        message("Release build.")
        set(CMAKE_CXX_FLAGS "-O2 -Wreturn-type")
    else()
        message("Debug build.")
        set(CMAKE_CXX_FLAGS_DEBUG "-Wall -Wreturn-type")
    endif()
else()
    set(BUILD_ARCH "-m64")
endif()