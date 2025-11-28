# Audio-Classification_with_Stm32F746G-
An audio signal is a time-varying waveform representing sound pressure (analog). For digital processing we sample it (e.g., 8 kHz, 16 kHz) and quantize amplitude into discrete numeric samples (PCM).
Two common views:
Time domain: amplitude vs time (waveform). Good for temporal info, transients, edges.
Frequency / time‑frequency domain: how energy is distributed across frequencies (FFT, spectrogram, mel-spectrogram, MFCC). These expose patterns humans/ML models use to distinguish sounds.
Use uncompressed PCM (WAV) or lossless compressed (FLAC) files for dataset capture and training. For on-device/embedded capture and inference use raw PCM (signed 16-bit) or WAV with PCM data. For speech keyword spotting: mono, 16 kHz, 16‑bit PCM is a good default. For broader environmental sounds you may choose 22.05 or 44.1 kHz if you need higher-frequency content.
