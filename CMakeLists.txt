cmake_minimum_required(VERSION 3.11)

project(slac VERSION 0.1
        DESCRIPTION "Simple ISO15118-3 SLAC implementation"
		LANGUAGES CXX C
)

# options
option(BUILD_SLAC_TOOLS "Build SLAC tools" OFF)
option(SLAC_INSTALL "Install the library (shared data might be installed anyway)" ${EVC_MAIN_PROJECT})

add_subdirectory(3rd_party)

add_library(slac)
add_library(slac::slac ALIAS slac)

target_include_directories(slac PRIVATE $<TARGET_PROPERTY:HashLibrary,INTERFACE_INCLUDE_DIRECTORIES>)

target_include_directories(slac PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

target_sources(slac
    PRIVATE
        src/channel.cpp
        src/slac.cpp
        src/packet_socket.cpp
        $<TARGET_OBJECTS:HashLibrary>
)

if (BUILD_SLAC_TOOLS)
    add_subdirectory(tools)
endif()

if (SLAC_INSTALL)
    install(
        TARGETS slac
        EXPORT slac-targets
    )

    install(
        DIRECTORY include/
        TYPE INCLUDE
    )

    evc_setup_package(
        NAME slac
        NAMESPACE slac
        EXPORT slac-targets
    )
endif()
