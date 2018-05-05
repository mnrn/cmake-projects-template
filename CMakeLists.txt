cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
cmake_policy(VERSION 2.8)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake")

# TODO: cmake_template -> your project name
set(PROJECT_NAME cmake_template)
project(${PROJECT_NAME})


set(PROJECTS_DIR_NAME projects)
include_directories(external)
include_directories(base)


# Compile mode
set(CMAKE_CXX_FLAGS_DEBUG           "${CMAKE_CXX_FLAGS_DEBUG}           -DDEBUG  -g -pg")
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO, "${CMAKE_CXX_FLAGS_RELWITHDEBINFO}           -g -pg   -O3")
set(CMAKE_CXX_FLAGS_RELEASE,        "${CMAKE_CXX_FLAGS_RELEASE}         -DNDEBUG         -O4 -flto ")
set(CMAKE_CXX_FLAGS_MINSIZEREL,     "${CMAKE_CXX_FLAGS_MINSIZEREL}      -DNDEBUG         -Oz")

# Resolve dependency
find_package(Threads REQUIRED)
set(Boost_USE_STATIC_LIBS ON)
find_package(Boost COMPONENTS system filesystem thread REQUIRED)
if (NOT Boost_FOUND)
    message(FATAL_ERROR "Could not find boost!")
else()
    include_directories(${Boost_INCLUDE_DIRS})
endif()


# Set preprocessor defines
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DNOMINMAX -D_USE_MATH_DEFINES")


# Warning settings
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wundef")
elseif(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Weverything")
    
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-c++98-compat -Wno-c++98-compat-pedantic")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-documentation")
endif()


add_definitions(-std=c++1z)
file(GLOB SOURCE *.cpp base/*.cpp)


# Function for building
function(build TARGET_NAME)
    # Main
    file(GLOB SOURCE *.cpp base/*.cpp ${PROJECTS_DIR_NAME}/${TARGET_NAME}/*.cpp)
    set(MAIN_CPP ${TARGET_NAME}/${TARGET_NAME}/main.cpp)
    if (EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/${PROJECTS_DIR_NAME}/${TARGET_NAME}/main.cpp)
        set(MAIN_CPP ${PROJECTS_DIR_NAME}/${TARGET_NAME}/main.cpp)
    endif()

    add_executable(${TARGET_NAME}
        ${MAIN_CPP}
        ${SOURCE}
    )
    target_link_libraries(${TARGET_NAME} 
        ${CMAKE_THREAD_LIBS_INIT}
        ${Boost_LIBRARIES}
    )
endfunction(build)


# Build All
function(buildAll)
    foreach(TARGET ${TARGETS})
        build(${TARGET})
    endforeach(TARGET)
endfunction(buildAll)


#link_libraries(${} ${})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/bin/")

# TODO: hello -> directory that you want build.
set(TARGETS
    hello
)
buildAll()