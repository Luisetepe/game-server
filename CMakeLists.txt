
# =============================
# Project and CMake Version
# =============================
cmake_minimum_required(VERSION 3.25)
project(game-server VERSION 1.0)

# =============================
# Compiler Settings
# =============================
# Use C++23 standard (required, no compiler-specific extensions)
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# =============================
# Set Architecture
# =============================
if(CMAKE_SYSTEM_PROCESSOR MATCHES "x86_64|AMD64")
    set(ARCH_DIR "x86_64")
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64|arm64")
    set(ARCH_DIR "aarch64")
else()
    message(FATAL_ERROR "Unsupported architecture: ${CMAKE_SYSTEM_PROCESSOR}")
endif()

set(PROJECT_TARGET_NAME game-server)

# =============================
# Dependencies: GameNetworkingSockets
# =============================
# Find the prebuilt GameNetworkingSockets library in the vendor directory
find_library(GNS_LIB
    NAMES GameNetworkingSockets
    PATHS "${CMAKE_SOURCE_DIR}/vendor/GameNetworkingSockets/bin/${ARCH_DIR}/"
    NO_DEFAULT_PATH
)

# Fail if the library is not found
if(NOT GNS_LIB)
    message(FATAL_ERROR "GameNetworkingSockets library not found in vendor/GameNetworkingSockets/bin")
endif()

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
# Include Directories
# =============================
# Add GameNetworkingSockets headers
target_include_directories(${PROJECT_TARGET_NAME} PRIVATE vendor/GameNetworkingSockets/include)

# =============================
# Link Libraries
# =============================
# Link the found GameNetworkingSockets library
target_link_libraries(${PROJECT_TARGET_NAME} PRIVATE ${GNS_LIB})

# =============================
# Output Directories
# =============================
# Place the executable in build/release
set_target_properties(
    ${PROJECT_TARGET_NAME}
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${CMAKE_BUILD_TYPE}"
)

add_custom_command(
    TARGET ${PROJECT_TARGET_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
            "${GNS_LIB}"
            "$<TARGET_FILE_DIR:${PROJECT_TARGET_NAME}>/"
)