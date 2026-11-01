# STM32H747-ADC-Pressure-Sensor

# STM32H747 ADC Interfacing – Pressure Sensor Data Acquisition

## 1. Project Overview

This project focuses on Analog-to-Digital Converter (ADC) interfacing using the STM32H747 microcontroller and STM32CubeIDE.

The objective is to configure the ADC peripheral, acquire analog input data, convert the digital ADC output into voltage, and prepare the system for pressure sensor interfacing.

The implementation uses STM32 Hardware Abstraction Layer (HAL) libraries and ADC polling mode.

## 2. Aim of the Task

The main aim is to develop an ADC-based data acquisition system using the STM32H747 microcontroller.

The objectives are:

- Configure the ADC peripheral for analog signal acquisition.
- Configure the GPIO pin in analog mode.
- Read analog input through ADC.
- Obtain the raw digital ADC value.
- Convert the ADC reading into an equivalent voltage.
- Prepare the firmware for future pressure sensor integration and calibration.

## 3. Hardware Configuration

| Parameter | Configuration |
|---|---|
| Microcontroller | STM32H747 |
| Development Board | STM32H747I-DISCO |
| ADC Peripheral | ADC1 |
| ADC Channel | Channel 18 |
| GPIO Pin | PA4 |
| ADC Resolution | 16-bit |
| ADC Input Mode | Single-ended |
| Conversion Mode | Continuous |
| Trigger Source | Software trigger |
| ADC Data Management | Data Register (DR) |
| Sampling Time | 1.5 ADC clock cycles |
| ADC Operating Mode | Independent |

## 4. GPIO Configuration

The GPIO pin PA4 is configured as an analog input for ADC1 Channel 18.

Configuration:

- GPIO Port: GPIOA
- GPIO Pin: PA4
- GPIO Mode: Analog
- Pull-up/Pull-down: No Pull

HAL configuration:

```c
GPIO_InitStruct.Pin = GPIO_PIN_4;
GPIO_InitStruct.Mode = GPIO_MODE_ANALOG;
GPIO_InitStruct.Pull = GPIO_NOPULL;
HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
```

## 5. ADC Configuration

ADC1 is configured using STM32CubeMX.

Important configuration parameters:

- Resolution: 16-bit
- Scan Conversion Mode: Disabled
- Continuous Conversion Mode: Enabled
- Number of Conversions: 1
- External Trigger: Software Start
- End of Conversion Selection: Single Conversion
- Oversampling: Disabled
- Data Management: Data Register

The ADC is configured to acquire an analog signal through Channel 18.

## 6. Software Implementation

The firmware is developed in STM32CubeIDE using STM32 HAL libraries.

The ADC data acquisition process follows these steps:

1. Start ADC conversion.
2. Wait for conversion completion using polling.
3. Read the ADC digital value.
4. Calculate the corresponding voltage.
5. Stop ADC conversion.
6. Repeat the process after a 100 ms delay.

### ADC Data Acquisition Code

```c
HAL_ADC_Start(&hadc1);

if (HAL_ADC_PollForConversion(&hadc1, 100) == HAL_OK)
{
    adc_raw = HAL_ADC_GetValue(&hadc1);

    adc_voltage = ((float)adc_raw * 3.3f) / 65535.0f;
}

HAL_ADC_Stop(&hadc1);

HAL_Delay(100);
```

### Variable Declaration

```c
volatile uint32_t adc_raw = 0;
volatile float adc_voltage = 0.0f;
```

- `adc_raw`: Stores the digital output received from the ADC.
- `adc_voltage`: Stores the calculated analog voltage.

## 7. Voltage Conversion

For a 16-bit ADC, the digital output ranges from 0 to 65535.

The voltage is calculated using:

\[
V_{in} = \frac{ADC_{\text{raw}} \times V_{\text{ref}}}{65535}
\]

Where:

- \(ADC_{raw}\) = Digital ADC reading
- \(V_{ref}\) = Assumed reference voltage of 3.3 V
- \(V_{in}\) = Calculated input voltage

The actual reference voltage and ADC input range must be verified during hardware testing.

## 8. Software Development Environment

| Tool | Purpose |
|---|---|
| STM32CubeMX | ADC and GPIO configuration |
| STM32CubeIDE | Firmware development and compilation |
| STM32 HAL | Peripheral initialization and ADC operations |
| C Programming | Embedded firmware development |
| GitHub | Source code management and version control |

## 9. Current Implementation Status

**Completed:**
- STM32H747 project configuration.
- ADC1 peripheral configuration.
- GPIO PA4 analog configuration.
- ADC Channel 18 selection.
- 16-bit ADC configuration.
- ADC polling implementation.
- Raw ADC data acquisition code.
- Voltage conversion calculation.
- Firmware compilation.
- Project version control using GitHub.

**Pending:**
- Physical board programming and debugging.
- Actual ADC input measurement.
- Pressure sensor hardware interfacing.
- Sensor calibration.
- Pressure calculation and validation.
