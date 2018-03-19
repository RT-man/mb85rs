- Tested in kernel version 3.10.x

- Tested on MB85RS64V
  Other series have seen and implemented data sheets
  
- The device tree is not considered

- How to add this driver to kernel directory

  1) Assuming you add it to a [drivers/misc/eeprom/] directory.
     At the end of Kconfig, add the following:  
     --------------------------------------------------------------------------
     config EEPROM_MB85RS
            tristate "Fujitsu SPI MB85RS FRAM(Ferroelectronic RAM) driver"
            depends on SPI && SYSFS
            help
              Enable this driver to get read/write support to Fujitsu MB85RS.
     --------------------------------------------------------------------------
     At the end of Makefile, add the following:
     --------------------------------------------------------------------------
     obj-$(CONFIG_EEPROM_MB85RS) += mb85rs.o
     --------------------------------------------------------------------------
     
  2) Copy mb85rs.c to to a [drivers/misc/eeprom/] directory.
     
  3) In the machine description file, register as follows:
     --------------------------------------------------------------------------
     int mb85rs_wp_gpio = GPIO_PORTB11;
     static struct spi_board_info spiN_board_info[] __initdata = {
             {
                     .modalias = "mb85rs",
                     .max_speed_hz = 20 * 1000 * 1000,
                     .bus_num = N,
                     .chip_select = 0,
                     .mode = SPI_MODE_0,
                     .platform_data = &mb85rs_wp_gpio,
             },
     };
    --------------------------------------------------------------------------

Things to do
- Device tree must be supported
- Support block protect option with sysfs
- Block protect should be considered when writing
- Easy and convenient access interface from user space
