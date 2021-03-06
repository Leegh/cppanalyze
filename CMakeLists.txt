cmake_minimum_required(VERSION 2.8)

project(CppAnalyze)

find_package(LLVM)

if( NOT LLVM_FOUND )
    message(FATAL_ERROR "LLVM package can't be found. Set CMAKE_PREFIX_PATH variable to LLVM's installation prefix.")
endif()

add_definitions(${LLVM_DEFINITIONS} -fno-rtti)
include_directories( ${LLVM_INCLUDE_DIRS} )
link_directories( ${LLVM_LIBRARY_DIRS} )


set(clang_used_libraries
    clangFrontendTool
    clangFrontend
    clangDriver
    clangSerialization
    clangCodeGen
    clangParse
    clangSema
    clangAnalysis
    clangIndex
    clangRewrite
    clangAST
    clangLex
    clangBasic
    )

set(SRC_DIR "src")
set(CPPANALYZE_SRC
    ${SRC_DIR}/cppanalyze.cpp
)
set(CPPANALYZE_HEADERS
    ${SRC_DIR}/rename_consumer.h
)


add_library(cppanalyze SHARED
            ${CPPANALYZE_SRC}
            ${CPPANALYZE_HEADERS}
)

target_link_libraries(cppanalyze
    ${clang_used_libraries}
    # XXX add find boost and clean that
    boost_filesystem
)
