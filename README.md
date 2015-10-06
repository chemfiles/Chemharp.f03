# Fortran binding for the Chemharp library

This repository contains the fortran 2003 binding to the [Chemharp](https://github.com/Luthaf/Chemharp) library.

It is known to compile with gfortran 4.9.2 and ifort 14. If you manage to build this code
with any other compiler, please let me know so that I can add it here.

## Getting started

You will need to install all the dependencies of the main C++ lib, as [documented here](http://chemharp.readthedocs.org/en/latest/installation.html). Then run

```bash
mkdir build
cd build
cmake ..  # you can use any of the Chemharp C++ options here
make install
```

## Usage example

Here is a simple example of how the usage feels in Fortran:

```fortran
program example
    use chemharp
    use iso_fortran_env, only: int64, real32

    implicit none
    type(chrp_trajectory) :: trajectory
    type(chrp_frame) :: frame
    integer(int64) :: natoms
    real(real32), dimension(:, :), allocatable :: positions
    integer :: status

    call trajectory%open("filename.xyz", "r", status=status)
    if status /= 0 stop "Error while opening file"
    call frame%init(0)

    call file%read(frame, status=status)
    if status /= 0 stop "Error while reading file"

    call frame%atoms_count(natoms)
    write(*, *) "There are ", natoms, "atoms in the frame"

    allocate(positions(3, natoms))

    call frame%positions(positions, natoms)

    ! Do awesome things with the positions here !

    call frame%free()
    call trajectory%close()
}
```

You can find other examples in the `examples` directory.

## Bug reports, feature requests

Please report any bug you find and any feature you may want as a [github issue](https://github.com/Luthaf/Chemharp.f03/issues/new).
