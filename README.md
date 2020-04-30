# BP+OSD (C version)
A C library implementing belief propagation with ordered statistics post-processing for decoding quantum codes.

## Build
To build, run the following commands from the repository root.

```
mkdir build
cd build
cmake ../
make
cd ..
```

## Running the BP+OSD Sim
The C++ source file for the BP+OSD sim script can be found at sim_scripts/bp_osd_decode.cpp
The example program to decode a 16_4_6 MKMN code can be run (from the repository root) with the following command

```
build/bp_osd_decode sim_scripts/mkmn_input_test.json sim_scripts/output
```

The general syntax for running the sim script is as follows

```
./build/bp_osd_decode <path_to_input_file.json> <path_to_output_directory>
``` 

### Input file structure
The bp_osd_decode script takes JSON files as input. An example input file is shown below:
```c
{
  "hx_filename": "codes/random/mkmn/hgp_mkmn_16_4_6_hx.alist",
  "lx_filename": "codes/random/mkmn/hgp_mkmn_16_4_6_lx.alist",
  "code_label": "mkmn_16_4_6",
  "input_seed": 666,
  "bit_error_rate": 0.05,
  "target_runs": 1000,
  "osd_order": 40,
  "max_iter": 50,
  "osd_method": "combination_sweep"
}
```

Where

- "hx_filename": path to hx parity check alist file
- "lx_filename": path to lx logical matrix alist file
- "code_label": whatever you like
- "input_seed": the input seed for the Mersenne twister rng
- "bit_error_rate": the bit error rate
- "target_runs": the number of runs you would like to simulate
- "osd_order": the osd order parameter
- "max_iter": the iterations depth for BP. If this is set to 0, then the value is set to N, where N is the code block length.
- "osd_method": Currenlty two options are implemented. 1) "combination_sweep"; 2) "exhaustive". Both of these are described in the paper.

### Output
The output is saved to the directory specified in the second command line argument. The output file format is JSON and the filename is autogenerated.

## Alist files

Alist files for the Surface and Toric codes can be found in the "codes" directory.

## Using this library in another project

To use this library in another project copy and past the repository into your project root. The library can then be imported by adding the followig to your cmake file:
```c
project(import_example)
add_subdirectory(bp_osd_c)
add_executable(import_example main.cpp)
target_link_libraries(import_example bp_osd)
```