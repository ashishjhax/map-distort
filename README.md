# map-distort
Visualise data on a map by distorting the regions to make their areas proportional to the data values

**Example**
![Sample](/sample/area_population.png "Comparing the area and population of the states of India")

## Sample data

Some example data for India is given, including only the states and Delhi. Jammu & Kashmir is taken as the pre-bifurcation state.

* [population.dat](/data/population.dat): Population of states in the 2011 census
* [popincrease.dat](/data/popincrease.dat): Population increase from 2001-11
* [LSseats.dat](/data/LSseats.dat): Lok Sabha seats
* [gdp.dat](/data/gdp.dat): Gross state domestic product (2018) from http://mospi.nic.in
* [covidcases.dat](/data/covidcases.dat): Confirmed COVID cases as of 21 May, 2021 from https://www.covid19india.org/
* [coviddeaths.dat](/data/coviddeaths.dat): COVID deaths

## Usage

First, build the program using
```
g++-11 -O3 -W -Wall -Wno-unused-result -std=c++2a    weighted_distort.cpp   -o weighted_distort
```
You would need a modern C++ compiler.

To modify a map you can then use
```
echo map_input.dat data_input.dat num_rounds | ./weighted_distort > map_output.dat
```
* `map_input.dat` is the input map with one region per line. Unfortunately, the program only works with regions that are **pseudo**-svg paths - that is, piecewise linear paths consisting of only "m, h, v, l" elements and commas between coordinates replaced by points. See [states_boundary.map](/data/states_boundary.map) for an example.
* `data_input.dat` contains the data of the weight, one line per region. Regions with zero weight are ignored
* `num_rounds` is the number of rounds to run the region relaxation for. I usually take around 10K rounds, and it stops earlier if every state is resized to 1% accuracy
* `map_output.dat` is the output file which can be easily processed by gnuplot, with `x y region_id` per line and a blank line between two subregions


Sample images can be generated by running
```
bash process.sh
```
This step requires gnuplot and imagemagick (convert) to be callable directly.
