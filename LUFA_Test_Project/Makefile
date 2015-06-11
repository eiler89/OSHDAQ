MCU=atxmega32e5

PROJECTNAME=dali_master

# Source files
# List C/C++/Assembly source files:
# (list all files to compile, e.g. 'a.c b.cpp as.S'):
# Use .cc, .cpp or .C suffix for C++ files, use .S 
# (NOT .s !!!) for assembly source code files.

PRJSRC += \
src/ASF/xmega/drivers/cpu/ccp.S \
src/ASF/xmega/drivers/nvm/nvm_asm.S \
src/ASF/common/boards/user_board/init.c \
src/ASF/common/services/clock/xmega/sysclk.c \
src/ASF/common/services/ioport/xmega/ioport_compat.c \
src/ASF/common/services/sleepmgr/xmega/sleepmgr.c \
src/ASF/xmega/drivers/ac/ac.c \
src/ASF/xmega/drivers/edma/edma.c \
src/ASF/xmega/drivers/nvm/nvm.c \
src/ASF/xmega/drivers/tc45/tc45.c \
src/ASF/xmega/drivers/usart/usart.c \
src/ASF/xmega/drivers/usart/usart_xmegae.c \
src/ASF/xmega/drivers/xcl/xcl.c \
src/dali/dali_state/dali_hal/dali_hal.c \
src/dali/dali_state/dali_state.c \
src/dali/dali_usart/dali_usart.c \
src/dali/dali_bit/dali_bit.c \
src/dali/dali_timer/dali_tc.c \
src/modbus/mb.c \
src/modbus/rtu/mbrtu.c \
src/modbus/ascii/mbascii.c \
src/modbus/functions/mbfunccoils.c \
src/modbus/functions/mbfuncdiag.c \
src/modbus/functions/mbfuncholding.c \
src/modbus/functions/mbfuncinput.c \
src/modbus/functions/mbfuncother.c \
src/modbus/functions/mbfuncdisc.c \
src/modbus/functions/mbutils.c \
src/modbus/port/portserial.c \
src/modbus/port/portevent.c \
src/modbus/port/porttimer.c \
src/modbus/port/mbcrc.c \
src/main.c \
src/asf.h

# additional includes (e.g. -I/path/to/mydir)

INC += \
-I src \
-I src/ASF/common/boards/user_board \
-I src/config \
-I src/ASF/xmega/drivers/cpu \
-I src/ASF/xmega/drivers/edma \
-I src/ASF/xmega/drivers/nvm \
-I src/ASF/xmega/drivers/pmic \
-I src/ASF/xmega/drivers/sleep \
-I src/ASF/xmega/drivers/tc45 \
-I src/ASF/xmega/drivers/xcl \
-I src/ASF/common/services/clock \
-I src/ASF/common/services/gpio \
-I src/ASF/common/services/sleepmgr \
-I src/ASF/xmega/drivers/usart \
-I src/dali \
-I src/dali/dali_bit \
-I src/dali/dali_timer \
-I src/dali/dali_state \
-I src/dali/dali_usart \
-I src/ASF/common/boards \
-I src/ASF/xmega/utils \
-I src/ASF/common/utils \
-I src/ASF/xmega/utils/preprocessor \
-I src/ASF/common/services/ioport \
-I src/ASF/common/services/ioport/xmega \
-I src/ASF/xmega/drivers/ac \
-I src/dali/dali_state/dali_hal \
-I src/modbus/rtu \
-I src/modbus/ascii \
-I src/modbus/tcp \
-I src/modbus/include \
-I src/modbus/port \

# libraries to link in (e.g. -lmylib)
LIBS=

# Optimization level, 
# use s (size opt), 1, 2, 3 or 0 (off)
OPTLEVEL=1

AVRDUDE_PROGRAMMERID=stk500
AVRDUDE_PORT=/dev/ttyS0


####################################################
#####                Config Done               #####
#####                                          #####
##### You shouldn't need to edit anything      #####
##### below to use the makefile but may wish   #####
##### to override a few of the flags           #####
##### nonetheless                              #####
#####                                          #####
####################################################


##### Flags ####

# HEXFORMAT -- format for .hex file output
HEXFORMAT=ihex

# compiler
CFLAGS=\
	-I. $(INC) \
	-g \
	-mmcu=$(MCU) \
	-O$(OPTLEVEL) \
	-DBOARD=USER_BOARD \
	-DIOPORT_XMEGA_COMPAT \
	-fdata-sections \
	-ffunction-sections \
	-fpack-struct \
	-std=gnu99 \
	-fno-strict-aliasing \
	-Wmissing-prototypes \
	-Werror-implicit-function-declaration \
	-Wpointer-arith \
	-mrelax \
	-Wall \
	-Wstrict-prototypes \
	-Wa,-ahlms=$(firstword $(filter %.lst, $(<:.c=.lst)))

# c++ specific flags
CPPFLAGS=-fno-exceptions \
	-Wa,-ahlms=$(firstword \
	$(filter %.lst, $(<:.cpp=.lst))\
	$(filter %.lst, $(<:.cc=.lst)) \
	$(filter %.lst, $(<:.C=.lst)))

