﻿cmake_minimum_required (VERSION 3.8)

if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project ("example")
add_subdirectory(thirdparty/luau)

add_library(example SHARED "test.cpp" "test.h")

target_link_libraries(example PRIVATE
    "${PROJECT_SOURCE_DIR}/libs/Luau.Compiler.lib"
    "${PROJECT_SOURCE_DIR}/libs/Luau.VM.lib"
    "${PROJECT_SOURCE_DIR}/libs/Luau.Ast.lib"
    raylib
)

target_include_directories(example PRIVATE
    "${PROJECT_SOURCE_DIR}/include"
)

target_link_libraries(example PRIVATE Luau.VM Luau.Compiler)

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET example PROPERTY CXX_STANDARD 20)
endif()