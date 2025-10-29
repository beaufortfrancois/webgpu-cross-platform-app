cmake_minimum_required(VERSION 3.22) # CMake version check
project(app)                         # Create project "app"
set(CMAKE_CXX_STANDARD 20)           # Enable C++20 standard

add_executable(app "main.cpp")

include(FetchContent)
FetchContent_Declare(
  dawn
  GIT_REPOSITORY https://dawn.googlesource.com/dawn
  GIT_TAG chromium/7444 # Chrome 142
  GIT_SHALLOW TRUE
  GIT_SUBMODULES ""
)
set(DAWN_FETCH_DEPENDENCIES ON)
FetchContent_MakeAvailable(dawn)

if(EMSCRIPTEN)
  set_target_properties(app PROPERTIES SUFFIX ".html")
  target_link_libraries(app PRIVATE emdawnwebgpu_cpp webgpu_glfw)
  target_link_options(app PRIVATE "-sASYNCIFY=1" "-sUSE_GLFW=3")
else()
  target_link_libraries(app PRIVATE webgpu_dawn webgpu_glfw glfw)
endif()
