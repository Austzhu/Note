# uboot下mtd设备

# spi nor
* 在uboot中spi nor驱动(driver/mtd/spi/...)
	1> 要使用spi nor flash的mtd驱动需要配置CONFIG_SPI_FLASH_MTD
	2> spi nor对外开放了底层flash操作接口
	```
		spi_flash_read(...);
		spi_flash_write(...);
		spi_flash_erase(...);
			...
	```
	3> uboot中大量代码使用的时spi nor的底层驱动，而不是通过mtd操作的
	```
		cmd/sf.c:81:static int do_spi_flash_probe(int argc, char * const argv[])
		cmd/sf.c:130:ret = spi_flash_probe_bus_cs(bus, cs, speed, mode, &new);
		cmd/sf.c:142:new = spi_flash_probe(bus, cs, speed, mode);
		cmd/sf.c:560:ret = do_spi_flash_probe(argc, argv);
		board/siemens/taurus/taurus.c:141:flash = spi_flash_probe(CONFIG_SF_DEFAULT_BUS,
		board/Synology/ds414/cmd_syno.c:41:flash = spi_flash_probe(bus, cs, speed, mode);
		board/congatec/cgtqmx6eval/cgtqmx6eval.c:1006:spi = spi_flash_probe(CONFIG_ENV_SPI_BUS,
		board/Arcturus/ucp1020/cmd_arc.c:76:flash = spi_flash_probe(CONFIG_ENV_SPI_BUS, CONFIG_ENV_SPI_CS,
		board/Arcturus/ucp1020/cmd_arc.c:115:flash = spi_flash_probe(CONFIG_ENV_SPI_BUS, CONFIG_ENV_SPI_CS,
		board/buffalo/lsxl/lsxl.c:217:flash = spi_flash_probe(0, 0, 1000000, SPI_MODE_3);
		board/davinci/da8xxevm/da850evm.c:55:flash = spi_flash_probe(CFG_MAC_ADDR_SPI_BUS, CFG_MAC_ADDR_SPI_CS,
		board/renesas/sh7753evb/sh7753evb.c:207:spi = spi_flash_probe(0, 0, 1000000, SPI_MODE_3);
		board/renesas/sh7753evb/sh7753evb.c:300:spi = spi_flash_probe(0, 0, 1000000, SPI_MODE_3);
		board/renesas/sh7757lcr/sh7757lcr.c:37:spi = spi_flash_probe(0, 0, 1000000, SPI_MODE_3);
		board/renesas/sh7757lcr/sh7757lcr.c:262:spi = spi_flash_probe(0, 0, 1000000, SPI_MODE_3);
		board/renesas/sh7757lcr/sh7757lcr.c:414:spi = spi_flash_probe(0, 0, 1000000, SPI_MODE_3);
		board/renesas/sh7752evb/sh7752evb.c:191:spi = spi_flash_probe(0, 0, 1000000, SPI_MODE_3);
		board/renesas/sh7752evb/sh7752evb.c:284:spi = spi_flash_probe(0, 0, 1000000, SPI_MODE_3);
		common/splash_source.c:27:sf = spi_flash_probe(CONFIG_SF_DEFAULT_BUS,
		common/env_sf.c:59:ret = spi_flash_probe_bus_cs(CONFIG_ENV_SPI_BUS, CONFIG_ENV_SPI_CS,
		common/env_sf.c:62:set_default_env("!spi_flash_probe_bus_cs() failed");
		common/env_sf.c:70:env_flash = spi_flash_probe(CONFIG_ENV_SPI_BUS,
		common/env_sf.c:74:set_default_env("!spi_flash_probe() failed");
		common/env_sf.c:169:env_flash = spi_flash_probe(CONFIG_ENV_SPI_BUS, CONFIG_ENV_SPI_CS,
		common/env_sf.c:172:set_default_env("!spi_flash_probe() failed");
		common/env_sf.c:249:ret = spi_flash_probe_bus_cs(CONFIG_ENV_SPI_BUS, CONFIG_ENV_SPI_CS,
		common/env_sf.c:252:set_default_env("!spi_flash_probe_bus_cs() failed");
		common/env_sf.c:260:env_flash = spi_flash_probe(CONFIG_ENV_SPI_BUS,
		common/env_sf.c:264:set_default_env("!spi_flash_probe() failed");
		common/env_sf.c:329:env_flash = spi_flash_probe(CONFIG_ENV_SPI_BUS, CONFIG_ENV_SPI_CS,
		common/env_sf.c:332:set_default_env("!spi_flash_probe() failed");
		drivers/dfu/dfu_sf.c:105:dev = spi_flash_probe(bus, cs, speed, mode);
		drivers/mtd/spi/sf-uclass.c:36:struct spi_flash *spi_flash_probe(unsigned int bus, unsigned int cs,
		drivers/mtd/spi/sf-uclass.c:41:if (spi_flash_probe_bus_cs(bus, cs, max_hz, spi_mode, &dev))
		drivers/mtd/spi/sf-uclass.c:52:int spi_flash_probe_bus_cs(unsigned int busnum, unsigned int cs,
		drivers/mtd/spi/sf_probe.c:21: * spi_flash_probe_slave() - Probe for a SPI flash device on a bus
		drivers/mtd/spi/sf_probe.c:26:static int spi_flash_probe_slave(struct spi_flash *flash)
		drivers/mtd/spi/sf_probe.c:58:static struct spi_flash *spi_flash_probe_tail(struct spi_slave *bus)
		drivers/mtd/spi/sf_probe.c:70:if (spi_flash_probe_slave(flash)) {
		drivers/mtd/spi/sf_probe.c:79:struct spi_flash *spi_flash_probe(unsigned int busnum, unsigned int cs,
		drivers/mtd/spi/sf_probe.c:87:return spi_flash_probe_tail(bus);
		drivers/mtd/spi/sf_probe.c:91:struct spi_flash *spi_flash_probe_fdt(const void *blob, int slave_node,
		drivers/mtd/spi/sf_probe.c:99:return spi_flash_probe_tail(bus);
		drivers/mtd/spi/sf_probe.c:156:return spi_flash_probe_slave(flash);
		drivers/mtd/spi/spi_spl_load.c:78:flash = spi_flash_probe(CONFIG_SF_DEFAULT_BUS,
		drivers/mtd/spi/fsl_espi_spl.c:19:flash = spi_flash_probe(CONFIG_ENV_SPI_BUS, CONFIG_ENV_SPI_CS,
		drivers/mtd/spi/fsl_espi_spl.c:22:puts("\nspi_flash_probe failed");
		drivers/mtd/spi/fsl_espi_spl.c:43:flash = spi_flash_probe(CONFIG_ENV_SPI_BUS, CONFIG_ENV_SPI_CS,
		drivers/mtd/spi/fsl_espi_spl.c:46:puts("\nspi_flash_probe failed");
		drivers/net/phy/cortina.c:153:ucode_flash = spi_flash_probe(CONFIG_ENV_SPI_BUS, CONFIG_ENV_SPI_CS,
		drivers/net/fm/fm.c:371:ucode_flash = spi_flash_probe(CONFIG_ENV_SPI_BUS, CONFIG_ENV_SPI_CS,
		spl/drivers/mtd/spi/sf-uclass.su:6:sf-uclass.c:52:5:spi_flash_probe_bus_cs32static
		spl/drivers/mtd/spi/sf-uclass.su:7:sf-uclass.c:36:19:spi_flash_probe24static
	```

# spi nand

* 与spi nor类似

# uboot mtd介绍:

![mtd](/home/Austzhu/Document/others/uboot_mtd.png)

