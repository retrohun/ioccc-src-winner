#!/usr/bin/env make
#
# IOCCC 1992 winning entry - buzzard.1

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
CSILENCE= -Wno-error -Wno-implicit-function-declaration -Wno-pedantic \
	  -Wno-unused-parameter -Wno-deprecated-non-prototype -Wno-implicit-int \
	  -Wno-builtin-declaration-mismatch -Wno-missing-parameter-type \
          -Wno-return-type

# Attempt to silence unknown warning option
#
CUNKNOWN= -Wno-unknown-warning-option

# Common C compiler warning flags
#
CWARN= -Wall -Wextra ${CSILENCE} ${CUNKNOWN}

# Compiler standard
#
CSTD= -std=gnu90

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
CSILENCE+= -Wno-missing-variable-declarations -Wno-poison-system-directories \
	   -Wno-unused-macros -Wno-unsafe-buffer-usage
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
ENTRY= buzzard.1
PROG= ${ENTRY}
#
OBJ= ${PROG}.o
DATA=
TARGET= ${PROG}
#
ALT_OBJ=
ALT_TARGET=

NOINCLUDE= ${SED} -e 's/^.\([	 ]*\)include/%include/'
SPACECLEAN= ${SED} -e 's/\([^	 ]\)\([	 ]\)[	 ][	 ]*/\1\2/g'
CPPCLEAN= ${GREP} -v '^. [0-9][0-9]*'


#################
# build the entry
#################
#
all: data ${TARGET}
	@${TRUE}

.PHONY: all alt data everything clean clobber

${PROG}: ${PROG}.c
	${CC} ${CFLAGS} $< -o $@ ${LDFLAGS}

# alternative executable
#
alt: data ${ALT_TARGET}
	@${TRUE}

babble.cppcb: ${PROG}.c
	${RM} -f tmp
	${NOINCLUDE} ${PROG}.c | \
	    ${CPP} ${CPPFLAGS} | \
	    ${CPPCLEAN} | \
	    ${UNIQ} | \
	    ${SPACECLEAN} > tmp 2>&1
	@if which "${GINDENT}" >/dev/null 2>&1; then \
	    echo ${RM} -f $@; \
	    ${RM} -f $@; \
	    echo "${GINDENT} < tmp > $@"; \
	    ${GINDENT} < tmp > $@; \
	elif which "${INDENT}" >/dev/null 2>&1; then \
	    echo ${RM} -f $@; \
	    ${RM} -f $@; \
	    echo "${INDENT} < $< > $@"; \
	    ${INDENT} < tmp > $@; \
	else \
	    echo ${RM} -f $@; \
	    ${RM} -f $@; \
	    echo "${MV} -f tmpbabble.cppcb $@"; \
	    ${MV} -f tmp $@; \
	fi
	${RM} -f tmp

babble: ${PROG}
	${RM} -f $@
	${LN} $< $@

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
	${RM} -f babble.cppcb babble


######################################
# optional include of 1337 hacker rulz
######################################

-include 1337.mk ../1337.mk ../../1337.mk
