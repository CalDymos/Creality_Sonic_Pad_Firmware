# Additional avr build rules

# Use the avr toolchain
CROSS_PREFIX=avr-

dirs-y += src/avr src/generic

CFLAGS += -mmcu=$(CONFIG_MCU)

# Add avr source files
modify_timer:
	@echo "Modifying hard_pwm.c"
	cp -f src/avr/hard_pwm_.c src/avr/hard_pwm.c
ifeq ($(CONFIG_TIMER_30HZ_D3_D11), y)
		sed -i 's@//#TIMER2_PWM_D3&D11#@if \(pin == GPIO\('\''D'\'', 3\) || pin == GPIO\('\''D'\'', 11\)\) { // Prüfen, ob der Pin D3 oder D11 ist\n    TCCR2B = \(TCCR2B \& 248\) | 7; // set Timer 1 divisor to PWM freq of 30.64 Hz\n} @' src/avr/hard_pwm.c
endif
ifeq ($(CONFIG_TIMER_122HZ_D3_D11), y)
		sed -i 's@//#TIMER2_PWM_D3&D11#@if \(pin == GPIO\('\''D'\'', 3\) || pin == GPIO\('\''D'\'', 11\)\) { // Prüfen, ob der Pin D3 oder D11 ist\n    TCCR2B = \(TCCR2B \& 248\) | 6; // set Timer 1 divisor to PWM freq of 122.55 Hz\n} @' src/avr/hard_pwm.c
endif
ifeq ($(CONFIG_TIMER_245HZ_D3_D11), y)
		sed -i 's@//#TIMER2_PWM_D3&D11#@if \(pin == GPIO\('\''D'\'', 3\) || pin == GPIO\('\''D'\'', 11\)\) { // Prüfen, ob der Pin D3 oder D11 ist\n    TCCR2B = \(TCCR2B \& 248\) | 5; // set Timer 1 divisor to PWM freq of 245.1 Hz\n} @' src/avr/hard_pwm.c
endif
ifeq ($(CONFIG_TIMER_DEFAULT_D3_D11), y)
		sed -i 's@//#TIMER2_PWM_D3&D11#@if \(pin == GPIO\('\''D'\'', 3\) || pin == GPIO\('\''D'\'', 11\)\) { // Prüfen, ob der Pin D3 oder D11 ist\n    TCCR2B = \(TCCR2B \& 248\) | 4; // set Timer 1 divisor to PWM freq of 490.2 Hz\n} @' src/avr/hard_pwm.c
endif
ifeq ($(CONFIG_TIMER_980HZ_D3_D11), y)
		sed -i 's@//#TIMER2_PWM_D3&D11#@if \(pin == GPIO\('\''D'\'', 3\) || pin == GPIO\('\''D'\'', 11\)\) { // Prüfen, ob der Pin D3 oder D11 ist\n    TCCR2B = \(TCCR2B \& 248\) | 3; // set Timer 1 divisor to PWM freq of 980.39 Hz\n} @' src/avr/hard_pwm.c
endif
ifeq ($(CONFIG_TIMER_3921HZ_D3_D11), y)
		sed -i 's@//#TIMER2_PWM_D3&D11#@if \(pin == GPIO\('\''D'\'', 3\) || pin == GPIO\('\''D'\'', 11\)\) { // Prüfen, ob der Pin D3 oder D11 ist\n    TCCR2B = \(TCCR2B \& 248\) | 2; // set Timer 1 divisor to PWM freq of 3921.16 Hz\n} @' src/avr/hard_pwm.c
endif
ifeq ($(CONFIG_TIMER_31KHZ_D3_D11), y)
		sed -i 's@//#TIMER2_PWM_D3&D11#@if \(pin == GPIO\('\''D'\'', 3\) || pin == GPIO\('\''D'\'', 11\)\) { // Prüfen, ob der Pin D3 oder D11 ist\n    TCCR2B = \(TCCR2B \& 248\) | 1; // set Timer 1 divisor to PWM freq of 31372.55 Hz\n} @' src/avr/hard_pwm.c
endif
src-y += avr/main.c avr/timer.c
src-$(CONFIG_HAVE_GPIO) += avr/gpio.c
src-$(CONFIG_HAVE_GPIO_ADC) += avr/adc.c
src-$(CONFIG_HAVE_GPIO_SPI) += avr/spi.c
src-$(CONFIG_HAVE_GPIO_I2C) += avr/i2c.c
src-$(CONFIG_HAVE_GPIO_HARD_PWM) += avr/hard_pwm.c
src-$(CONFIG_AVR_WATCHDOG) += avr/watchdog.c
src-$(CONFIG_USBSERIAL) += avr/usbserial.c generic/usb_cdc.c
src-$(CONFIG_SERIAL) += avr/serial.c generic/serial_irq.c

# Suppress broken "misspelled signal handler" warnings on gcc 4.8.1
CFLAGS_klipper.elf := $(CFLAGS_klipper.elf) $(if $(filter 4.8.1, $(shell $(CC) -dumpversion)), -w)

# Build the additional hex output file
target-y += $(OUT)klipper.elf.hex

$(OUT)klipper.elf.hex: $(OUT)klipper.elf
	@echo "  Creating hex file $@"
	$(Q)$(OBJCOPY) -j .text -j .data -O ihex $< $@

flash: $(OUT)klipper.elf.hex
	@echo "  Flashing $< to $(FLASH_DEVICE) via avrdude"
	$(Q)if [ -z $(FLASH_DEVICE) ]; then echo "Please specify FLASH_DEVICE"; exit 1; fi
	$(Q)avrdude -p$(CONFIG_MCU) -c$(CONFIG_AVRDUDE_PROTOCOL) -P"$(FLASH_DEVICE)" -D -U"flash:w:$(<):i"
