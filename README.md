Frequency-Division-Multiplexing---Modulation-and-Demodulation-using-Python

__Aim__:

To generate an FDM signal by multiplexing multiple baseband message signals on different carrier frequencies, transmit (sum) them, optionally add channel noise, then recover each message by bandpass filtering and coherent demodulation in Python (Google Colab). Observe time & frequency domain signals and measure recovery quality.

Apparatus Required:

Google Colab (or any Python environment)

Python libraries: numpy, matplotlib, scipy (scipy.signal)

Theory:

FDM places different message signals in separate, non-overlapping frequency bands by modulating each message onto a distinct carrier frequency. The multiplexed signal is the sum of all modulated channels. At the receiver, bandpass filters (or tuned filters) isolate each channel; then each isolated carrier is demodulated (coherently multiplied by a synchronized carrier) and low-pass filtered to recover the original baseband.

Procedure:

1 — Imports and parameters

2 — Create message signals and carriers

3 — Modulate each message (standard AM DSB-SC) and form FDM signal

4 — Frequency domain (spectrum) of FDM signal

5 — (Optional) Add AWGN noise to FDM signal

6 — Receiver: isolate each channel with bandpass filter

7 — Demodulate each isolated channel (coherent) and low-pass filter to recover baseband

Program:
~~~

import numpy as np
import matplotlib.pyplot as plt

# Time vector
t = np.linspace(0, 1, 1000)

# Frequencies of individual signals
freqs = [5, 5.5, 6, 6.5, 7, 7.5]

# Generate 6 input signals
signals = np.zeros((6, len(t)))

for i in range(6):
    signals[i, :] = np.sin(2 * np.pi * freqs[i] * t)

# FDM (adding all signals)
fdm_signal = np.zeros(len(t))

for i in range(6):
    fdm_signal += signals[i, :]

# Demultiplexing using synchronous detection
demux_signals = np.zeros((6, len(t)))

for i in range(6):
    demux_signals[i, :] = fdm_signal * np.sin(2 * np.pi * freqs[i] * t)

# ---------------- PLOTTING ---------------- #

# Plot original signals
plt.figure(figsize=(10, 8))
for i in range(6):
    plt.subplot(3, 2, i + 1)
    plt.plot(t, signals[i])
    plt.title(f"Original Signal f = {freqs[i]} Hz")
    plt.grid()
plt.tight_layout()
plt.show()

# Plot FDM Combined Signal
plt.figure(figsize=(10, 4))
plt.plot(t, fdm_signal)
plt.title("FDM Signal")
plt.grid()
plt.show()

# Plot demultiplexed signals
plt.figure(figsize=(10, 8))
for i in range(6):
    plt.subplot(3, 2, i + 1)
    plt.plot(t, demux_signals[i])
    plt.title(f"Demultiplexed Output f = {freqs[i]} Hz")
    plt.grid()
plt.tight_layout()
plt.show()


end
~~~
## Output:

<img width="989" height="790" alt="image" src="https://github.com/user-attachments/assets/0ba6951d-3777-4050-8ad5-0553539cf05a" />
<img width="825" height="374" alt="image" src="https://github.com/user-attachments/assets/c055ea14-769d-477d-8ccc-31c49d061697" />
<img width="990" height="790" alt="image" src="https://github.com/user-attachments/assets/639b2a22-b931-4d38-becd-a1446479e12f" />


Result:
The signals were successfully modulated the signals were multiplexed into an FDM signal and accurately demodulated using coherent detection.

FDM was successfully simulated. The six input signals were combined into one composite signal and later recovered correctly through demultiplexing using Scilab.
