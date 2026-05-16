# Pulse-Code-Modulation
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Tools required
Personal Laptop
Google Colab
# Program
```
# ==============================
# PCM - Pulse Code Modulation
# ==============================

import numpy as np
import matplotlib.pyplot as plt

# Parameters
sampling_rate = 5000      # Sampling rate (samples per second)
frequency = 50            # Frequency of message signal
duration = 0.1            # Duration of signal
quantization_levels = 16  # Number of quantization levels

# Generate time vector
t = np.linspace(0, duration,
                int(sampling_rate * duration),
                endpoint=False)

# Generate message signal (Analog Signal)
message_signal = np.sin(2 * np.pi * frequency * t)

# Generate clock signal
clock_signal = np.sign(np.sin(2 * np.pi * 200 * t))

# Quantization
quantization_step = (
    max(message_signal) - min(message_signal)
) / quantization_levels

quantized_signal = (
    np.round(message_signal / quantization_step)
    * quantization_step
)

# PCM digital signal
pcm_signal = (
    (quantized_signal - min(quantized_signal))
    / quantization_step
)

pcm_signal = pcm_signal.astype(int)

# ------------------------------
# Plotting PCM Signals
# ------------------------------

plt.figure(figsize=(12, 10))

# Message Signal
plt.subplot(4, 1, 1)
plt.plot(t, message_signal,
         label="Message Signal (Analog)",
         color='blue')
plt.title("Message Signal (Analog)")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)

# Clock Signal
plt.subplot(4, 1, 2)
plt.plot(t, clock_signal,
         label="Clock Signal",
         color='green')
plt.title("Clock Signal (Increased Frequency)")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)

# PCM Modulated Signal
plt.subplot(4, 1, 3)
plt.step(t, quantized_signal,
         label="PCM Modulated Signal",
         color='red')
plt.title("PCM Modulated Signal (Quantized)")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)

# PCM Demodulated Signal
plt.subplot(4, 1, 4)
plt.plot(t, quantized_signal,
         label="PCM Demodulation Signal",
         color='purple',
         linestyle='--')
plt.title("PCM Demodulation Signal")
plt.xlabel("Time [s]")
plt.ylabel("Amplitude")
plt.grid(True)

plt.tight_layout()
plt.show()


# ==============================
# DM - Delta Modulation
# ==============================

import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt

# Parameters
fs = 10000       # Sampling frequency
f = 10           # Signal frequency
T = 1            # Duration
delta = 0.1      # Step size

# Time vector
t = np.arange(0, T, 1/fs)

# Message Signal
message_signal = np.sin(2 * np.pi * f * t)

# Delta Modulation Encoding
encoded_signal = []
dm_output = [0]
prev_sample = 0

for sample in message_signal:

    if sample > prev_sample:
        encoded_signal.append(1)
        dm_output.append(prev_sample + delta)

    else:
        encoded_signal.append(0)
        dm_output.append(prev_sample - delta)

    prev_sample = dm_output[-1]

# Delta Demodulation
demodulated_signal = [0]

for bit in encoded_signal:

    if bit == 1:
        demodulated_signal.append(
            demodulated_signal[-1] + delta
        )

    else:
        demodulated_signal.append(
            demodulated_signal[-1] - delta
        )

# Convert to NumPy array
demodulated_signal = np.array(demodulated_signal)

# Low-pass Butterworth Filter
def low_pass_filter(signal, cutoff_freq, fs, order=4):

    nyquist = 0.5 * fs
    normal_cutoff = cutoff_freq / nyquist

    b, a = butter(order,
                  normal_cutoff,
                  btype='low',
                  analog=False)

    return filtfilt(b, a, signal)

# Filtered output
filtered_signal = low_pass_filter(
    demodulated_signal,
    cutoff_freq=20,
    fs=fs
)

# ------------------------------
# Plotting DM Signals
# ------------------------------

plt.figure(figsize=(12, 6))

# Original Signal
plt.subplot(3, 1, 1)
plt.plot(t,
         message_signal,
         label='Original Signal',
         linewidth=1)

plt.legend()
plt.grid()

# Delta Modulated Signal
plt.subplot(3, 1, 2)
plt.step(t,
         dm_output[:-1],
         label='Delta Modulated Signal',
         where='mid')

plt.legend()
plt.grid()

# Demodulated & Filtered Signal
plt.subplot(3, 1, 3)
plt.plot(t,
         filtered_signal[:-1],
         label='Demodulated & Filtered Signal',
         linestyle='dotted',
         linewidth=1,
         color='r')

plt.legend()
plt.grid()

plt.tight_layout()
plt.show()
```
# Output Waveform

<img width="840" height="918" alt="Screenshot 2026-05-16 102413" src="https://github.com/user-attachments/assets/92aa0fb3-55da-43ab-97c3-fa87b5013623" />

# Results
Thus, The Python program for the modulation and demodulation of PCM, and DM is verified Successfully
