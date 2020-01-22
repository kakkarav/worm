cmake_minimum_required(VERSION 3.13)
project(worm_mbl)

set(CMAKE_CXX_STANDARD 14)
find_package(HDF5 COMPONENTS CXX)
find_package(Boost
        COMPONENTS serialization system)
find_package(Python COMPONENTS Development)
include_directories(src)
include_directories(src/bcl)
include_directories(src/HelpLibs)
include_directories(src/worms)
add_subdirectory(${CMAKE_SOURCE_DIR}/src/HelpLibs/pybind11)

set(SOURCE_FILES
        src/bcl/condition.hpp
        src/bcl/heatbath.hpp
        src/bcl/markov.hpp
        src/bcl/metropolis.hpp
        src/bcl/observable.hpp
        src/bcl/random_choice.hpp
        src/bcl/random_shuffle.hpp
        src/bcl/st2010.hpp
        src/bcl/version.hpp
        src/HelpLibs/aligned.h
        src/HelpLibs/ArrayMultiD.cpp
        src/HelpLibs/ArrayMultiD.h
        src/HelpLibs/def.cpp
        src/HelpLibs/def.h
        src/HelpLibs/fftwPlan.cpp
        src/HelpLibs/fftwPlan.h
        src/HelpLibs/gzip.cpp
        src/HelpLibs/hdf5RW.cpp
        src/HelpLibs/hdf5RW.h
        src/HelpLibs/InputFile.cpp
        src/HelpLibs/InputFile.h
        src/HelpLibs/Observable.cpp
        src/HelpLibs/Observable.h
        src/HelpLibs/QmcParameters.cpp
        src/HelpLibs/QmcParameters.h
        src/HelpLibs/RandomGen.cpp
        src/HelpLibs/RandomGen.h
        src/HelpLibs/zlib.cpp
        src/worms/bond_operator.hpp
        src/worms/chain_lattice.hpp
        src/worms/heisenberg_operator.hpp
        src/worms/observable.hpp
        src/worms/operations.hpp
        src/worms/outgoing_weight.hpp
        src/worms/spacetime_point.hpp
        src/worms/spin_state.hpp
        src/worms/version.hpp
        src/worms/weight.hpp
        src/bcl.hpp
        src/LatticeData.hpp
        src/Measure.cpp
        src/Measure.h
        src/worms.cpp
        src/worms.hpp
    )
message(STATUS ${Python_INCLUDE_DIRS})
add_executable(worms_bin ${SOURCE_FILES} src/main.cpp )
target_include_directories(worms_bin PUBLIC ${HDF5_INCLUDE_DIRS} ${BOOST_INCLUDE_DIRS} ${CMAKE_CURRENT_SOURCE_DIR}/src/HelpLibs)
target_link_libraries(worms_bin ${HDF5_LIBRARIES} ${Boost_LIBRARIES} fftw3 )
target_compile_definitions(worms_bin PUBLIC NDEBUG)
target_compile_options(worms_bin PUBLIC -O3 -std=c++11 )

#add_library(z2dqmc_matter_python_pt SHARED ${SOURCE_FILES_MATTER})
pybind11_add_module(worms ${SOURCE_FILES})
target_include_directories(worms PUBLIC ${HDF5_INCLUDE_DIRS} ${Python_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS} ${CMAKE_SOURCE_DIR}/src/HelpLibs)
target_link_libraries(worms PUBLIC ${HDF5_LIBRARIES} ${Boost_LIBRARIES} fftw3)
target_compile_definitions(worms PUBLIC PYTHON  NDEBUG)
target_compile_options(worms PUBLIC -O0 -std=c++11)
