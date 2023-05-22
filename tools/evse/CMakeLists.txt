# Include external dependencies using FetchContent
include(FetchContent)

FetchContent_Declare(
    fsm
    GIT_REPOSITORY https://github.com/jacek-arrow/libfsm.git
    GIT_TAG        v0.3.0
)

FetchContent_MakeAvailable(fsm)

add_executable(evse
    evse.cpp
    evse_fsm.cpp
    slac_io.cpp
)

target_link_libraries(evse PRIVATE
    Threads::Threads
    fmt::fmt
    fsm::fsm
    slac::slac
)
