# Changelog

This will track all major changes to the Thetis instrumentation board hardware.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project uses the following versioning semantics:
  Letter - Major variant
  Number - Minor Revision
  e.g. Rev F4 is the fourth revision of the "F" variant of Thetis.
  
The difference between variants is largely subjective, but key distinguishers can be:
- Different microcontrollers
- Different board shapes or sizes
- Different components
- Different applications
- Different component specifications
- Or any combination thereof

<!-- 
Release sections
### Known Bugs
### Added
### Changed
### Fixed
### Deprecated
### Removed
### Security 
-->

## Rev F4 - 05/22

### Known Bugs
- [CRITICAL] Device crashes when trying to access GPIO 26 (SD_CS)
    - This is caused by the ESP32-S2 module with additional PSRAM using GPIO 26 as the CS for the PSRAM. 
      When unexpectedly accessed by the main application, the chip crashes.
      This is documented in the [ESP32-S2-MINI-1U datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-s2-mini-1_esp32-s2-mini-1u_datasheet_en.pdf), page 11.
    - This issue is mitigated in hardware by cutting the trace from GPIO 26 to the CS pin on the SD card receptacle and bodging a wire from the CS pin to GND
    - This issue is mitigated in software by avoiding a definition of anything to `GPIO 26` and using a spoofed pin number (typ. `GPIO 13`) for the SPI bus.
      This does not result in any lost functionality as the SD_CS is constantly tied to GND through the hardware bodge.
