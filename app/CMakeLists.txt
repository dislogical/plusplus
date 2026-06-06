# ---- Options ----

set(EXECUTABLE_NAME plusplus-app)

# ---- Dependencies ----

CPMAddPackage(
  GITHUB_REPOSITORY jarro2783/cxxopts
  VERSION 3.3.1
  OPTIONS "CXXOPTS_BUILD_EXAMPLES NO" "CXXOPTS_BUILD_TESTS NO" "CXXOPTS_ENABLE_INSTALL YES"
)

# ---- Create standalone executable ----

file(GLOB sources CONFIGURE_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/source/*.cpp)

add_executable(${EXECUTABLE_NAME} source/main.cpp)
set_target_properties(${EXECUTABLE_NAME} PROPERTIES CXX_STANDARD 17 OUTPUT_NAME "plusplus")
target_link_libraries(${EXECUTABLE_NAME} plusplus::plusplus cxxopts)
