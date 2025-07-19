cmake_minimum_required(VERSION 3.25)

project(game-server VERSION 1.0)

# Set the C++ standard to C++23
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Add the dependencies subdirectories
add_subdirectory(deps/GameNetworkingSockets)

# Create an executable from our source files
file(GLOB_RECURSE SOURCES "src/*.cpp" "src/*.hpp")
add_executable(game-server ${SOURCES})

# Set the output directory for the executable
set_target_properties(
    game-server
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/release"
)

# Link our application against dependent libraries
target_link_libraries(
    game-server
    PRIVATE
    GameNetworkingSockets::GameNetworkingSockets_s
)