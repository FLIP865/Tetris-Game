cmake_minimum_required(VERSION 3.10)
project(Tetris LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(TARGET_NAME tetris)

set(SOURCE_DIR ${CMAKE_SOURCE_DIR}/src)
set(SOURCES
   ${SOURCE_DIR}/block.cpp
   ${SOURCE_DIR}/blocks.cpp
   ${SOURCE_DIR}/colors.cpp
   ${SOURCE_DIR}/game.cpp
   ${SOURCE_DIR}/grid.cpp
   ${SOURCE_DIR}/main.cpp
   ${SOURCE_DIR}/position.cpp
)


find_package(raylib QUIET)
   if (NOT raylib_FOUND)
       include(FetchContent)
       FetchContent_Declare(
           raylib
           GIT_REPOSITORY https://github.com/raysan5/raylib.git
           GIT_TAG 5.0
       )
       FetchContent_MakeAvailable(raylib)
   endif()

   add_executable(${TARGET_NAME} ${SOURCES})
   
   target_include_directories(${TARGET_NAME} PRIVATE ${SOURCE_DIR})

   target_link_libraries(${TARGET_NAME} PRIVATE raylib)

   if (WIN32)
       target_link_libraries(${TARGET_NAME} PRIVATE opengl32 gdi32 winmm)
   endif()

   if (UNIX AND NOT APPLE)
       target_link_libraries(${TARGET_NAME} PRIVATE m GL pthread dl rt X11)
   endif()

   if (EXISTS ${CMAKE_SOURCE_DIR}/Sounds)
       file(COPY ${CMAKE_SOURCE_DIR}/Sounds DESTINATION ${CMAKE_BINARY_DIR})
endif()
