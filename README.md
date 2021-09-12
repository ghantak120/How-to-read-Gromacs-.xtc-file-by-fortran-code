# How-to-read-Gromacs-.xtc-file-by-fortran-code

Just keep the xdrf directory in your home directory

Run your fortran code by using

f95 /Path of your xdrf directory/./xdrf/xtciof.o /Path of your xdrf directory/./xdrf/libxdrf.a -lm' Program.F90

Here is the simple reading part of .xtc file 


Here natoms is the number of atom in your system which will read from .xtc file

maxatom is the number of atoms defined by you

nread=0                                 ! #of frame read

nused=0                                 ! #of frame used for calculation

do nput=1,nxtc                          ! .xtc loop start

call xdrfopen(iunit,ifile(ibk,nput),"r",ret)

do ii=1,nframes                         ! frame loop start Define initially how many no of frames you want to use

call readxtc(iunit,natoms,step,time,box,x,prec,ret)

if(maxatom /= natoms)then

write(*,*) 'Natoms and maxatoms not matching'

stop

endif

nread=nread+1

if(mod(nread,nskip) /= 0)cycle

nused=nused+1

bx=box(1)

by=box(5)

bz=box(9)

j=0

do i=1,maxatom*3,3                         ! Atom loop start

j=j+1                                      ! 3 for coordinate along 3 axis

x(j)=x(i)

y(j)=x(i+1)

z(j)=x(i+2)

enddo                                      ! Atom loop over

