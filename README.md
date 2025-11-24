**SIMULATION OF FREQUENCY DIVISION MULTIPLEXING (FDM) AND DEMULTIPLEXING USING SCILAB**

**AIM:**

To write a Scilab program to simulate frequency division multiplexing and demultiplexing for six different frequencies, and verify the demultiplexed outputs correspond to the original signals

**EQUIPMENTS Needed**

computer with i3 processor
scilab

**ALGORITHM**

Define six different frequencies to generate six sine wave signals.

Generate the time vector to represent time samples.

Compute six sine signals for each frequency over the time vector.

Frequency Division Multiplexing: sum all six sine signals to make one multiplexed signal.

Frequency Division Demultiplexing: for each frequency, multiply the multiplexed signal by a sine wave of that frequency (mixing), then apply a lowpass filter to extract the baseband (original) signal.

Plot original signals, multiplexed signal, and demultiplexed signals for verification

**PROGRAM**
```
clc;
clear;
close;

// ---------- PARAMETERS ----------
fs = 22400;                // Sampling frequency
t = 0:1/fs:0.02;           // Time vector (20 ms)

// ---------- MESSAGE SIGNALS ----------
m1 = sin(2*%pi*224*t);
m2 = sin(2*%pi*324*t);
m3 = sin(2*%pi*424*t);
m4 = sin(2*%pi*524*t);
m5 = sin(2*%pi*624*t);
m6 = sin(2*%pi*724*t);

// ---------- CARRIER FREQUENCIES ----------
fc = [2240 3240 4240 5240 6240 7240];

// ---------- MODULATION (DSB-SC) ----------
s1 = m1 .* cos(2*%pi*fc(1)*t);
s2 = m2 .* cos(2*%pi*fc(2)*t);
s3 = m3 .* cos(2*%pi*fc(3)*t);
s4 = m4 .* cos(2*%pi*fc(4)*t);
s5 = m5 .* cos(2*%pi*fc(5)*t);
s6 = m6 .* cos(2*%pi*fc(6)*t);

// ---------- MULTIPLEXING ----------
fdm = s1 + s2 + s3 + s4 + s5 + s6;

// ---------- BANDPASS FILTER (For Demultiplexing) ----------
function y = bandpass(x, f_low, f_high, fs)
    N = length(x);
    X = fft(x);
    f = (0:N-1)*(fs/N);

    H = zeros(1,N);
    H( f > f_low & f < f_high ) = 1;
    H( f > (fs - f_high) & f < (fs - f_low) ) = 1;

    Y = X .* H;
    y = real(ifft(Y));
endfunction

bw = 800;  // bandwidth around carrier

// ---------- DEMULTIPLEXING ----------
dm1 = bandpass(fdm, fc(1)-bw, fc(1)+bw, fs);
dm2 = bandpass(fdm, fc(2)-bw, fc(2)+bw, fs);
dm3 = bandpass(fdm, fc(3)-bw, fc(3)+bw, fs);
dm4 = bandpass(fdm, fc(4)-bw, fc(4)+bw, fs);
dm5 = bandpass(fdm, fc(5)-bw, fc(5)+bw, fs);
dm6 = bandpass(fdm, fc(6)-bw, fc(6)+bw, fs);

// ---------- PLOTTING ----------

// 1. Original message baseband signals
figure(1);
subplot(3,2,1); plot(t,m1); title('Message Signal 1');
subplot(3,2,2); plot(t,m2); title('Message Signal 2');
subplot(3,2,3); plot(t,m3); title('Message Signal 3');
subplot(3,2,4); plot(t,m4); title('Message Signal 4');
subplot(3,2,5); plot(t,m5); title('Message Signal 5');
subplot(3,2,6); plot(t,m6); title('Message Signal 6');
xlabel("Time (s)");

// 2. FDM Multiplexed Signal
figure(2);
plot(t, fdm);
title('FDM Multiplexed Signal');
xlabel("Time (s)");

// 3. Demultiplexed (Bandpass Retrieved) Signals
figure(3);
subplot(3,2,1); plot(t,dm1); title('Demultiplexed Channel 1');
subplot(3,2,2); plot(t,dm2); title('Demultiplexed Channel 2');
subplot(3,2,3); plot(t,dm3); title('Demultiplexed Channel 3');
subplot(3,2,4); plot(t,dm4); title('Demultiplexed Channel 4');
subplot(3,2,5); plot(t,dm5); title('Demultiplexed Channel 5');
subplot(3,2,6); plot(t,dm6); title('Demultiplexed Channel 6');
xlabel("Time (s)");
```
**OUTPUT:**
<img width="800" height="700" alt="image" src="https://github.com/user-attachments/assets/1cfb6e15-7e1c-4b7a-b50a-f16e732f4496" />

<img width="800" height="700" alt="image" src="https://github.com/user-attachments/assets/e3e95f0f-2e7f-4e96-a12f-97b59907ba83" />

<img width="800" height="455" alt="image" src="https://github.com/user-attachments/assets/6322636e-336d-4c91-ad12-8fe83016a56d" />

**TABULATION**


**RESULTS**

The program successfully simulates FDM and demultiplexing for multiple frequency signals with filtering to recover original signals accurately in Scilab.



