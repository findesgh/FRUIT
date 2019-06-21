add_library(fruit SHARED fruit.f90)

if(MPI_Fortran_FOUND)
  add_library(fruit_mpi SHARED fruit_mpi.f90)
  add_dependencies(fruit_mpi fruit)
endif(MPI_Fortran_FOUND)
