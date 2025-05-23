#!/usr/bin/env make
#
# IOCCC 2015 winning entry - endoh1

################################################################################
#
# IOCCC winning entry code may not work on your system.  What was liked/allowed
# and worked in the past may no longer be liked/allowed or compile/run today.
#
# Bug fixes, corrections and typo fixes are VERY WELCOME.  If you see a problem,
# first check this URL for a list of known bugs and (mis)features of IOCCC entries:
#
#	https://www.ioccc.org/bugs.html
#
# GitHub pull requests are welcome!  Please see the above URL for details.
#
################################################################################
#
# This file is Copyright (c) 2023 by Landon Curt Noll.  All Rights Reserved.
# You are free to share and adapt this file under the terms of this license:
#
#	Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)
#
# For more information, see: https://creativecommons.org/licenses/by-sa/4.0/


#############################
# shell used by this Makefile
#############################
#
SHELL= bash

#######################
# common tool locations
#######################
#
include ../../var.mk

# Common C compiler warnings to silence
#
CSILENCE= -Wno-strict-prototypes -Wno-misleading-indentation \
	  -Wno-multistatement-macros

# Attempt to silence unknown warning option
#
CUNKNOWN= -Wno-unknown-warning-option

# Common C compiler warning flags
#
CWARN= -Wall -Wextra ${CSILENCE} ${CUNKNOWN}

# Compiler standard
#
CSTD= -std=gnu11

# Compiler bit architecture
#
ARCH=

# Defines that are needed to compile
#
CDEFINE=

# Include files that are needed to compile
#
CINCLUDE=

# Optimization
#
OPT= -O3

# Default flags for ANSI C compilation
#
CFLAGS= ${CSTD} ${CWARN} ${ARCH} ${CDEFINE} ${CINCLUDE} ${OPT}

# Libraries needed to build
#
LDFLAGS=

# C compiler to use
#
CC= cc

# Compiler add-ons or replacements for clang only
#
ifeq "$(findstring $(CLANG),${CC})" "$(CLANG)"
#
CSILENCE+= -Wno-comma -Wno-double-promotion -Wno-float-conversion \
	   -Wno-implicit-float-conversion -Wno-implicit-int-conversion \
	   -Wno-missing-prototypes -Wno-missing-variable-declarations \
	   -Wno-poison-system-directories -Wno-redundant-parens -Wno-shadow \
	   -Wno-sign-conversion -Wno-strict-prototypes \
	   -Wno-extra-semi-stmt -Wno-misleading-indentation \
	   -Wno-unsafe-buffer-usage
#
CWARN+= -Weverything
#
endif

# Specific add-ons or replacements for gcc only
#
ifeq "$(findstring $(GCC),${CC})" "$(GCC)"
#
#CSILENCE+=
#
#CWARN+=
#
endif


##############################
# Special Makefile variables for this entry
##############################
#
ENTRY= endoh1
PROG= prog
CSRC= ${PROG}.c
#
OBJ= ${PROG}.o
DATA=
TARGET= ${PROG} gray-scott-1 gray-scott-2 gray-scott-3 gray-scott-4 \
	fitzhugh-nagumo-1 fitzhugh-nagumo-2 ginzburg-landau oregonator
#
ALT_OBJ=
ALT_TARGET=

GRAY_SCOTT_MODEL = -Delta_u="Du*laplacian(u)-u*v*v+F*(1-u)" \
		   -Delta_v="Dv*laplacian(v)+u*v*v-v*(F+K)"

FITZHUGH_NAGUMO_MODEL = -Delta_u="Du*laplacian(u)+u-u*u*u-v" \
			-Delta_v="Dv*laplacian(v)+E*(u-A1*v-A0)"

GINZBURG_LANDAU_MODEL = \
    -Delta_u="Du*laplacian(u)+A*u-G*v+(-B*u+D*v)*(u*u+v*v)" \
    -Delta_v="Dv*laplacian(v)+A*v+G*u+(-B*v-D*u)*(u*u+v*v)"

OREGONATOR_MODEL = -Delta_u="Du*laplacian(u)+(u-u*u-F*v*(u-Q)/(u+Q))/E" \
		   -Delta_v="Dv*laplacian(v)+u-v"


#################
# build the entry
#################
#
all: ${TARGET} ${DATA}
	@${TRUE}

.PHONY: all alt data everything clean clobber

${PROG}: gray-scott-1
	${RM} -f $@
	${CP} gray-scott-1 $@

