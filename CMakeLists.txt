cmake_minimum_required(VERSION 3.12.0)

project(http_server LANGUAGES CXX)

include(pmm.cmake)

pmm(
    # VERBOSE
    # DEBUG
    VCPKG
        REVISION 14bfca23c51ca7bdeea44681677a93be1fbeb5fb
        REQUIRES beast nlohmann-json
)

pmm(CMakeCM
        ROLLING
)

include(CMakeRC)

cmrc_add_resource_library(
    html-resources
    ALIAS html::rc
    NAMESPACE html
    WHENCE html
    html/index.html
)

# https://stackoverflow.com/a/54334181/7130817
if(WIN32)
    add_definitions(-D_WIN32_WINNT=0x0A00 -DURLOPENER=\"start\")
elseif(LINUX)
    add_definitions(-DURLOPENER=\"xdg-open\")
elseif(MACOS)
    add_definitions(-DURLOPENER=\"open\")
endif()

find_package(Boost)
add_executable(http_server main.cpp)
target_link_libraries(http_server PRIVATE Boost::boost html::rc)