# assembler
ASMFLAGS =\
	-I. $(INC) \
	-mmcu=$(MCU) \
	-DBOARD=USER_BOARD \
	-x assembler-with-cpp \
	-mrelax \
	-Wa,-gdwarf2,-ahlms=$(firstword $(<:.S=.lst) $(<.s=.lst))


# linker
LDFLAGS=\
	-Wl,-Map,$(TRG).map \
	-Wl,--start-group \
	-Wl,--end-group \
	-Wl,--gc-sections \
	-Wl,--relax \
	-mmcu=$(MCU) \
	-lm $(LIBS)

##### executables ####
CC=avr-gcc
OBJCOPY=avr-objcopy
OBJDUMP=avr-objdump
SIZE=avr-size
AVRDUDE=avrdude
REMOVE=rm -f

##### automatic target names ####
TRG=$(PROJECTNAME).elf
DUMPTRG=$(PROJECTNAME).s

HEXROMTRG=$(PROJECTNAME).hex 
HEXTRG=$(HEXROMTRG) $(PROJECTNAME).ee.hex
GDBINITFILE=gdbinit-$(PROJECTNAME)

# Define all object files.

# Start by splitting source files by type
#  C++
CPPFILES=$(filter %.cpp, $(PRJSRC))
CCFILES=$(filter %.cc, $(PRJSRC))
BIGCFILES=$(filter %.C, $(PRJSRC))
#  C
CFILES=$(filter %.c, $(PRJSRC))
#  Assembly
ASMFILES=$(filter %.S, $(PRJSRC))


# List all object files we need to create
OBJDEPS=$(CFILES:.c=.o)    \
	$(CPPFILES:.cpp=.o)\
	$(BIGCFILES:.C=.o) \
	$(CCFILES:.cc=.o)  \
	$(ASMFILES:.S=.o)

# Define all lst files.
LST=$(filter %.lst, $(OBJDEPS:.o=.lst))

# All the possible generated assembly 
# files (.s files)
GENASMFILES=$(filter %.s, $(OBJDEPS:.o=.s)) 


.SUFFIXES : .c .cc .cpp .C .o .out .s .S \
	.hex .ee.hex .h .hh .hpp


.PHONY: writeflash clean stats gdbinit stats

# Make targets:
# all, disasm, stats, hex, writeflash/install, clean
all: $(TRG)

disasm: $(DUMPTRG) stats

stats: $(TRG)
	$(OBJDUMP) -h $(TRG)
	$(SIZE) $(TRG) 

hex: $(HEXTRG)


writeflash: hex
	$(AVRDUDE) -c $(AVRDUDE_PROGRAMMERID)   \
	 -p $(MCU) -P $(AVRDUDE_PORT) -e        \
	 -U flash:w:$(HEXROMTRG)

install: writeflash

test: 
	@echo $(firstword $(<:.S=.lst) $(<.s=.lst))

$(DUMPTRG): $(TRG) 
	$(OBJDUMP) -S  $< > $@


$(TRG): $(OBJDEPS) 
	$(CC) $(LDFLAGS) -o $(TRG) $(OBJDEPS)


#### Generating assembly ####
# asm from C
%.s: %.c
	$(CC) -S $(CFLAGS) $< -o $@

# asm from (hand coded) asm
%.s: %.S
	$(CC) -S $(ASMFLAGS) $< > $@


# asm from C++
.cpp.s .cc.s .C.s :
	$(CC) -S $(CFLAGS) $(CPPFLAGS) $< -o $@



#### Generating object files ####
# object from C
.c.o: 
	$(CC) $(CFLAGS) -c $< -o $@


# object from C++ (.cc, .cpp, .C files)
.cc.o .cpp.o .C.o :
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $<

# object from asm
.S.o :
	$(CC) $(ASMFLAGS) -c $< -o $@

#### Generating hex files ####
# hex files from elf
#####  Generating a gdb initialisation file    #####
%.hex: %.elf
	$(OBJCOPY) \
		-R .eeprom \
		-R .fuse \
		-R .lock \
		-R .signature \
		-R .user_signatures \
		-O $(HEXFORMAT) $< $@

%.ee.hex: %.elf
	$(OBJCOPY) \
		-j .eeprom \
		--change-section-lma .eeprom=0 \
		-O $(HEXFORMAT) $< $@


#####  Generating a gdb initialisation file    #####
##### Use by launching simulavr and avr-gdb:   #####
#####   avr-gdb -x gdbinit-myproject           #####
gdbinit: $(GDBINITFILE)

$(GDBINITFILE): $(TRG)
	@echo "file $(TRG)" > $(GDBINITFILE)
	
	@echo "target remote localhost:1212" \
		                >> $(GDBINITFILE)
	
	@echo "load"        >> $(GDBINITFILE) 
	@echo "break main"  >> $(GDBINITFILE)
	@echo "continue"    >> $(GDBINITFILE)
	@echo
	@echo "Use 'avr-gdb -x $(GDBINITFILE)'"


#### Cleanup ####
clean:
	$(REMOVE) $(TRG) $(TRG).map $(DUMPTRG)
	$(REMOVE) $(OBJDEPS)
	$(REMOVE) $(LST) $(GDBINITFILE)
	$(REMOVE) $(GENASMFILES)
	$(REMOVE) $(HEXTRG)
	


#####                    EOF                   #####
