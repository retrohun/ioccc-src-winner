#!/usr/bin/env make
#
# IOCCC 2004 winning entry - kopczynski

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
CSILENCE= -Wno-error -Wno-implicit-function-declaration -Wno-implicit-int \
	  -Wno-main -Wno-parentheses -Wno-deprecated-non-prototype \
	  -Wno-unused-parameter -Wno-builtin-declaration-mismatch

# Attempt to silence unknown warning option
#
CUNKNOWN= -Wno-unknown-warning-option

# Common C compiler warning flags
#
CWARN= -Wall -Wextra ${CSILENCE} ${CUNKNOWN}

# Compiler standard
#
CSTD= -std=gnu99

# Compiler bit architecture
#
ARCH=

# Defines that are needed to compile
#
CDEFINE= -Dif=while

# Include files that are needed to compile
#
CINCLUDE=

# Optimization
#
# We DISABLE the optimiser because having it enabled BREAKS the program.
OPT= -O0

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
CSILENCE+= -Wno-conditional-uninitialized -Wno-pedantic -Wno-poison-system-directories \
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
ENTRY= kopczynski
PROG= ${ENTRY}
#
OBJ= ${PROG}.o
DATA= kopczynski-10 kopczynski-11 kopczynski-8a kopczynski-8b \
	kopczynski-8c kopczynski-9
TARGET= ${PROG}
#
ALT_OBJ= ${PROG}.alt.o
ALT_TARGET= ${PROG}.alt


#################
# build the entry
#################
#
all: data ${TARGET}
	@${TRUE}

.PHONY: all alt data everything clean clobber

# NOTE: We FORCE the use of -O0 even if someone overrides it because this entry
# will not work with it enabled.
${PROG}: ${PROG}.c
	${CC} ${CFLAGS} -O0 $< -o $@ ${LDFLAGS}

# alternative executable
#
alt: data ${ALT_TARGET}
	@${TRUE}

${PROG}.alt: ${PROG}.alt.c
	${CC} ${CFLAGS} $< -o $@ ${LDFLAGS}

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
