cmake_minimum_required(VERSION 3.31)
project(glfw_project)

set(CMAKE_CXX_STANDARD 20)

set(GLFW_ROOT "C:/glfw")

# Добавляем include директорию
include_directories("${GLFW_ROOT}/include")

# Указываем путь к библиотеке (например, для Visual Studio 2022)
link_directories("${GLFW_ROOT}/lib-vc2022")


add_executable(glfw_project main.cpp)

find_package(fmt CONFIG REQUIRED)
find_package(OpenGL REQUIRED)
find_package(FreeGLUT CONFIG REQUIRED)

target_link_libraries(${PROJECT_NAME} glfw3dll opengl32 fmt::fmt OpenGL::GL $<IF:$<TARGET_EXISTS:FreeGLUT::freeglut>,FreeGLUT::freeglut,FreeGLUT::freeglut_static>)

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${GLFW_ROOT}/lib-vc2022/glfw3.dll"
        $<TARGET_FILE_DIR:${PROJECT_NAME}>)