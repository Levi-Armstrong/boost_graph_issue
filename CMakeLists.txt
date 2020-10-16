cmake_minimum_required(VERSION 3.5.0)
project(boost_graph_issue VERSION 0.1.0 LANGUAGES CXX)

if(WIN32)
  set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()

find_package(Boost REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(console_bridge REQUIRED)

add_library(${PROJECT_NAME} src/graph.cpp)
target_link_libraries(${PROJECT_NAME} PUBLIC ${Boost_LIBRARIES} console_bridge)
target_compile_options(${PROJECT_NAME} PRIVATE ${TESSERACT_COMPILE_OPTIONS})
target_compile_definitions(${PROJECT_NAME} PUBLIC _USE_MATH_DEFINES=ON Boost_USE_STATIC_LIBS=OFF Boost_USE_MULTITHREADED=ON Boost_USE_STATIC_RUNTIME=OFF)
target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_14)
target_include_directories(${PROJECT_NAME} PUBLIC
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
    "$<INSTALL_INTERFACE:include>")
target_include_directories(${PROJECT_NAME} SYSTEM PUBLIC
    ${EIGEN3_INCLUDE_DIRS}
    ${Boost_INCLUDE_DIRS})

add_executable(graph_test src/graph_test.cpp)
target_link_libraries(graph_test PUBLIC ${PROJECT_NAME})
target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_14)
target_include_directories(graph_test PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>")

# Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME}
  DESTINATION include
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

install(TARGETS ${PROJECT_NAME} graph_test
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)
