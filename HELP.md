# Help Documentation for Audio Classification with STM32F746G Discovery

This document provides comprehensive guidance for using the Audio Classification project with the STM32F746G Discovery board.

## Table of Contents

- [Overview](#overview)
- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Getting Started](#getting-started)
- [Audio Preprocessing](#audio-preprocessing)
- [Model Deployment](#model-deployment)
- [Output Options](#output-options)
- [Troubleshooting](#troubleshooting)
- [References](#references)

## Overview

This project enables real-time audio classification on the STM32F746G Discovery board. It performs on-device audio preprocessing (MFCC or log-mel spectrogram extraction) and runs optimized inference using either TensorFlow Lite Micro or STM32Cube.AI.

### Key Features

- **Real-time audio capture**: Uses the onboard microphone for live audio input
- **On-device preprocessing**: MFCC or log-mel feature extraction directly on the microcontroller
- **Optimized inference**: Supports TFLite Micro and STM32Cube.AI frameworks
- **Multiple output options**: Serial communication and LED indicators

## Hardware Requirements

- **STM32F746G-DISCO** (Discovery Board)
  - ARM Cortex-M7 processor @ 216 MHz
  - 1 MB Flash, 340 KB SRAM
  - Onboard digital microphone (MP34DT01)
  - 4.3" LCD touchscreen display
  - USB OTG connectors

- **USB cables** for programming and serial communication
- **Computer** running Windows, macOS, or Linux

## Software Requirements

### Development Tools

- **STM32CubeIDE** (v1.10.0 or later) - Integrated development environment
- **STM32CubeMX** - For peripheral configuration
- **STM32Cube.AI** - For neural network optimization (optional)

### Libraries and Frameworks

- **TensorFlow Lite for Microcontrollers** - For model inference
- **CMSIS-DSP** - For digital signal processing operations
- **HAL drivers** - STM32 Hardware Abstraction Layer

### Model Training (Optional)

- Python 3.8+
- TensorFlow 2.x
- librosa (for audio preprocessing)
- NumPy, SciPy

## Getting Started

### Step 1: Clone the Repository

```bash
git clone https://github.com/MehediEEE45/Audio-Classification_with_Stm32F746G-.git
cd Audio-Classification_with_Stm32F746G-
```

### Step 2: Set Up the Development Environment

1. Install STM32CubeIDE from the ST website
2. Install STM32Cube.AI plugin (if using STM32Cube.AI for inference)
3. Configure the project settings for the STM32F746G-DISCO target

### Step 3: Build and Flash

1. Open the project in STM32CubeIDE
2. Build the project (Project → Build All)
3. Connect the STM32F746G-DISCO board via USB
4. Flash the firmware (Run → Debug or Run)

### Step 4: Monitor Output

Use a serial terminal (e.g., PuTTY, minicom, or the STM32CubeIDE serial monitor) at **115200 baud** to view classification results.

## Audio Preprocessing

### MFCC (Mel-Frequency Cepstral Coefficients)

MFCCs are widely used features for audio/speech recognition. The preprocessing pipeline:

1. **Frame the audio** - Split into overlapping windows (e.g., 25ms frames, 10ms hop)
2. **Apply window function** - Hamming or Hann window
3. **Compute FFT** - Fast Fourier Transform
4. **Apply Mel filterbank** - Convert to Mel scale
5. **Take logarithm** - Log compression
6. **Apply DCT** - Discrete Cosine Transform to get MFCCs

### Log-Mel Spectrogram

An alternative to MFCCs that preserves more spectral information:

1. **Frame the audio** - Similar windowing as MFCCs
2. **Compute FFT** - Short-time Fourier Transform
3. **Apply Mel filterbank** - Map to Mel frequency scale
4. **Take logarithm** - Log-scale the power spectrum

### Configuration Parameters

| Parameter | Typical Value | Description |
|-----------|---------------|-------------|
| Sample Rate | 16000 Hz | Audio sampling frequency |
| Frame Length | 25 ms | Duration of each audio frame |
| Frame Hop | 10 ms | Time between consecutive frames |
| Num Mel Bins | 40 | Number of Mel filterbank channels |
| Num MFCCs | 13 | Number of MFCC coefficients |
| FFT Length | 512 | Size of FFT window |

## Model Deployment

### Using TensorFlow Lite Micro

1. **Train your model** in TensorFlow/Keras
2. **Convert to TFLite** using the TFLite Converter
3. **Quantize** (recommended: INT8 quantization for STM32)
4. **Convert to C array** using `xxd -i model.tflite > model_data.cc`
5. **Include in firmware** and use the TFLite Micro interpreter

### Using STM32Cube.AI

1. **Import model** in STM32CubeMX with X-CUBE-AI
2. **Analyze and optimize** the network
3. **Generate code** for integration
4. **Build and deploy** with the generated API

### Model Optimization Tips

- Use **INT8 quantization** for smaller model size and faster inference
- Minimize model complexity (fewer layers, smaller filter sizes)
- Consider **pruning** to remove unnecessary weights
- Test inference time on-device to meet real-time requirements

## Output Options

### Serial (UART) Output

Classification results are sent via UART at 115200 baud:

```
Audio Class: speech
Confidence: 0.92
---
Audio Class: music
Confidence: 0.78
```

### LED Indicators

The onboard LEDs can indicate classification results:

- **Green LED**: Class 1 detected
- **Red LED**: Class 2 detected
- **Blue LED**: Class 3 detected

### LCD Display (Optional)

The 4.3" touchscreen can display:
- Real-time audio waveform
- Classification results
- Confidence scores

## Troubleshooting

### Common Issues

| Problem | Possible Cause | Solution |
|---------|---------------|----------|
| No audio input | Microphone not initialized | Check I2S/SAI configuration |
| Model inference fails | Insufficient memory | Reduce model size or optimize |
| Low accuracy | Preprocessing mismatch | Ensure training and inference preprocessing match |
| Slow inference | Model too large | Apply quantization or pruning |
| Build errors | Missing dependencies | Install required libraries |

### Debugging Tips

1. **Enable debug output** via serial to trace execution
2. **Check memory usage** with STM32CubeIDE Memory Analyzer
3. **Profile inference time** using DWT cycle counter
4. **Verify preprocessing output** by comparing with Python implementation

### Memory Considerations

The STM32F746G has limited RAM (340 KB). Memory allocation:

- Audio buffer: ~32 KB (1 second @ 16kHz, 16-bit)
- Feature buffer: ~10 KB (MFCC/Mel features)
- Model weights: Varies (target < 100 KB for typical models)
- Runtime tensors: ~50-100 KB (depends on model architecture)

## References

### Documentation

- [STM32F746G-DISCO User Manual](https://www.st.com/resource/en/user_manual/um1907-discovery-kit-with-stm32f746ng-mcu-stmicroelectronics.pdf)
- [TensorFlow Lite for Microcontrollers](https://www.tensorflow.org/lite/microcontrollers)
- [STM32Cube.AI Documentation](https://www.st.com/en/embedded-software/x-cube-ai.html)
- [CMSIS-DSP Library](https://arm-software.github.io/CMSIS_5/DSP/html/index.html)

### Tutorials

- [Getting Started with STM32F7](https://www.st.com/content/st_com/en/support/learning/stm32-education.html)
- [Audio Processing on STM32](https://wiki.st.com/stm32mcu/wiki/AI:How_to_perform_speech_recognition)
- [TinyML with TFLite Micro](https://www.tensorflow.org/lite/microcontrollers/get_started)

### Related Projects

- [TensorFlow Lite Micro Examples](https://github.com/tensorflow/tflite-micro)
- [STM32Cube.AI Examples](https://github.com/STMicroelectronics/STM32CubeAI)

---

For additional help or questions, please open an issue on the GitHub repository.
