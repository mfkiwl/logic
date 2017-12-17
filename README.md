Logic
=====

CMake, SystemVerilog and SystemC utilities for creating, building and testing
RTL projects for FPGAs and ASICs.

Includes:

  * CMake utilities for rapid building and testing RTL projects
  * SystemVerilog modules for creating high quality RTL projects
  * Modern C++ framework for UVM with SystemC for creating high quality and
    performance efficient tests for RTL projects

Requirements
------------

These 3rd party tools and libraries are required. They must be installed to
build logic library:

  * [CMake](https://cmake.org/) - build, test and package project
  * [SystemC 2.3.2](http://accellera.org/downloads/standards/systemc) - SystemC C++ library
  * [UVM-SystemC 1.0](http://www.eda.org/activities/working-groups/systemc-verification) - UVM for SystemC
  * [SystemC Verification 2.0.1](http://accellera.org/downloads/standards/systemc) - SystemC data randomization

These 3rd party tools and libraries are optional. They must be installed to
build and run tests:

  * [Verilator](https://www.veripool.org/wiki/verilator/) - simulator, lint and coverage tool
  * [GoogleTest](https://github.com/google/googletest) - C++ unit test framework
  * [SVUnit](http://agilesoc.com/open-source-projects/svunit/) - SystemVerilog unit test framework

These 3rd party tools and libraries are optional:

  * [Intel FPGA Quartus](https://www.altera.com/downloads/download-center.html) - synthesis tool for Intel FPGAs
  * [Xilinx Vivado](https://www.xilinx.com/products/design-tools/vivado.html) - synthesis tools for Xilinx FPGAs
  * [Open Verification Library](http://accellera.org/activities/working-groups/ovl) - library of assertion checkers
  * [Natural Docs](http://www.naturaldocs.org/) - code documentation generator
  * [GTKWave](http://gtkwave.sourceforge.net/) - waveform viewer
  * [WaveDrom](http://wavedrom.com/) - digital timing diagram

Environment setup guides:

  * [Environment setup for Linux](doc/environment-setup-linux.md)

Workspace
---------

  * README.md       - this read me file in MarkDown format
  * LICENSE         - license file
  * CMakeLists.txt  - CMake root script for building and testing project
  * doc             - configuration files for code documentation generator
  * rtl             - RTL source files
  * src             - C++ source files
  * include         - C++ include headers
  * tests           - unit tests and verification tests in SystemC using
                      Google Test or UVM and SystemVerilog using SVUnit
  * cmake           - additional CMake scripts for building project
  * scripts         - additional scripts in TCL or Python for building project

Build
-----

Clone project repository:

    git clone git@github.com:tymonx/logic.git

Change current location to project directory:

    cd logic

Create build directory:

    mkdir build

Change current location to build directory:

    cd build

Create build scripts using CMake:

    cmake ..

Build project using CMake:

    cmake --build . --target all

Or build project using make:

    make -j`nproc`

Documentation
-------------

To build documentation:

    cmake --build . target doc

Built HTML documentation can be found in:

    doc/html

To view HTML documentation, open it using web browser:

    <WEB_BROWSER> doc/html/index.html

Tests
-----

Run all unit tests:

    ctest

Run only unit tests for AXI4-Stream:

    ctest -R axi4_stream

All waveforms generated from unit tests are located in:

    build/output

All unit tests logs are stored in:

    build/Testing/Temporary/LastTest.log

Verilator Coverage
------------------

Run Verilator coverage after running all tests:

    cmake --build . --target verilator-coverage

Verilator analysis
------------------

Enable Verilator analysis:

    add_hdl_source(<hdl-module-filename>
        VERILATOR_ANALYSIS
            TRUE
    )

Run Verilator analysis for `<hdl-module-name>`:

    make verilator-analysis-<hdl-module-name>

Rin Verilator analysis for all HDL modules:

    make verilator-analysis-all

Creating Intel FPGA Quartus project
-----------------------------------

Use `add_quartus_project()` function to create Quartus project:

    add_quartus_project(<top_level_entity>)

Quartus project will be created under:

    quartus/<top_level_entity>

RTL analysis and elaboration in `Intel FPGA Quartus` for top level entity:

    cmake --build . --target quartus-analysis-<top_level_entity>

RTL compilation in `Intel FPGA Quartus` for top level entity:

    cmake --build . --target quartus-compile-<top_level_entity>

RTL analysis and elaboration in `Intel FPGA Quartus` for all top level
entities:

    cmake --build . --target quartus-analysis-all

RTL compilation in `Intel FPGA Quartus` for all top level entities:

    cmake --build . --target quartus-compile-all

Creating Xilinx Vivado project
------------------------------

Use `add_vivado_project()` function to create Vivado project:

    add_vivado_project(<top_level_entity>)

Quartus project will be created under:

    vivado/<top_level_entity>

RTL analysis and elaboration in `Xilinx Vivado` for top level entity:

    cmake --build . --target vivado-compile-<top_level_entity>

RTL analysis and elaboration in `Xilinx Vivado` for all top level
entities:

    cmake --build . --target vivado-compile-all

Using with other CMake projects
-------------------------------

Change current location to another RTL project root directory:

    cd <rtl_project_root_directory>

Clone and add logic repository to RTL project as git submodule:

    git submodule add git@github.com:tymonx/logic.git

Add these lines to CMakeLists.txt root file:

    set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH}
        ${CMAKE_CURRENT_LIST_DIR}/logic/cmake
    )

    include(AddLogic)

    enable_testing()

    add_subdirectory(logic)
