# ---- Options ----

option(ENABLE_TEST_COVERAGE "Enable test coverage" OFF)

set(EXECUTABLE_NAME plusplus-test)

# ---- Dependencies ----

CPMAddPackage("gh:doctest/doctest@2.5.2")

# ---- Create binary ----

file(GLOB sources CONFIGURE_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/source/*.cpp)
add_executable(${EXECUTABLE_NAME}
  source/greeter.cpp
  source/main.cpp
)
target_link_libraries(${EXECUTABLE_NAME} doctest::doctest plusplus::plusplus)
set_target_properties(${EXECUTABLE_NAME} PROPERTIES CXX_STANDARD 17)

# enable compiler warnings
if(CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR CMAKE_CXX_COMPILER_ID MATCHES "GNU")
  target_compile_options(${EXECUTABLE_NAME} PUBLIC -Wall -Wpedantic -Wextra -Werror)
elseif(MSVC)
  target_compile_options(${EXECUTABLE_NAME} PUBLIC /W4 /WX)
  target_compile_definitions(${EXECUTABLE_NAME} PUBLIC DOCTEST_CONFIG_USE_STD_HEADERS)
endif()

# ---- Add plusplusTests ----

# Note: doctest and similar testing frameworks can automatically configure CMake tests. For other
# testing frameworks add the tests target instead: add_test(NAME ${EXECUTABLE_NAME} COMMAND
# ${EXECUTABLE_NAME})

include(${doctest_SOURCE_DIR}/scripts/cmake/doctest.cmake)
doctest_discover_tests(${EXECUTABLE_NAME})

# ---- code coverage ----

if(ENABLE_TEST_COVERAGE)
  target_compile_options(${EXECUTABLE_NAME} PUBLIC -O0 -g -fprofile-arcs -ftest-coverage)
  target_link_options(${EXECUTABLE_NAME} PUBLIC -fprofile-arcs -ftest-coverage)
endif()
