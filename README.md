# L476-SPI2-W25QFlash
# STM32L476RG + W25Q128 SPI Flash Driver

## Overview

This project demonstrates SPI Flash communication between the STM32L476RG Nucleo board and the Winbond W25Q128 Serial Flash memory.

The driver was developed using STM32 HAL APIs and verified through UART debugging output.

The project implements basic Flash memory operations including:

- Read JEDEC ID
- Write Enable
- Read Status Register
- Wait Busy
- Sector Erase
- Page Program (Write Data)
- Read Data
- Data Verification

---

## Hardware

### MCU
- STM32L476RG (Nucleo-L476RG)

### SPI Flash
- Winbond W25Q128JV
- Capacity: 128 Mbit (16 MB)

### UART Debug
- USART2
- 9600 baud

---

## Hardware Connection

| STM32L476RG | W25Q128 |
|------------|----------|
| PB10 (SPI2_SCK) | CLK |
| PC3 (SPI2_MOSI) | DI |
| PC2 (SPI2_MISO) | DO |
| PC0 (GPIO Output) | CS |
| 3.3V | VCC |
| GND | GND |

---

## SPI Configuration

```c
SPI2

Mode            : Master
Direction       : 2 Lines
Data Size       : 8-bit
CLK Polarity    : Low
CLK Phase       : 1 Edge
NSS             : Software
Prescaler       : 32
First Bit       : MSB
```

## Driver Architecture

Project structure:

```text
Core/
├── Inc/
│   ├── main.h
│   └── w25qxx.h
│
├── Src/
│   ├── main.c
│   └── w25qxx.c
```

### Implemented APIs

```c
void W25Q_WriteEnable(void);

uint8_t W25Q_ReadStatus(void);

void W25Q_WaitBusy(void);

void W25Q_SectorErase(uint32_t addr);

void W25Q_Read(uint32_t addr,
               uint8_t *buf,
               uint16_t len);

void W25Q_Write(uint32_t addr,
                uint8_t *buf,
                uint16_t len);
```

## Commands Used

### Read JEDEC ID

```text
0x9F
```

### Write Enable

```text
0x06
```

### Read Status Register

```text
0x05
```

### Sector Erase

```text
0x20
```

### Page Program

```text
0x02
```

### Read Data

```text
0x03
```

## Functional Test

### 1. Read JEDEC ID

Expected:

```text
JEDEC ID = EF 40 18
```

Meaning:

| Byte | Description | Value |
|--------|--------|--------|
| Manufacturer ID | Winbond | EF |
| Memory Type | Serial Flash | 40 |
| Capacity | 128 Mbit | 18 |

---

### 2. Write Enable

Expected:

```text
Status = 0x02
```

Bit1 (WEL) is set.

---

### 3. Sector Erase

Erase address:

```text
0x000000
```

Verification:

```text
FF FF FF FF FF FF FF FF
FF FF FF FF FF FF FF FF
```

---

### 4. Write Data

Written string:

```text
Justin
```

---

### 5. Read Data

Expected:

```text
Read = Hello W25Qxx
```

---

### 6. Verification

```text
PASS
```

Data comparison performed using:

```c
strcmp(writeData, readData)
```

---

## UART Output

```text
Hello World! Nucleo-L476RG

JEDEC ID = EF 40 18

Status = 0x02

Sector Erase OK

FF FF FF FF FF FF FF FF
FF FF FF FF FF FF FF FF

Write OK

Read = Hello W25Qxx

PASS
```

## Development Environment

- STM32CubeIDE
- STM32 HAL Driver
- C Language
- PuTTY Serial Terminal

## Learning Outcomes

This project demonstrates:

- SPI Peripheral Configuration
- Flash Memory Driver Development
- Embedded C Programming
- UART Debugging
- Memory Read/Write Verification
- Driver Modularization (w25qxx.c / w25qxx.h)

## Future Improvements

- Page Boundary Handling
- Multi-Page Write
- Block Erase
- Chip Erase
- DMA-Based SPI Transfer
- FreeRTOS Integration
- File System (LittleFS / FATFS)

