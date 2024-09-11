
cmake_minimum_required(VERSION 3.12)

project(libxmlmm VERSION 1.0.0)

find_package(ament_cmake REQUIRED)

set(CMAKE_STATIC_LIBRARY_PREFIX "")
set(CMAKE_SHARED_LIBRARY_PREFIX "")

file(GLOB header
    "libxmlmm/*.h"
)
file(GLOB src
    "libxmlmm/*.cpp"
)

find_package(LibXml2 REQUIRED)

add_library(${PROJECT_NAME} SHARED "")

include_directories(${LIBXML2_INCLUDE_DIR})

target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/libxmlmm>
    $<INSTALL_INTERFACE:include>
)

target_link_libraries(${PROJECT_NAME} ${LIBXML2_LIBRARIES})

target_sources(${PROJECT_NAME}
    PRIVATE ${header}
    PRIVATE ${src}
)

install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

install(
    DIRECTORY libxmlmm/
    DESTINATION include
    FILES_MATCHING PATTERN "*.h")

ament_export_include_directories(include)
ament_export_targets(${PROJECT_NAME})
ament_package()
