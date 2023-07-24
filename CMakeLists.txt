cmake_minimum_required(VERSION 3.8)
cmake_policy(SET CMP0048 "NEW")

project(delabella LANGUAGES CXX VERSION 1.0)

add_library(${PROJECT_NAME} STATIC ${PROJECT_SOURCE_DIR}/delabella.cpp)

target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR})

add_library(delabella::delabella ALIAS delabella)


set_target_properties( ${PROJECT_NAME} PROPERTIES CXX_STANDARD 17 )
set_target_properties( ${PROJECT_NAME} PROPERTIES POSITION_INDEPENDENT_CODE ON )

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    set_target_properties( ${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-ffp-model=strict -ffp-contract=off -mfma")
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "AppleClang")
    set_target_properties( ${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-ffp-model=strict -ffp-contract=off -mfma")
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    set_target_properties( ${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-frounding-math -fsignaling-nans")
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Intel")
    if (WIN32)
        set_target_properties( ${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-fp:strict")
    else()
        set_target_properties( ${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-fp-model=strict")
    endif()
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
    set_target_properties( ${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-fp:strict")
else()
    message("Compiler not recognized")
    message(${CMAKE_CXX_COMPILER_ID})
endif()


install(FILES ${PROJECT_SOURCE_DIR}/delabella.h DESTINATION include)
install(TARGETS ${PROJECT_NAME} ARCHIVE DESTINATION lib)
