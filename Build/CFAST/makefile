# Makefile for CFAST, Version 7

# To use this makefile, select the appropriate compiler and OS from the list below. For example, to
# compile the 64 bit version of CFAST under Linux using the Intel compilers type:
#
# make intel_linux_64

# A make utility for windows may be downloaded from http://gnuwin32.sourceforge.net/packages/make.htm

# The variable VPATH defines where the source code is relative to the current directory

VPATH = ../../../Source/CFAST
VERSION=Test

ifeq ($(shell echo "check_quotes"),"check_quotes")
# windows
  GIT_HASH := $(shell ..\..\scripts\githash)
  GIT_DATE := $(shell ..\..\scripts\gitlog)
  BUILD_DATE := $(shell date /t) $(shell time /t)
  GIT_BRANCH := $(shell ..\..\scripts\gitbranch)
else
# linux/osx
  GIT_HASH := $(shell git describe --long --dirty)
  GIT_DATE := $(shell git log -1 --format=%cd)
  BUILD_DATE := $(shell date "+%b %d, %Y  %T")
  GIT_BRANCH := $(shell git rev-parse --abbrev-ref HEAD)
endif

GITINFO_BASE=-DGITHASH_PP=\"$(GIT_HASH)-$(GIT_BRANCH)\" -DGITDATE_PP=\""$(GIT_DATE)\""  -DVERSION_PP=\""$(VERSION)\""
GITINFO=-fpp $(GITINFO_BASE) -DBUILDDATE_PP=\""$(BUILD_DATE)\""
GITINFOGNU=-cpp $(GITINFO_BASE) -DBUILDDATE_PP=\""$(BUILD_DATE)\""
PREPROCESS=-Dpp_FIRE


# Definition of the object variables

objf = prec.o cfast_parameters.o cfast_structures.o cfast_data.o convection.o keyboard_int.o \
       srev.o cfastexit.o utilities.o ssHeaders.o outputdebug.o compartments.o conduction.o flowfan.o flowhorizontal.o flowvertical.o fire.o \
       numerics.o radiation.o target.o outputsmv.o outputspreadsheet.o output.o solve.o initialization.o input_namelist.o input.o \
       cfast.o 

objwin = $(objf:.o=.obj)

objfgnu = prec.o cfast_parameters.o cfast_structures.o cfast_data.o convection.o keyboard_gnu.o \
       srev.o cfastexit.o utilities.o ssHeaders.o outputdebug.o compartments.o conduction.o flowfan.o flowhorizontal.o flowvertical.o fire.o \
       numerics.o radiation.o target.o outputsmv.o outputspreadsheet.o output.o solve.o initialization.o  input_namelist.o input.o cfast.o  


# General Purpose Rules

no_target:
	@echo \******** You did not specify a make target \********
	@echo Please read the comments at the top of the makefile

setup:
%.o : %.mod

setup_win:
%.obj : %.mod

.SUFFIXES: .f90 .f .o .obj

.f.o:
	$(FCOMPL) -c $(FFLAGS) $<
.f.obj:
	$(FCOMPL) -c $(FFLAGS) $<
.f90.o:
	$(FCOMPL) -c $(FFLAGS) $<
.f90.obj:
	$(FCOMPL) -c $(FFLAGS) $<

# run make_fds.sh or make_fds.bat scripts in individual build directories

# ------------- Windows -------------------

# gnu windows
gnu_win_64 : FFLAGS = -O2 -finit-local-zero -ffpe-trap=invalid,zero,overflow -fbacktrace $(GITINFOGNU) $(PREPROCESS)
gnu_win_64 : FCOMPL = gfortran
gnu_win_64 : LFLAGS = 
gnu_win_64 : obj = cfast7_win_64
gnu_win_64 : $(objfgnu)
	$(FCOMPL) $(FFLAGS) -o $(obj) $(objfgnu)

# gnu windows debug
gnu_win_64_db : FFLAGS = -O0 -Og -finit-local-zero -ffpe-trap=invalid,zero,overflow -fbacktrace $(GITINFOGNU) $(PREPROCESS)
gnu_win_64_db : FCOMPL = gfortran
gnu_win_64_db : LFLAGS = 
gnu_win_64_db : obj = cfast7_win_64_db
gnu_win_64_db : $(objfgnu)
	$(FCOMPL) $(FFLAGS) -o $(obj) $(objfgnu)

# 64 bit
intel_win_64 : FFLAGS = /O2 /fp:consistent /fpconstant /traceback /stand:f08  $(GITINFO) $(PREPROCESS)
intel_win_64 : FCOMPL = ifx
intel_win_64 : obj = cfast7_win_64
intel_win_64 : setup_win $(objwin)
	$(FCOMPL) -o $(obj) $(FFLAGS) /F1000000000 $(objwin) 

intel_win_64_db : FFLAGS = /Qtrapuv /Qinit:snan /debug:full /Z7 /Od /warn:all /warn:nointerfaces /Qtrapuv /fp:consistent /fpconstant /traceback /check:all /stand:f18 $(GITINFO) $(PREPROCESS)
intel_win_64_db : FCOMPL = ifx
intel_win_64_db : obj = cfast7_win_64_db
intel_win_64_db : setup_win $(objwin)
	$(FCOMPL) -o $(obj) $(FFLAGS) /F1000000000 $(objwin)  	

# gnu linux_64

