add_definitions(-DTF_DLL)

find_package(Qt5 COMPONENTS Core Network Xml Sql Concurrent REQUIRED)
if (NOT Qt5_FOUND)
  message(FATAL_ERROR "Qt5 was not found. Consider setting QT5_CMAKE_PATH to the Qt5Config.cmake directory.")
endif()

file(GLOB model_headers ${PROJECT_SOURCE_DIR}/models/*.h)
file(GLOB model_srcs    ${PROJECT_SOURCE_DIR}/models/*.cpp)
file(GLOB model_sqlobjects_headers   ${PROJECT_SOURCE_DIR}/models/sqlobjects/*.h)
file(GLOB model_mongoobjects_headers ${PROJECT_SOURCE_DIR}/models/mongoobjects/*.h)

add_library(model SHARED
  ${model_headers}
  ${model_srcs}
  ${model_sqlobjects_headers}
  ${model_mongoobjects_headers}
)
target_include_directories(model PUBLIC
  ${Qt5Core_INCLUDE_DIRS}
  ${Qt5Network_INCLUDE_DIRS}
  ${Qt5Xml_INCLUDE_DIRS}
  ${Qt5Sql_INCLUDE_DIRS}
  ${Qt5Concurrent_INCLUDE_DIRS}
  ${TreeFrog_INCLUDE_DIR}
  ${PROJECT_SOURCE_DIR}/models
  ${PROJECT_SOURCE_DIR}/models/sqlobjects
  ${PROJECT_SOURCE_DIR}/models/mongoobjects
)
target_link_libraries(model
  Qt5::Core
  Qt5::Network
  Qt5::Xml
  Qt5::Sql
  Qt5::Concurrent
  ${TreeFrog_LIB}
  helper
)
set_target_properties(model PROPERTIES
  LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib
  SOVERSION 1.0
)
add_dependencies(model
  helper
)
