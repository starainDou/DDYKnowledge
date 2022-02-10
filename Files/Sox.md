[Sox特性](http://sox.sourceforge.net/Docs/Features)

* Tone/filter effects  音色/滤波

```
全通(allpass): RBJ all-pass biquad IIR filter
带通(bandpass): RBJ band-pass biquad IIR filter
带阻(bandreject): RBJ band-reject biquad IIR filter
通带(band): SPKit resonator band-pass IIR filter
均衡(equalizer): RBJ peaking equalisation biquad IIR filter
FFTFIR滤波(firfit+): FFT convolution FIR filter using given freq. response (W.I.P.)
高通(highpass): High-pass filter: Single pole or RBJ biquad IIR
希尔伯特滤波器(hilbert): Hilbert transform filter (90 degrees phase shift)
低通(lowpass): Low-pass filter: single pole or RBJ biquad IIR
sinc滤波器(sinc): Sinc-windowed low/high-pass/band-pass/reject FIR
高音(treble): Tone control: RBJ shelving biquad IIR filter
```

* Production effects  附加效果

```
合唱(chorus): Make a single instrument sound like many
延时(delay): Delay one or more channels
回声(echo): Add an echo
多回声(echos): Add a sequence of echos
法兰效应(flanger): Stereo flanger
过载(overdrive): Non-linear distortion
相移(phaser): Phase shifter
重复(repeat): Loop the audio a number of times
混响(reverb): Add reverberation
翻转(reverse): Reverse the audio (to search for Satanic messages 😉
颤音(tremolo): Sinusoidal volume modulation
```

* Volume/level effects

```
幅度变化(compand): Signal level compression/expansion/limiting
对比化(contrast): Phase contrast volume enhancement
去直流(dcshift): Apply or remove DC offset
淡入淡出(fade): Apply a fade-in and/or fade-out to the audio
增益变化(gain): Apply gain or attenuation; normalise/equalise/balance/headroom
响度控制(loudness): Gain control with ISO 226 loudness compensation
多带幅度变化(mcompand): Multi-band compression/expansion/limiting
归一化(norm): Normalise to 0dB (or other)
音量调节(vol): Adjust audio volume
```

* Editing effects

```
拉长(pad): Pad (usually) the ends of the audio with silence
去除静音(silence): Remove portions of silence from the audio
结合(splice): Perform the equivalent of a cross-faded tape splice
裁剪(trim): Cuts portions out of the audio
VAD: Voice activity detector
```

* Mixing effects 混音

```
声道混音(channels): Auto mix or duplicate to change number of channels
分声道(divide+): Divide sample values by those in the 1st channel (W.I.P.)
任意输出混音(remix): Produce arbitrarily mixed output channels
翻转声道(swap): Swap stereo channels
```

* Pitch/tempo effects 音调/节拍

```
弯曲声调(bend): Bend pitch at given times without changing tempo
变调(pitch): Adjust pitch (= key) without changing tempo
变速(speed): Adjust pitch & tempo together
拉伸压缩(stretch): Adjust tempo without changing pitch (simple alg.)
变节拍(tempo): Adjust tempo without changing pitch (WSOLA alg.)
```

* Mastering effects 母带

```
加入噪声(dither): Add dither noise to increase quantisation SNR
重采样(rate): Change audio sampling rate
```

* Specialised filters/mixers 特殊滤波器/混音器

```
去加重(deemph): ISO 908 CD de-emphasis (shelving) IIR filter
耳机增强(earwax): Process CD audio to best effect for headphone use
去噪声(noisered): Filter out noise from the audio
卡拉ok效应(oops): Out Of Phase Stereo (or `Karaoke’) effect
RIAA: RIAA vinyl playback equalisation
```

* Analysis effects 效果分析

```
噪声分析(noiseprof): Produce a DFT profile of the audio (use with noisered)
频谱分析(spectrogram): graph signal level vs. frequency & time (needs libpng)
状态分析(stat): Enumerate audio peak & RMS levels, approx. freq., etc.
多声道状态(stats): Multichannel aware `stat’
```

* Miscellaneous effects 其他效果

```
LADSPA: Apply LADSPA plug-in effects e.g. CMT (Computer Music Toolkit)
合成声音(synth): Synthesise/modulate audio tones or noise signals
新文件(newfile): Create a new output file when an effects chain ends.
重试(retart): Restart 1st effects chain when multiple chains exist.
```

* Low-level signal processing effects 底层信号处理

```
滤波(biquad): 2nd-order IIR filter using externally provided coefficients
降采样(downsample): Reduce sample rate by discarding samples
fir滤波(fir): FFT convolution FIR filter using externally provided coefficients
升采样(upsample): Increase sample rate by zero stuffing
```


```
sox file1.wav -v 0.6 file2.wav
-v 是调节音量的选项，0.6是参数，它是一种线性调整，并不是调整到原来的0.6，而是幅值调整，如果参数大于1则增加音量，反之减小，取值太大会出现削波现象

sox file1.wav -n stat -v
1.0003，输出值为不失真最大调整量，stat为效果器，作用是对音频文件做一个统计分析并将结果打印到标准错误文件，-v将打印与音量相关的以Volume Adjustment开头的那行，-n表示输出文件为空，这样在不干扰音频文件的情况下得到文件信息
```

[参考](https://www.shuzhiduo.com/A/1O5EgvMaz7/)   
[参考](https://github.com/pxhbug123/SoxLibInAndroid/blob/77038094325ebaddc2d0919a88c20127bdd2c70c/soxcommandlibrary/src/main/sox/src/JniNative.c)   
[参考](https://github.com/samychen/ffmpegplayer/blob/6e50d5314f2bfaf9300ab6438c1cb2e484bab404/app/src/main/cpp/native-lib.cpp)  
[参考](https://github.com/shieldlock/SoX-iPhone-Lib/blob/master/AudioEffects/AudioEffects/ViewController.m)   
[参考](https://github.com/mixflame/MixDJ/blob/c125d7372a434a3353c2269fe27c00e52a2d2f3e/MixDJ/soxEncode.m)   
[编译](https://github.com/KelvinJin/libsox-iOS)