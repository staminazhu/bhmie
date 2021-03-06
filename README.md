About
=====

This is a wrapper program for the dust scattering property calculation code
by C.F. Bohren and D. Huffman (improved by B. Draine).

Installation
============

Requirements
------------

A recent Fortran compiler.

Downloading
-----------

To download, use

    git clone http://github.com/hyperion-rt/bhmie.git
    cd bhmie
    git submodule init
    git submodule update

Updating
--------

To update, go inside the dust/ directory, and use

    git pull
    git submodule update

Compiling
---------

To install, type:

    ./configure
    make
    make install

which will create bin/bhmie. The configure script will automatically pick the
Fortran compiler. To install in a user-defined path, use e.g.:

    ./configure --prefix=$HOME/usr
    make
    make install

Running
=======

To use, type:

    bin/bhmie parameter_file

Examples are included inside the examples/ directory.

Parameters
==========

The parameter file contains, in the following order:

- the prefix for the output files
- the file format, which can be:
    - 1 = single file with wavelength, extinction and scattering cross-sections,
        opacity to extinction, the average cos(theta), and the maximum linear polarization.
    - 2 = separate files for the wavelength (.wav), scattering angles (.mu),
        albedo (.alb), opacity to extinction (.chi), <cos(theta)> (.g), and the
        scattering matrix elements (.f11, .f12, .f33, .f34)
    - 3 = one file per wavelength, contains the scattering matrix elements
- the minimum grain size to use for the calculations (in microns)
- the maximum grain size to use for the calculations (in microns)
- the number of grain sizes to use for the calculations
- the number of main scattering angles between 0 and 90 degrees
- the number of extra scattering angles close to theta=0 to resolve strong peaks
- the number of chemical components
- the gas-to-dust ratio to assume for the opacity (set this to zero if you
  don't care about gas)
- the minimum, maximum, and number of wavelengths (microns)

After these parameters should be the following parameters, repeated for each
component:

- a blank line (or separator)
- the fraction of the mass in the component
- the density of the grains (in g/cm^3)
- the file containing the refractive indices
- the type of dust size distribution
- the parameters of the dust size distribution

There are three types of size distributions supported:

* 'power' - a power-law distribution:

        n(a) ~ a^apower

  The parameters to specify are:

        amin amax apower

  where amin and amax are the minimum and maximum size for the distribution,
  and apower is the power of the size distribution.

* 'ped' - a power-law distribution with exponential dropoff:

        n(a) ~ a^apower * exp(a/aturn)

  The parameters to specify are:

        amin aturn apower

  where amin and aturn are the minimum and turn-off size for the distribution,
  and apower is the power of the size distribution.

* 'table' - a table containing the relative number of grains as a function of
  size. This should be given as a two column space-delimited ASCII table where
  the first column is the grain size in microns, and the second column is the
  relative number of grains (the distribution is normalized by the code)

Notes
=====

At this time, all components for a given computation should contain the same
number of wavelengths, and the values should be given at the same wavelengths.
