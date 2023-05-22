# Include external dependencies using FetchContent
include(FetchContent)

FetchContent_Declare(
    fmt
    GIT_REPOSITORY https://github.com/fmtlib/fmt.git
    GIT_TAG        9.1.0
)

# Make external dependencies available to the project
FetchContent_MakeAvailable(fmt)

find_package(Threads REQUIRED)

add_subdirectory(evse)

add_executable(bridge bridge.cpp)
target_link_libraries(bridge PRIVATE
    Threads::Threads
    fmt::fmt
    slac::slac
)
