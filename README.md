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
Depending on the clock configuration (in file "clock_config.c") I could get 66 MHz working - but waveform is wrong!
The max. SPI speed SCLK is: 30 MHz (waveform OK).

```
    /* Configure FLEXIO2 using OSC_RC_48M_DIV2 */
    rootCfg.mux = kCLOCK_FLEXIO2_ClockRoot_MuxOscRc48MDiv2;
    rootCfg.div = 1;			/* max. SPI clock is 46.887 MHz */
    /* make the FLEXIO2 faster: 528 MHz div 4 = 132 MHz (135 MHz is max. LPSPIx_CLK_ROOT) */
    ////rootCfg.mux = kCLOCK_FLEXIO2_ClockRoot_MuxSysPll2Out;
    ////rootCfg.div = 4;
    CLOCK_SetRootClock(kCLOCK_Root_Flexio2, &rootCfg);
```

## Conclusion
The max. SPI SCLK speed with FLEXIO is: 30 MHz.
With regular LPSPIx SCLK can be:        45 MHz.
(anything faster, next and fastest as 66 MHz - has "ugly" waveform)

