
# =============================
# Project and CMake Version
# =============================
cmake_minimum_required(VERSION 3.25)
project(game-server VERSION 0.0.1)

# =============================
# Compiler Settings
# =============================
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(PROJECT_TARGET_NAME game-server)

# =============================
# Dependencies: GameNetworkingSockets
# =============================
find_package(GameNetworkingSockets CONFIG REQUIRED)

# =============================
# Source Files
# =============================
# Gather all .cpp and .hpp files in src/
file(GLOB_RECURSE SOURCES "src/*.cpp" "src/*.hpp")

# =============================
# Executable Target
# =============================
add_executable(${PROJECT_TARGET_NAME} ${SOURCES})

# =============================
# Link Libraries
# =============================
target_link_libraries(${PROJECT_TARGET_NAME} PRIVATE GameNetworkingSockets::GameNetworkingSockets)

# =============================
# Output Directories
# =============================
# Place the executable in build/release
set_target_properties(
    ${PROJECT_TARGET_NAME}
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${CMAKE_BUILD_TYPE}"
)