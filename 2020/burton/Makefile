#!/usr/bin/env make
#
# IOCCC 2020 winning entry - burton

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
CSILENCE=

# Attempt to silence unknown warning option
#
# Example: CUNKNOWN= -Wno-unknown-warning-option
#
CUNKNOWN=

# Common C compiler warning flags
#
CWARN= -Wall -Wextra ${CSILENCE} ${CUNKNOWN}

# Compiler standard
#
CSTD= -std=gnu17

# Compiler bit architecture
#
ARCH=

# Defines that are needed to compile
#
CDEFINE=

# Include files that are needed to compile
#
CINCLUDE= -include stdlib.h -include stdio.h

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
CSILENCE+= -Wno-poison-system-directories -Wno-unsafe-buffer-usage
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
ENTRY= burton
PROG= prog
#
OBJ= ${PROG}.o ${PROG}_le.o ${PROG}_be.o
DATA= correct
TARGET= ${PROG} ${PROG}_le ${PROG}_be
#
ALT_OBJ=
ALT_TARGET=

# magic define parameters for Little Endian machines
#
B=6945503773712347754LL
I=5859838231191962459LL
T=0
S=7

# magic define parameters for Big Endian machines
#
_B=7091606191627001958LL
_I=6006468689561538903LL
_T=1
_S=0


#################
# build the entry
#################
#
all: data ${TARGET}
	@${TRUE}

.PHONY: all alt data everything clean clobber

${PROG}: ${PROG}.c
	@echo "NOTE: this entry will crash on no arg specified."
	${CC} ${CFLAGS} -DB=${B} -DI=${I} -DT=${T} -DS=${S} $< -o $@ ${LDFLAGS}

${PROG}_le: ${PROG}.c
	${CC} ${CFLAGS} -DB=${B} -DI=${I} -DT=${T} -DS=${S} $< -o $@ ${LDFLAGS}

${PROG}_be: ${PROG}.c
	${CC} ${CFLAGS} -DB=${_B} -DI=${_I} -DT=${_T} -DS=${_S} $< -o $@ ${LDFLAGS}

test: check_le.sh check_be.sh prog_le prog_be correct
	./check_le.sh ./prog_le | ${DIFF} -sbw - correct
	./check_be.sh ./prog_be | ${DIFF} -sbw - correct

# alternative executable
#
alt: data ${ALT_TARGET}
	@${TRUE}

# data files
#
data: ${DATA}
	@${TRUE}


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
	${RM} -f ${PROG}_be ${PROG}_le


######################################
# optional include of 1337 hacker rulz
######################################

-include 1337.mk ../1337.mk ../../1337.mk
