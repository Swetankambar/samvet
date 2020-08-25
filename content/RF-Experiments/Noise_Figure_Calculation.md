---
date: 2019-04-12
tags: ["RF"]
title: "Calculation of Noise Figure of a Radio receiver"
menu: "RF Game"
---


# Noise Figure Calculation


I got a USRP device at hand and the post uses it as the radio whose noise figure has to be estimated. The method described below is generic and is valid for any RF receiver.

{{< katex inline>}}
Noise\, Figure= 10*log_{10}(Noise \,Factor) \\[3mm]

Noise\, Factor=\frac{SNR_{IN}}{SNR_{OUT}}\\[3mm]
{{< /katex >}}


Following approach is taken to calculate the NF experimentally:

* Provide input to the USRP RIO using a known signal generator with a low amplitude ~-70dBm. This signal should be measured by a VSA or SA to check the signal input to USRP RIO.
* Set the gain setting on USRP Recieve channel to maximum, {{< katex inline >}}{G= 95\,dB} {{< /katex >}} 

* Set the BW of capture to be {{< katex inline >}}{10 \,MHz}{{< /katex >}}
* Set the number of points to be captured to be 1024
*Acquire signal from a USRP channel and plot the power spectrum to observe the signal level
* Ensure that the USRP is not running in saturation with the given gain and the input signal. This can be verified by changing the input signal by 10dB and observing an approx 10dB change in the spectrum captured. Do ensure that the signal is above the noise floor as seen in the power spectrum
* Remove any input signal from USRP input and put a {{< katex inline >}}50 {\Omega}{{< /katex >}} termination at the input. Now acquire the spectrum using the same settings as before to observe the noise floor.
* Apply the following formula for calculation


{{< katex inline>}}  Noise Figure=10*log_{10}(\frac{N_{out}}{N_{in}*Gain})\thickspace \dots(1)\\[5mm]{{< /katex >}}
 {{< katex inline >}}         {N_{out}} {{< /katex >}} and  {{< katex inline >}}{N_{in}} {{< /katex >}}are both calculated in per Hertz.


{{< katex inline>}}  10log_{10}(N_{in})= 174dBm/Hz\dots(2)\\[5mm]{{< /katex >}}


{{< katex >}}  10*log_{10}(N_{out}\, per \,Hertz) =Noise \,Floor(dBm)- 10*log_{10}(Effective \,Noise \,Bandwidth)\\[5mm]{{< /katex >}}

{{< katex >}}  Effective\,Noise\,Bandwidth = \frac{Sampling\,Bandwidth}{FFT\,points}*Window\,Scaling\,Factor{{< /katex >}}

[FFT Scaling For Noise](https://www.ap.com/technical-library/fft-scaling-for-noise/)  contains the details of the Scaling Factor that should be used for different window types. Default window used in "FFT Power Spectrum and PSD VI" used in "Rx Streaming(Four Channel)" is hanning, for with Scaling factor of 1.5. Thus

{{< katex inline >}}  Effective\,Noise\,Bandwidth=\frac{10\,MHz}{1024}*1.5=14648.43\,Hz\\[3mm]{{< /katex >}}

{{< katex inline >}}  10*log_{10}(Effective,Noise,Bandwidth)\approx 41.65\dots(3)\\[5mm]{{< /katex >}}

{{< katex >}}  10*log(Gain)= Power \, Observed \, in \, Spectrum - Input \, Power \, Level\dots(4)\\[5mm]{{< /katex >}} Say this as K.  
Find average noise floor from the power spectrum, say this as N

Plugging (2),(3),(4) in 1 we get

{{< katex inline >}}  NF= N-41.65+174-K{{< /katex >}}

This is an approximate calculation as the noise floor is generally not constant throughout, a bunch of RMS averaging should be done to make the noise power measurement. This will give a more accurate N used above and hence a more accurate NF.
