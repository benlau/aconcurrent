cmake_minimum_required(VERSION 3.10)
project(aconcurrent VERSION 1.0.0 LANGUAGES CXX)

find_package(Qt5 COMPONENTS Concurrent REQUIRED)
find_package(qtasyncfuture REQUIRED)
set(SOURCES
    aconcurrent.cpp)
add_library(aconcurrent ${SOURCES})

target_include_directories(aconcurrent PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>)

if(ENABLE_TESTING)
    add_subdirectory(tests)
endif()

target_link_libraries(aconcurrent PRIVATE Qt5::Concurrent)
set_target_properties(aconcurrent PROPERTIES PUBLIC_HEADER "aconcurrent.h")

if(ENABLE_TESTING)
    add_subdirectory(tests)
endif()

install(TARGETS aconcurrent
    EXPORT aconcurrentTargets
    ARCHIVE  DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY  DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    RUNTIME  DESTINATION ${CMAKE_INSTALL_BINDIR})
export(TARGETS aconcurrent NAMESPACE aconcurrent:: FILE aconcurrentConfig.cmake)
set(CMAKE_EXPORT_PACKAGE_REGISTRY ON)
export(PACKAGE aconcurrent)
install(EXPORT aconcurrentTargets NAMESPACE aconcurrent:: DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/aconcurrent FILE aconcurrentConfig.cmake)
