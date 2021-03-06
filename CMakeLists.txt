cmake_minimum_required(VERSION 3.1)
set(CUSTOM_PROJECT_NAME "glBasic" CACHE STRING "Project name")
project(${CUSTOM_PROJECT_NAME})

#option(GLFW_BUILD_DOCS OFF)
option(GLFW_BUILD_EXAMPLES OFF)
option(GLFW_BUILD_TESTS OFF)
add_subdirectory(glfw)

if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic -std=c++11")
    if(NOT WIN32)
        set(GLAD_LIBRARIES dl)
    endif()
endif()

include_directories(
	glad/include/
	glfw/include/
	glm/
	stb/
)

file(GLOB TemplateFiles template/*)
file(COPY ${TemplateFiles} DESTINATION ${CMAKE_BINARY_DIR}/src/)

file(GLOB GLAD_SOURCE glad/src/glad.c)
source_group("GladSource" FILES ${GLAD_SOURCE})

add_definitions(-DGLFW_INCLUDE_NONE -DPROJECT_SOURCE_DIR=\"${PROJECT_SOURCE_DIR}\")
add_executable(${PROJECT_NAME} ${GLAD_SOURCE})
target_link_libraries(${PROJECT_NAME} glfw ${GLFW_LIBRARIES} ${GLAD_LIBRARIES})
target_sources(${PROJECT_NAME} PUBLIC ${CMAKE_BINARY_DIR}/src/main.cpp)

set_target_properties(${PROJECT_NAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${PROJECT_NAME})