gray-scott-1: ${CSRC}
	${CC} ${CFLAGS} -o $@ ${CSRC} \
	    ${GRAY_SCOTT_MODEL} -DDu=0.100 -DDv=0.050 -DF=0.035 -DK=0.060 \
	    -DUV_BACKGROUND=1,0 -DUV_DROP=-0.5,0.5 \
	    -DTIMESTEP=1.0 -DSPEED=8 -DRGB=255:128:192 ${LDFLAGS}

gray-scott-2: ${CSRC}
	${CC} ${CFLAGS} -o $@ ${CSRC} \
	    ${GRAY_SCOTT_MODEL} -DDu=0.082 -DDv=0.041 -DF=0.035 -DK=0.063 \
	    -DUV_BACKGROUND=1,0 -DUV_DROP=-0.5,0.5 \
	    -DTIMESTEP=1.0 -DSPEED=8 -DRGB=128:255:192 ${LDFLAGS}

gray-scott-3: ${CSRC}
	${CC} ${CFLAGS} -o $@ ${CSRC} \
	    ${GRAY_SCOTT_MODEL} -DDu=0.100 -DDv=0.050 -DF=0.030 -DK=0.060 \
	    -DUV_BACKGROUND=1,0 -DUV_DROP=-0.5,0.5 \
	    -DTIMESTEP=1.0 -DSPEED=8 -DRGB=255:128:192 ${LDFLAGS}

gray-scott-4: ${CSRC}
	${CC} ${CFLAGS} -o $@ ${CSRC} \
	    ${GRAY_SCOTT_MODEL} -DDu=0.040 -DDv=0.020 -DF=0.012 -DK=0.050 \
	    -DUV_BACKGROUND=1,0 -DUV_DROP=-0.5,0.5 \
	    -DTIMESTEP=1.0 -DSPEED=8 -DRGB=192:192:255 ${LDFLAGS}

fitzhugh-nagumo-1: ${CSRC}
	${CC} ${CFLAGS} -o $@ ${CSRC} \
	    ${FITZHUGH_NAGUMO_MODEL} -DDu=1 -DDv=3 -DA0=-0.1 -DA1=2 -DE=0.06 \
	    -DUV_BACKGROUND=0 -DUV_DROP=1,0 \
	    -DTIMESTEP=0.050 -DSPEED=80 -DRGB=128:192:255 ${LDFLAGS}

fitzhugh-nagumo-2: ${CSRC}
	${CC} ${CFLAGS} -o $@ ${CSRC} \
	    ${FITZHUGH_NAGUMO_MODEL} -DDu=1 -DDv=10 -DA0=0.0 -DA1=1 -DE=1 \
	    -DUV_BACKGROUND=0 -DUV_DROP=1,1 \
	    -DTIMESTEP=0.005 -DSPEED=20 -DRGB=128:192:255 ${LDFLAGS}

ginzburg-landau: ${CSRC}
	${CC} ${CFLAGS} -o $@ ${CSRC} \
	    ${GINZBURG_LANDAU_MODEL} -DDu=0.1 -DDv=0.1 -DA=0.0625 -DB=1 -DD=1.05 -DG=0.0625 \
	    -DUV_BACKGROUND=-0.1 -DUV_DROP=0,1 \
	    -DTIMESTEP=0.60 -DSPEED=40 -DRGB=128:255:128 ${LDFLAGS}

oregonator: ${CSRC}
	${CC} ${CFLAGS} -o $@ ${CSRC} \
	    ${OREGONATOR_MODEL} -DDu=0.01 -DDv=0.01 -DE=0.1 -DF=1.4 -DQ=0.002 \
	    -DUV_BACKGROUND=0 -DUV_DROP=0.05,0.2 \
	    -DTIMESTEP=0.001 -DSPEED=80 -DRGB=128:192:255 ${LDFLAGS}

# alternative executable
#
alt: data ${ALT_TARGET}
	@${TRUE}

# data files
#
data: ${DATA}
	@${TRUE}

# both all and alt
#
everything: all alt
	@${TRUE}


###############
# utility rules
###############
#
clean:
	${RM} -f ${OBJ} ${ALT_OBJ}

clobber: clean
	${RM} -f ${TARGET} ${ALT_TARGET}
	${RM} -rf *.dSYM


######################################
# optional include of 1337 hacker rulz
######################################

-include 1337.mk ../1337.mk ../../1337.mk
