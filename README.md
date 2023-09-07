# MaaxBoard-RT_flexio_spi_edma_lpspi_transfer_,master_cm7
 FLEXIO as SPI master plus LSPI2 as slave

## Demo: FLEXIO as SPI master
This is a converted SDK project plus modifications.
It implements a SPI master with FLEXIO and the SPI slave is the LPSPI2.
With FLEXIO you can implement additional devices, such as SPI, I2C, UART, ...

## Debug UART is LPUART6
As Debug UART the LPUART6 is used:
* J1, pin 16 = RXD
* J1, pin 18 = TXD
The original Debug UART pins are used for LPSPI2 slave (SCK, PCS0).
Connect the Debug UART on the pins mentioned above.

## Testing SPI Loopback
Connect the SPI Master (FLEXIO) to the SPI Slave (LPSPI2):
 * J1, pin 33 - J1, pin 31 = MOSI - MOSI
 * J1, pin 13 - J1, pin 08 = SCLK - SCLK
 * J1, pin 11 - J1, pin 10 = PCS  - PCS
 * J1, pin 32 - J1, pin 29 = MISO - MISO

## Max. Speed
Depending on the clock configuration (in file "clock_config.c") I could get 258 MHz working - impressive (with short wires to connect).

With unchanged clock config, the max. speed is 47 MHz.

```
    /* Configure FLEXIO2 using OSC_RC_48M_DIV2 */
    rootCfg.mux = kCLOCK_FLEXIO2_ClockRoot_MuxOscRc48MDiv2;
    rootCfg.div = 1;			/* max. SPI clock is 46.887 MHz */
    /* make the FLEXIO2 faster: results in 257.865 MHz! */
    ////rootCfg.mux = kCLOCK_FLEXIO2_ClockRoot_MuxSysPll2Out;
    ////rootCfg.div = 4;
    CLOCK_SetRootClock(kCLOCK_Root_Flexio2, &rootCfg);
```

