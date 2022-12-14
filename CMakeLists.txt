cmake_minimum_required(VERSION 3.18...3.18 FATAL_ERROR)

project(ANN VERSION 1.1.2 LANGUAGES CXX)

set(ANN_HEADERS
  include/ANN/ANN.h
  include/ANN/ANNperf.h
  include/ANN/ANNx.h
)

set(ANN_SOURCES
  src/ANN.cpp
  src/bd_fix_rad_search.cpp
  src/bd_pr_search.cpp
  src/bd_search.cpp
  src/bd_tree.cpp
  src/brute.cpp
  src/kd_dump.cpp
  src/kd_fix_rad_search.cpp
  src/kd_pr_search.cpp
  src/kd_search.cpp
  src/kd_split.cpp
  src/kd_tree.cpp
  src/kd_util.cpp
  src/perf.cpp
)

add_library(ann SHARED ${ANN_HEADERS} ${ANN_SOURCES})

target_include_directories(ann
  PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
         "$<INSTALL_INTERFACE:include>"
)

target_compile_definitions(ann PRIVATE $<$<PLATFORM_ID:Windows>:DLL_EXPORTS>)

set_target_properties(ann PROPERTIES
  DEBUG_POSTFIX d
  SOVERSION ${PROJECT_VERSION_MAJOR}
  VERSION ${PROJECT_VERSION}
)

set(${PROJECT_NAME}_LIBRARIES ann)

install(TARGETS ${${PROJECT_NAME}_LIBRARIES} EXPORT ${PROJECT_NAME}_TARGETS
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
)

install(FILES ${ANN_HEADERS}
  DESTINATION include/${PROJECT_NAME}
)

configure_file(
  ${PROJECT_NAME}Config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
  @ONLY
)

configure_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}ConfigVersion.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
  @ONLY
)

export(EXPORT ${PROJECT_NAME}_TARGETS
  FILE ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Targets.cmake
)

set(config_package_location lib/cmake/${PROJECT_NAME})

install(EXPORT ${PROJECT_NAME}_TARGETS
  FILE ${PROJECT_NAME}Targets.cmake
  DESTINATION ${config_package_location}
)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
  DESTINATION ${config_package_location}
)
