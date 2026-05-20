# Pulse-Code-Modulation
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Tools required
GOOGLE COLAB
# Program
```
# PCM
import numpy as np
import matplotlib.pyplot as plt

fs, f, d, L = 5000, 50, 0.1, 16
t = np.linspace(0, d, int(fs*d), endpoint=False)
msg = np.sin(2*np.pi*f*t)

clk = np.sign(np.sin(2*np.pi*200*t))

step = (msg.max() - msg.min()) / L
q = np.round(msg/step) * step
pcm = ((q - q.min())/step).astype(int)

plt.figure(figsize=(10,8))
plt.subplot(4,1,1); plt.plot(t,msg); plt.title("Message"); plt.grid()
plt.subplot(4,1,2); plt.plot(t,clk); plt.title("Clock"); plt.grid()
plt.subplot(4,1,3); plt.step(t,q); plt.title("PCM Signal"); plt.grid()
plt.subplot(4,1,4); plt.plot(t,q,'--'); plt.title("Demodulated"); plt.grid()
plt.tight_layout(); plt.show()


# DM
from scipy.signal import butter, filtfilt

fs, f, T, delta = 10000, 10, 1, 0.1
t = np.arange(0, T, 1/fs)
msg = np.sin(2*np.pi*f*t)

enc, dm = [], [0]
prev = 0

for s in msg:
    bit = 1 if s > prev else 0
    enc.append(bit)
    prev += delta if bit else -delta
    dm.append(prev)

demod = [0]
for b in enc:
    demod.append(demod[-1] + (delta if b else -delta))
demod = np.array(demod)

b,a = butter(4, 20/(0.5*fs), 'low')
filt = filtfilt(b, a, demod)

plt.figure(figsize=(10,6))
plt.subplot(3,1,1); plt.plot(t,msg); plt.title("Original"); plt.grid()
plt.subplot(3,1,2); plt.step(t,dm[:-1],where='mid'); plt.title("DM Signal"); plt.grid()
plt.subplot(3,1,3); plt.plot(t,filt[:-1],'r:'); plt.title("Demodulated"); plt.grid()
plt.tight_layout(); plt.show()
```
# Output Waveform
```
```
<img width="990" height="590" alt="pulse" src="https://github.com/user-attachments/assets/07f102e2-409b-4f15-8c3a-23f6635ec1fc" />

```
# Results
```
Thus, the PCM and DC is verified successfully
```

