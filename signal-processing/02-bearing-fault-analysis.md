# Signal Processing Project 2: Bearing-Fault Feature Analysis

## Goal

Analyze a real vibration recording from a rotating machine, identify periodic components and impulsive behavior, and build an interpretable feature-extraction workflow for condition comparison.

## Input and learning outcomes

Use a public vibration dataset or a recording collected from an accelerometer, and document the source, sensor location, sampling frequency, and machine condition labels. The project practices resampling checks, detrending, windowing, FFT analysis, spectrograms, envelope detection, and feature validation.

## Requirements

Load at least two labeled conditions, such as healthy and fault, and select equal-duration segments. Remove the mean and linear trend. Calculate time-domain features including RMS, peak-to-peak value, crest factor, and kurtosis. Compute a one-sided power spectrum and a spectrogram for each condition. Identify frequency bands where the conditions differ and explain whether the difference is stable across multiple windows.

Implement an envelope-analysis path by band-pass filtering around a documented resonance band, taking the analytic-signal magnitude with `hilbert`, and examining the envelope spectrum. Create a multi-panel figure saved as `bearing_fault_features.png` and export a feature table as `bearing_features.csv`.

## Suggested workflow

```matlab
[x, fs] = audioread('vibration_condition.wav'); % Replace with sensor reader as needed
x = detrend(x(:));
N = numel(x);
rmsValue = rms(x);
peakToPeak = max(x) - min(x);
crestFactor = max(abs(x))/rmsValue;
kurtosisValue = kurtosis(x);

window = hann(min(4096, N));
overlap = floor(0.5*numel(window));
[s, f, t] = spectrogram(x, window, overlap, [], fs);
powerSpectrum = abs(s).^2;

[b, a] = butter(4, [1000 5000]/(fs/2), 'bandpass');
bandSignal = filtfilt(b, a, x);
envelope = abs(hilbert(bandSignal));
```

If the dataset has unequal sampling rates, resample carefully and record the method. Never infer a fault solely from one spectral peak; require evidence across windows and conditions.

## Validation checklist

Verify that frequency units are correct, the FFT frequency axis ends at the Nyquist frequency, and each feature is computed on the same duration. Compare feature distributions rather than only their means. Use a held-out segment to test whether the proposed features reproduce the condition separation.

## Extensions

Add order tracking when rotational speed varies, compare multiple sensors, use wavelets for transient events, or train a simple classifier only after the signal-processing features have been independently validated.

## References

1. [MathWorks: Fast Fourier Transform](https://www.mathworks.com/discovery/fft.html)
