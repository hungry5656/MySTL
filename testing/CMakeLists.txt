cmake_minimum_required(VERSION 3.20)
project(STL_gtest)
set(CMAKE_CXX_STANDARD 14)

include_directories(${PROJECT_SOURCE_DIR}/src)

find_package(Threads REQUIRED) #needed to build google test
if(NOT IS_DIRECTORY ${PROJECT_SOURCE_DIR}/googletest) # if googletest is not a part of the project
    #go and download it
    include(FetchContent) #so we can pull in google test
    FetchContent_Declare( #declare information about google test
            googletest #its variable name
            #its location on the internet
            GIT_REPOSITORY https://github.com/google/googletest.git
            GIT_TAG main #what branch we want
            )

    #download google test if we don't have it
    FetchContent_GetProperties(googletest)
    if (NOT googletest_POPULATED)
        FetchContent_Populate(googletest)
    endif ()
    #add this to the cmake command line -DINSTALL_GTEST=OFF
    add_subdirectory(${googletest_SOURCE_DIR} ${googletest_BINARY_DIR})
else()
    add_subdirectory(googletest)
endif()

set(APP_SRC testing.cpp array_test.h ../src/array.h)
set(APP_SRC2 manualTest.cpp)
add_executable(STL_gtest ${APP_SRC})
add_executable(STL_manual_test ${APP_SRC2})

#these aren't technically needed for google test to build
#but for CLion to stop complaining
target_include_directories(STL_gtest
        PRIVATE
        #google test
        ${googletest_SOURCE_DIR}/googletest/include
        #gmock
        ${googletest_SOURCE_DIR}/googlemock/include)

target_link_libraries(STL_gtest
        PRIVATE
        STLSrc #src code to test
        gtest #link in google test
        gmock #link in google mock
        gtest_main #add this one if you want to use google's basic main
        )

target_compile_options(STL_gtest PRIVATE -Wall -Werror -Wextra)
target_compile_options(STL_manual_test PRIVATE -Wall -Werror -Wextra)