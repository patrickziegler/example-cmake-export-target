cmake_minimum_required(VERSION 3.12)

project(CalcCli VERSION 0.1.0)

set(TARGET_NAME add)

find_package(Calc)

add_executable(
    ${TARGET_NAME})

target_sources(
    ${TARGET_NAME} PRIVATE
    src/main.cpp)

target_link_libraries(
    ${TARGET_NAME} PRIVATE Calc)
