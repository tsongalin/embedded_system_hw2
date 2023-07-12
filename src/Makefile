#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk


# Platform Overrides
PLATFORM = 

# Architectures Specific Flags
# Flags shared by HOST and MSP432
TARGET = c1m2
DEP = -MD

# if statement to customize HOST/MSP432 options
ifeq ($(PLATFORM),HOST)
CC = gcc
D = HOST
I = -I$(INCLUDES)
SIZE = size
else ifeq ($(PLATFORM),MSP432)
CPU = -mcpu=cortex-m4
ARCH = -mthumb -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16
SPECS = --specs=nosys.specs
LINKER_FILE = -T ../msp432p401r.lds
CC = arm-none-eabi-gcc
D = MSP432
I1 = $(INCLUDES1)
I2 = $(INCLUDES2)
I = -I$(INCLUDES) -I$(I1) -I$(I2)
SIZE = arm-none-eabi-size
endif

### Generate list of different object files
# pre-processes
OBJS1 = $(SOURCES:.c=.i)
# compiler
OBJS2 = $(SOURCES:.c=.asm)
# Dependency files used in clean process 
OBJS3 = $(SOURCES:.c=.d)
# assembler
OBJS = $(SOURCES:.c=.o)

%.i : %.c
	$(CC) -E $< $(CFLAGS) -D$(D) $(I) $(CPU) $(DEP) $(ARCH) $(SPECS) -o $@

%.asm : %.c
	$(CC) -S $< $(CFLAGS) -D$(D) $(I) $(CPU) $(DEP) $(ARCH) $(SPECS) -o $@

%.o : %.c
	$(CC) -c $< $(CFLAGS) -D$(D) $(I) $(CPU) $(DEP) $(ARCH) $(SPECS) -o $@
# Compiler Flags and Defines

LD = arm-none-eabi-ld
LDFLAGS = -Wl,-Map=$(TARGET).map
CFLAGS = -Wall -Werror -g -O0 -std=c99
CPPFLAGs = 
#CC = gcc

.PHONY: compile-all
compile-all: $(OBJS)

%.o : %.c
	$(CC) -c $< $(CFLAGS) -D$(D) $(I) $(CPU) $(DEP) $(ARCH) $(SPECS) -o $@

.PHONY: build
build: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) -D$(D) $(I) $(CPU) $(DEP) $(ARCH) $(SPECS) $(LDFLAGS) $(LINKER_FILE) $(LDFLAGS) -o $@
	$(SIZE) $@

.PHONY: clean
clean:
	rm -rf $(OBJS) $(OBJS1) $(OBJS2) $(OBJS3) $(TARGET).out $(TARGET).map
