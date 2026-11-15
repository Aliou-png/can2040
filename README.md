
RASPBERRY PI PICO CAN BUS & FREERTOS
==========================================

This project implements a simple CAN bus communication and FreeRTOS tasks on the Raspberry Pi Pico (RP2040) using the can2040 library. It supports building and flashing different roles (Sender and Receiver) onto separate Pico boards.

CAN transceivers:

https://ww1.microchip.com/downloads/aemDocuments/documents/APID/ProductDocuments/DataSheets/MCP2561-2FD-High-Speed-CAN-Flexible-Data-Rate-Transceiver-DS20005284.pdf

PREREQUISITES
-------------
Make sure you have the following installed and configured on your system:
- Raspberry Pi Pico SDK (PICO_SDK_PATH environment variable set)
- CMake (version 3.13 or higher)
- ARM GCC Toolchain (arm-none-eabi-gcc)
- picotool (for flashing firmware via USB)


PROJECT EXECUTABLES
-------------------
- can_activity (main.c): Main CAN bus node. Compiles as either a SENDER or RECEIVER depending on the build configuration.

BUILDING AND FLASHING
---------------------
1. Configure the Builds
Generate separate build directories for the Sender and Receiver roles:

    cmake -B build_sender -S . -DROLE=SENDER
    cmake -B build_receiver -S . -DROLE=RECEIVER

2. Flash to the Raspberry Pi Pico
Connect the target Raspberry Pi Pico to your computer via USB (hold the BOOTSEL button while plugging it in if picotool is not configured to force a reboot), then run the corresponding flash command:

    # Flash the Sender Pico (ensure only the Sender Pico is connected)
    cmake --build build_sender --target flash

    # Flash the Receiver Pico (ensure only the Receiver Pico is connected)
    cmake --build build_receiver --target flash

(Note: The flash target automatically compiles can_activity and loads it onto the connected board using picotool.)

RUNNING & MONITORING OUTPUT
---------------------------
Standard output (stdio) is enabled over USB and disabled over UART for all targets.

1. Power both flashed Raspberry Pi Pico boards and connect them to your CAN transceiver/bus.
2. Open a serial monitor (such as minicom, screen, PuTTY, or the VS Code Serial Monitor) at 115200 baud on the Pico's USB serial port (e.g., /dev/ttyACM0 on Linux/macOS or COMx on Windows) to view live logs.


