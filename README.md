# Audio-Classification_with_Stm32F746G-
An audio signal is a time-varying waveform representing sound pressure (analog). For digital processing we sample it (e.g., 8 kHz, 16 kHz) and quantize amplitude into discrete numeric samples (PCM).
Two common views:
Time domain: amplitude vs time (waveform). Good for temporal info, transients, edges.
Frequency / time‑frequency domain: how energy is distributed across frequencies (FFT, spectrogram, mel-spectrogram, MFCC). These expose patterns humans/ML models use to distinguish sounds.
Use uncompressed PCM (WAV) or lossless compressed (FLAC) files for dataset capture and training. For on-device/embedded capture and inference use raw PCM (signed 16-bit) or WAV with PCM data. For speech keyword spotting: mono, 16 kHz, 16‑bit PCM is a good default. For broader environmental sounds you may choose 22.05 or 44.1 kHz if you need higher-frequency content.

# Step 
1) Hardware & audio capture (STM32F746G-DISCO specifics)
What the board provides :- The STM32F746G-DISCO includes dual MEMS microphones (PDM), a WM8994 audio codec, SAI/I2S connectivity, MicroSD and a headphone/line jack — so audio is commonly captured by the WM8994 which converts the PDM mic data to 16-bit PCM and sends it to the MCU over SAI/I2S. 
2)Use HAL drivers + DMA :- Configure the audio SAI (or I2S/SAI peripheral) in capture mode and start reception with DMA into a pair of ping-pong buffers (circular DMA). Keep buffer size = samples_per_frame (e.g. 16000 samples/s × frame_length_seconds) or smaller (e.g. 512/1024 samples) depending on latency and memory.On DMA half-complete and complete callbacks, process that buffer (store to SD or pass to preprocessing).
ST provides example audio capture projects for the Discovery family — use them to bootstrap SAI + WM8994 initialization. 
Optional: record WAV to microSD
Use FATFS (STM32Cube example) to write captured buffers into a WAV file for offline training / debugging. There are community examples that record WAV from the F746.
link
CMSIS DSP Website
https://arm-software.github.io/CMSIS_6/latest/General/index.html
GitHub Repositories 
G4: https://github.com/STMicroelectronics...
F4: https://github.com/STMicroelectronics...
F7: https://github.com/STMicroelectronics...

STM32G4 Reference Manual
https://www.st.com/resource/en/refere...

STM32G4 HAL API
https://www.st.com/resource/en/user_m...
