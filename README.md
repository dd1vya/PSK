# PSK & QPSK
# Aim
Write a simple Python program for the modulation and demodulation of PSK and QPSK.
# Tools required
Google Colab
# Program
## PSK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs, fc, br = 1000, 50, 10
t = np.linspace(0, 1, fs, endpoint=False)

bits = np.random.randint(0, 2, br)
spb = fs // br
m = np.repeat(bits, spb)

c = np.sin(2*np.pi*fc*t)
psk = np.sin(2*np.pi*fc*t + np.pi*m)

dem = psk * c
b, a = butter(5, fc/(fs/2))
f = lfilter(b, a, dem)

dec = (f[::spb] < 0).astype(int)

plt.figure(figsize=(12,8))

plt.subplot(4,1,1)
plt.plot(t, m)
plt.title('Message Signal')
plt.grid()

plt.subplot(4,1,2)
plt.plot(t, c)
plt.title('Carrier Signal')
plt.grid()

plt.subplot(4,1,3)
plt.plot(t, psk)
plt.title('PSK Modulated Signal')
plt.grid()

plt.subplot(4,1,4)
plt.step(np.arange(len(dec)), dec, marker='x')
plt.title('Decoded Bits')
plt.grid()

plt.tight_layout()
plt.show()
```
## QPSK
```
import numpy as np
import matplotlib.pyplot as plt

bits = ['10','11','11','10']
t = np.arange(-np.pi, np.pi, 0.1)

pm = {'00':np.pi/4,'01':3*np.pi/4,'10':5*np.pi/4,'11':7*np.pi/4}

mod = np.concatenate([np.sin(t + pm[b]) for b in bits])
inp = np.array([int(x) for b in bits for x in b])

inp_wave = np.repeat(inp, 2)
inp_time = np.repeat(np.arange(len(inp)), 2)

dem = []
ptr = 20

for i in range(len(bits)):
    v = mod[i*len(t) + ptr]
    dem += [1,0] if v>0.7 else [1,1] if v>0 else [0,1] if v>-0.7 else [0,0]

dem_wave = np.repeat(dem, 2)
dem_time = np.repeat(np.arange(len(dem)), 2)

plt.figure(figsize=(10,6))

plt.subplot(3,1,1)
plt.plot(inp_time, inp_wave, drawstyle='steps-post')
plt.title('Input Bainar Data')
plt.ylim(-0.5,1.5)
plt.grid()

plt.subplot(3,1,2)
plt.plot(mod)
plt.title('QPSK Modulated Signal')
plt.grid()

plt.subplot(3,1,3)
plt.plot(dem_time, dem_wave, drawstyle='steps-post')
plt.title('Demodulated Signal')
plt.ylim(-0.5,1.5)
plt.grid()

plt.tight_layout()
plt.show()
```
# Output Waveform
## PSK
<img width="1190" height="790" alt="image" src="https://github.com/user-attachments/assets/4356c6b2-2614-4809-9abd-b6278e289e4a" />


## QPSK
<img width="989" height="590" alt="image" src="https://github.com/user-attachments/assets/fcaa4ecd-154d-479b-9d62-f6a864c86824" />


# Results
The modulation and demodulation of PSK and QPSK is executed correctly.
