# Intermediate Project 2: Signal Frequency Analyzer

## Goal

Generate or load a sampled sensor signal, inspect it in the time and frequency domains, estimate its dominant frequencies, and evaluate the effect of noise reduction.

## Learning outcomes

This project practices sampling, vectorized signal generation, the discrete Fourier transform, frequency-axis construction, filtering, and evidence-based interpretation of plots.

## Signal definition

Use a sampling frequency of 1,000 Hz and a duration of 2 seconds. Construct a signal containing 50 Hz and 120 Hz components plus Gaussian noise:

```matlab
fs = 1000;
t = (0:1/fs:2-1/fs)';
signalClean = 1.0*sin(2*pi*50*t) + 0.5*sin(2*pi*120*t);
rng(7);
noise = 0.35*randn(size(t));
signalNoisy = signalClean + noise;
```

## Requirements

Plot the first 0.25 seconds of the clean and noisy signals. Compute the FFT of the noisy signal, construct a one-sided amplitude spectrum, and plot frequencies from 0 to the Nyquist frequency. Identify the two strongest nonzero frequency peaks and report their estimated frequencies.

Apply a moving-average filter or a simple frequency-domain mask, then compare the filtered signal with the clean reference. Calculate the root-mean-square error before and after filtering. Save a figure named `signal_frequency_analysis.png`.

## Suggested FFT implementation

```matlab
N = numel(signalNoisy);
fftValues = fft(signalNoisy);
amplitude = abs(fftValues/N);
amplitude = amplitude(1:floor(N/2)+1);
amplitude(2:end-1) = 2*amplitude(2:end-1);
frequency = fs*(0:floor(N/2))/N;

[peaks, locations] = findpeaks(amplitude(2:end), ...
    'SortStr', 'descend', 'NPeaks', 2);
estimatedFrequencies = frequency(locations + 1);
```

If `findpeaks` is unavailable, sort the amplitude vector after excluding the DC bin and select the two largest bins. Document that FFT frequency resolution depends on the signal duration and sample count.

## Validation checklist

The spectrum should show strong components near 50 Hz and 120 Hz. Confirm that the frequency vector has the same length as the one-sided amplitude vector. Compare RMS error before and after filtering and explain whether the filter improves the signal without excessively changing the desired components.

## Extensions

Study spectral leakage by changing the duration, apply a Hann window, compare several filter widths, or package the analysis into a function that returns dominant frequencies and RMS error.
