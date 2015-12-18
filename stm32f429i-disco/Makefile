PROJ_NAME = stm32f429-example

##########################################
# Toolchain Settings
##########################################

CROSS_COMPILE ?= arm-none-eabi-
CC = $(CROSS_COMPILE)gcc
LD = $(CROSS_COMPILE)ld
OBJDUMP = $(CROSS_COMPILE)objdump
OBJCOPY = $(CROSS_COMPILE)objcopy
SIZE = $(CROSS_COMPILE)size

##########################################
# Files 
##########################################

USER_SRCS = $(wildcard src/*.c)
STDLIB_SRCS = $(wildcard stdlib/src/*.c)
STDIO_SRCS = $(wildcard stdio/*.c)
STARTUP_SRCS = $(wildcard cmsis_boot/*.c)
SYSCALL_SRCS = $(wildcard syscalls/*.c)
UGUI_SRCS = $(wildcard uGUI/*c)

OBJS = $(USER_SRCS:.c=.o) \
		$(STDLIB_SRCS:.c=.o) \
		$(STDIO_SRCS:.c=.o) \
		$(STARTUP_SRCS:.c=.o) \
		$(SYSCALL_SRCS:.c=.o) \
		$(UGUI_SRCS:.c=.o)

INCLUDES = -Icmsis \
			-Icmsis_boot \
			-Iinc \
			-Istdlib/inc \
			-IuGUI
##########################################
# Flag Settings 
##########################################

MCU = -mcpu=cortex-m4 -mthumb
FPU = -mfpu=fpv4-sp-d16 -mfloat-abi=hard -D__FPU_USED
DEFINES = -DSTM32F4XX -DSTM32F429_439xx -DUSE_STDPERIPH_DRIVER -D__ASSEMBLY__

CFLAGS = $(MCU) $(DEFINES) $(INCLUDES) -g2 -Wall -O0 -c
LDFLAGS = $(MCU) -g2 -nostartfiles -Wl,-Map=STM32F429.map -O0 -Wl,--gc-sections -Tarm-gcc-link.ld

##########################################
# Targets
###########################################

all: $(PROJ_NAME).bin info

$(PROJ_NAME).elf: $(OBJS)
			 @$(CC) $(OBJS) $(LDFLAGS) -o $@
			 @echo $@

$(PROJ_NAME).bin: $(PROJ_NAME).elf
			 @$(OBJCOPY) -O binary $(PROJ_NAME).elf $(PROJ_NAME).bin
			 @echo $@

info: $(PROJ_NAME).elf
	@$(SIZE) --format=berkeley $(PROJ_NAME).elf

.c.o:
	@$(CC) $(CFLAGS) -c -o $@ $<
	@echo $@

clean:
	rm -f $(OBJS)
	rm -f $(PROJ_NAME).elf
	rm -f $(PROJ_NAME).bin
	rm -f $(PROJ_NAME).map






