Frequency-Division-Multiplexing---Modulation-and-Demodulation-using-Python

__Aim__:

To generate an FDM signal by multiplexing multiple baseband message signals on different carrier frequencies, transmit (sum) them, optionally add channel noise, then recover each message by bandpass filtering and coherent demodulation in Python (Google Colab). Observe time & frequency domain signals and measure recovery quality.


__Apparatus Required__:

Google Colab (or any Python environment)

Python libraries: numpy, matplotlib, scipy (scipy.signal)


__Theory__:

FDM places different message signals in separate, non-overlapping frequency bands by modulating each message onto a distinct carrier frequency. The multiplexed signal is the sum of all modulated channels. At the receiver, bandpass filters (or tuned filters) isolate each channel; then each isolated carrier is demodulated (coherently multiplied by a synchronized carrier) and low-pass filtered to recover the original baseband.

__Procedure__:

1 — Imports and parameters

2 — Create message signals and carriers

3 — Modulate each message (standard AM DSB-SC) and form FDM signal

4 — Frequency domain (spectrum) of FDM signal

5 — (Optional) Add AWGN noise to FDM signal

6 — Receiver: isolate each channel with bandpass filter

7 — Demodulate each isolated channel (coherent) and low-pass filter to recover baseband
__PROGRAM__:
```asm
clc;
clear;
close;

// ---------- settings ----------
PI = 3.14;                // use 3.14 as requested
fs = 80000;               // high sampling rate -> smooth plots
t = 0:1/fs:0.01;          // time vector

fm = [200, 400, 600, 800, 1000, 1200];       // message freqs (Hz)
fc = [5000, 7000, 9000, 11000, 13000, 15000]; // carrier freqs (Hz)

// ---------- generate message signals ----------
m = zeros(length(fm), length(t));
for i = 1:length(fm)
    m(i, :) = sin(2 * PI * fm(i) * t);
end

// ---------- multiplex (AM with cosine carriers) ----------
fdm = zeros(1, length(t));
for i = 1:length(fc)
    carrier = cos(2 * PI * fc(i) * t);
    fdm = fdm + m(i, :) .* carrier;
end

// ---------- design FIR low-pass filter (sinc * Hamming) ----------
fc_cut = 2000;           // cutoff 2 kHz (pass message band)
M = 200;                 // half filter order -> filter length = 2*M+1 = 401
n = -M:M;
wc = 2 * fc_cut / fs;    // normalized (0..1) where 1 => Nyquist

// ideal sinc (note: use PI for pi)
sinc = zeros(n);
for k = 1:length(n)
    x = 2 * fc_cut * n(k) / fs;               // x = 2*fc_cut*n/fs
    if x == 0 then
        sinc(k) = 1;
    else
        // sinc(x) = sin(pi * x) / (pi * x)
        sinc(k) = sin(PI * x) / (PI * x);
    end
end

// ideal impulse response scaled by cutoff (2*fc_cut/fs)
h_ideal = (2 * fc_cut / fs) * sinc;

// apply Hamming window
w = 0.54 - 0.46 * cos(2 * PI * (n + M) / (2*M)); // Hamming length 2M+1
h = h_ideal .* w;

// normalize filter gain at DC
h = h / sum(h);

// ---------- demodulate (coherent detection + LPF) ----------
demod = zeros(length(fm), length(t));
for i = 1:length(fc)
    x = fdm .* cos(2 * PI * fc(i) * t);     // mix to baseband
    y = conv(x, h, 'same');                 // filter (same length)
    demod(i, :) = y;
end

// ---------- plots ----------
scf(1); clf;
for i = 1:size(m,1)
    subplot(size(m,1),1,i);
    plot(t, m(i,:));
end

scf(2); clf;
plot(t, fdm);

scf(3); clf;
for i = 1:size(demod,1)
    subplot(size(demod,1),1,i);
    plot(t, demod(i,:));
end

disp("Done: smooth demodulated signals.");
```

__Output_:
<img width="1918" height="1032" alt="image" src="https://github.com/user-attachments/assets/3aeca046-22e9-4a4c-bf0b-733a94f6071d" />
<img width="1917" height="1041" alt="image" src="https://github.com/user-attachments/assets/bab73ab1-0e7a-4897-9c47-6454c658b4f1" />
<img width="1917" height="1105" alt="image" src="https://github.com/user-attachments/assets/289715d0-7988-405d-8900-c3dbc1b9b9d9" />

![WhatsApp Image 2025-11-20 at 23 07 06_0b1a1678](https://github.com/user-attachments/assets/874b1d92-770b-4e31-9a82-27bf53b22a38)


__Result__:
Thus, the maximum range of a radar system calculated using the Radar Range Equation is successfully verified using Scilab programming.
