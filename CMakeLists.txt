cmake_minimum_required(VERSION 3.17)

project(cherry_engine LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
        
add_executable(cherry_engine src/Main.cpp
)

find_package(glfw3 CONFIG REQUIRED)
find_package(glad CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)

find_path(STB_INCLUDE_DIRS "stb.h")

find_package(imgui CONFIG REQUIRED)

target_include_directories(cherry_engine PRIVATE ${STB_INCLUDE_DIRS})
target_link_libraries(cherry_engine PRIVATE glfw
                                        glad::glad
                                        glm::glm
                                        imgui::imgui
)

if(MSVC)
  target_compile_options(cherry_engine PRIVATE /W4 /wd4201) 
else()
  target_compile_options(cherry_engine PRIVATE -Wall -Wextra -pedantic)
endif()

if (WIN32)
    add_custom_command(TARGET cherry_engine POST_BUILD
                       COMMAND ${CMAKE_COMMAND} -E copy_directory
                       "${CMAKE_SOURCE_DIR}/data"
                       $<TARGET_FILE_DIR:cherry_engine>
    )
endif()