# Signal Processing Project 1: Audio Denoising and Spectral Evaluation

## Goal

Build a reproducible MATLAB workflow that removes stationary noise from an audio recording, compares time-domain and frequency-domain behavior, and measures the trade-off between noise reduction and signal distortion.

## Input and learning outcomes

Use a real speech or environmental recording in WAV format, such as a recording created by the student or a properly licensed public sample. Read it with `audioread`, convert stereo to mono when necessary, and preserve the original sample rate. The project practices FFT analysis, spectrograms, filter design, zero-phase filtering, and quantitative evaluation.

## Requirements

Inspect the waveform and magnitude spectrum of the recording. Estimate the dominant noise band from a short segment that contains mostly background noise. Design a reproducible band-stop or low-pass filter based on that observation, apply it with `filtfilt` when appropriate, and compare the original and filtered audio using time plots and spectrograms. Save `audio_denoising_comparison.png` and write the cleaned signal to `denoised_audio.wav`.

Calculate RMS level before and after filtering. If a clean reference is available, also calculate signal-to-noise ratio improvement and normalized mean-square error. If no clean reference exists, clearly label the evaluation as a proxy measure rather than a true SNR estimate.

## Suggested workflow

```matlab
[x, fs] = audioread('recording.wav');
if size(x, 2) == 2
    x = mean(x, 2);
end
x = x - mean(x);

nfft = 2^nextpow2(numel(x));
f = (0:nfft/2)'*fs/nfft;
spectrum = abs(fft(x, nfft));
spectrum = spectrum(1:nfft/2+1);

% Choose cutoff frequencies after inspecting the spectrum.
[b, a] = butter(6, 400/(fs/2), 'low');
y = filtfilt(b, a, x);
y = y / max(1, max(abs(y)));
audiowrite('denoised_audio.wav', y, fs);
```

For nonstationary noise, compare the filter approach with spectral subtraction or short-time spectral gating. Document filter order, cutoff frequencies, edge handling, and any normalization.

## Validation checklist

Listen to both files at a safe volume, confirm the output has the same sample rate, check for clipping, and inspect spectrograms for lost speech or tonal content. Compare RMS values and explain why a lower RMS does not automatically mean better audio quality.

## Extensions

Implement an interactive parameter sweep, compare Butterworth and FIR designs, evaluate the short-time Fourier transform, or create a small report that ranks parameter settings using a chosen objective function.

## References

1. [MathWorks: Fast Fourier Transform](https://www.mathworks.com/discovery/fft.html)