gnu_linux_64 : FFLAGS = -O0 -finit-local-zero -ffpe-trap=invalid,zero,overflow -fbacktrace $(GITINFOGNU) $(PREPROCESS)
gnu_linux_64 : FCOMPL = gfortran
gnu_linux_64 : LFLAGS = 
gnu_linux_64 : obj = cfast7_linux_64
gnu_linux_64 : $(objfgnu)
	$(FCOMPL) $(FFLAGS) -o $(obj) $(objfgnu)

# gnu linux debug
gnu_linux_64_db : FFLAGS = -O0 -Og -finit-local-zero -ffpe-trap=invalid,zero,overflow -fbacktrace $(GITINFOGNU) $(PREPROCESS)
gnu_linux_64_db : FCOMPL = gfortran
gnu_linux_64_db : LFLAGS = 
gnu_linux_64_db : obj = cfast7_linux_64_db
gnu_linux_64_db : $(objfgnu)
	$(FCOMPL) $(FFLAGS) -o $(obj) $(objfgnu)

# 64 bit
intel_linux_64 : FFLAGS = -m64 -O0 -static -gen-interfaces -warn interfaces -traceback $(GITINFO) $(PREPROCESS)
intel_linux_64 : LFLAGS = -static-intel
intel_linux_64 : FCOMPL = ifx
intel_linux_64 : obj = cfast7_linux_64
intel_linux_64 : $(objf)
	$(FCOMPL) $(LFLAGS) -o $(obj) $(objf)

#intel_linux_64_db : FFLAGS = -m64 -check all -ftrapuv -warn unused -gen-interfaces -warn interfaces -O0 -WB -traceback -g -fpe0 -fp-model strict -debug full
intel_linux_64_db : FFLAGS = -m64 -check -warn unused -O0 -init=snan -static -WB -traceback -g -fpe0 -fp-model strict $(GITINFO) $(PREPROCESS)
intel_linux_64_db : LFLAGS = -static-intel
intel_linux_64_db : FCOMPL = ifx
intel_linux_64_db : obj = cfast7_linux_64_db
intel_linux_64_db : $(objf)
	$(FCOMPL) $(LFLAGS) -o $(obj) $(objf)

# ------------- OSX -------------------

# gnu osx
gnu_osx_64 : FFLAGS = -O2 -finit-local-zero -ffpe-trap=invalid,zero,overflow -ffpe-summary=none -fbacktrace $(GITINFOGNU) $(PREPROCESS)
gnu_osx_64: FCOMPL = gfortran
gnu_osx_64 : LFLAGS = 
gnu_osx_64 : obj = cfast7_osx_64
gnu_osx_64 : $(objfgnu)
	$(FCOMPL) $(FFLAGS) -o $(obj) $(objfgnu)

# gnu osx_db
gnu_osx_64_db : FFLAGS = -O0 -Og -finit-local-zero -ffpe-trap=invalid,zero,overflow -fbacktrace $(GITINFOGNU) $(PREPROCESS)
gnu_osx_64_db : FCOMPL = gfortran
gnu_osx_64_db : LFLAGS = 
gnu_osx_64_db : obj = cfast7_osx_64_db
gnu_osx_64_db : $(objfgnu)
	$(FCOMPL) $(FFLAGS) -o $(obj) $(objfgnu)

# 64 bit
intel_osx_64 : FFLAGS = -fpp -O2 -save -zero -m64 -traceback -heap-arrays -static-intel -fp-model strict $(GITINFO) $(PREPROCESS)
intel_osx_64 : FCOMPL = ifx
intel_osx_64 : obj = cfast7_osx_64
intel_osx_64 : $(objf)
	$(FCOMPL) $(FFLAGS) -mmacosx-version-min=10.7 -o $(obj) $(objf)

intel_osx_64_db : FFLAGS = -fpp -check -warn unused -O0 -save -zero -m64 -g -traceback -save-temps -fpe:0 -fp-model strict -fp-stack-check -FR -WB $(GITINFO) $(PREPROCESS)
intel_osx_64_db : FCOMPL = ifx
intel_osx_64_db : obj = cfast7_osx_64_db
intel_osx_64_db : $(objf)
	$(FCOMPL) $(FFLAGS) -mmacosx-version-min=10.7 -o $(obj) $(objf)
#*** End Compiler Specific Rules ***

# *** Object Dependencies ***


cfast.o : utilities.o cfastexit.o initialization.o input_namelist.o input.o numerics.o \
              output.o outputsmv.o outputspreadsheet.o ssHeaders.o \
              compartments.o conduction.o convection.o radiation.o target.o \
              fire.o flowfan.o flowhorizontal.o flowvertical.o solve.o


cfast.obj : utilities.obj cfastexit.obj initialization.obj input_namelist.o input.obj numerics.obj \
              output.obj outputsmv.obj outputspreadsheet.obj ssHeaders.obj \
              compartments.obj conduction.obj convection.obj radiation.obj target.obj \
              fire.obj flowfan.obj flowhorizontal.obj flowvertical.obj solve.o

#-------------- force compilation of srev.c -----------

srev.o:	.FORCE
srev.obj:	.FORCE

.FORCE:

output.o:	.FORCE2
output.obj:	.FORCE2

.FORCE2:

#*** Clean Target to remove Object and Module files ***

.PHONY : clean
clean:
	-rm -f *.o *.mod *.obj 

.PHONY : winclean
winclean:
	-erase *.mod *.obj 

