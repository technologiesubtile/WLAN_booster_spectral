Spectral characterization af the EP-AB003 WLAN booster
heinrich.diesinger@gmx.net, F4HYZ

The EP-AB003 should make a nice radio frontend for 2.4 GHz (and possibly more!) ham radio both as transmitter and receiver. After the integrated SAW (?) filter has attracted my curiosity, i acquire some spectra using the nano VNA. In a previous post i have presented a modification consisting in adding an optional RX port for connecting the booster to a SDR transceiver with separate Rx/Tx ports. The spectral characterization presented here can be performed both on the modified and on the original version of the EP-AB003.


Reminder about the drawbacks and limitations of the nanoVNA: 

- the stimulus power of this VNA is not adjustable, not known and difficult to measure; it will empirically be taken to an appropriate level using a combination of attenuators and preamplifiers to test the RF paths in a realistic scenario.

- the fundamental mode is not attenuated even when working on harmonics, therefore two problems: 1) the stimulus power is not reliably measurable with a power meter, 2) S-parameters of active nonlinear components are likely overestimated if the DUT generates itself harmonics from the fundamental.

See the manual : https://qsl.net/g0ftd/other/nano-vna-original/docs/NanoVNA%20User%20Guide-English-reformat-Oct-2-19.pdf

Nevertheless some spectra will be recorded to determine qualitatively the response of the two amplification branches. I start by the receive path. 

![P4262729](https://user-images.githubusercontent.com/96028811/234713957-c27145bb-7f71-42cd-9b24-5bd601eea3e7.JPG)

Therefore the injection is on the "Antenna" side of the booster and the measurement is at the new optional Rx output (it could also be on the "router" port also on a non-modified booster). Typical signal levels for WiFi are between -100 .. -50 dBm. To not overdrive the amplifier, i start by inserting - 50 dB of attenuation between the stimulus and the antenna port of our booster. The stimulus in the GHz range is a harmonic and should be far below 1 mW, say 0.1 mW or -10 dBm. Keeping the level at a reasonable value is to prevent the booster from commuting into TX mode, because on the "Router" side, the port with its carrier detector is by default connected to the receive branch and when the threshold of 3 .. 5 dBm is exceeded, it would commute to Tx mode. Then, i progressively remove attenuators to inject more power.

![P4262731](https://user-images.githubusercontent.com/96028811/234714046-a4c42822-6edd-4870-9fad-1c10ed68bc43.JPG)

With -50 dB attenuation, i obtain a nice spectrum showing on one hand the response of the SAW filter, and on the other hand the gain that is realistically close to 25 dB as on the manual.

![P4262735](https://user-images.githubusercontent.com/96028811/234714099-2a48a519-7390-42e9-929e-f7b2f4b41c6b.JPG)

With 20 dB of attenuation, i get the same spectrum at an output level increased proportionally to the input. Maybe slightly less, but i would not consider the measurement as accurate enough to conclude on gain compression.

![737cropped](https://user-images.githubusercontent.com/96028811/234714261-824d6ffd-a4ba-4758-8b11-3cd23a8cfca3.jpg)

With only 10 dB attenuation, the spectrum saturates, i dont know if it is the amplifier or the VNA. Therefore, i add - 10 db attenuation to the Opt Rx output and get this spectrum:

![738cropped](https://user-images.githubusercontent.com/96028811/234714358-5c3d32dc-a67c-4673-bfec-1c20a7c93d80.jpg)

With both attenuators the spectrum is no longer cropped, so the previous saturation was on the side of the nanoVNA; however it shows that the gain maximumum is now only 22.5 dB versus the roughly 25 dB measured with 50 dB of attenuators. This clearly demonstrates a gain decrease. The manual does not state an absolute maximum rating for the antennna input signal but the input of roughly - 20 dBm (- 10 dBm stimulus, - 10 dB attenuator) is above all realistic scenarios and i do not want to test whether the input is "bullet proof".
The SAW filtering should tremendously improve the reception in particular in presence of powerful transmitters outside the frequency of interest that otherwise could desensitize the receiver frontend.
I have no means of measuring the noise figure (homebrew NF measurements are just recently emerging with the tinySA, https://tinysa.org/wiki/pmwiki.php?n=Main.NoiseFactor). I therefore trust the 2.5 db from the specs, which I think is comparable to WiFi receiver frontends or slightly better. It is not something close to the 0.1 dB advertized on our satellite LNBs.


Next i measure the transmit path:

![P4262757](https://user-images.githubusercontent.com/96028811/234714590-d646f143-72bd-4832-8737-3345011074f0.JPG)

Therefore, the stimulus of the VNA is preamplified by two SPF5189Z in series by about 20 dB before injecting into the "Router" port of the booster, and the "Antenna" port connected to the VNA input via 40 dB attenuation (two 20 dB attenuators). 

At first i have an additional 3 dB attenuator between the two SPF5189Z as a measure of precaution. The spectrum is this:

![P4262742](https://user-images.githubusercontent.com/96028811/234714699-8b98b1e7-3b77-4f65-a2df-43deb1a9a1ab.JPG)

It appears that the input signal is at the switching threshold of the booster and the status LED flickers between red and green and the spectrum has a discontinuity.

After taking out the 3 dB attenuator i get this spectrum

![P4262744](https://user-images.githubusercontent.com/96028811/234714722-9d13232e-df91-4121-8c72-a64c55d24220.JPG)

and the status LED stays green showing the booster remains in Tx mode. 

In the end, i measure approximately the input power of the booster by this setup

![753cropped](https://user-images.githubusercontent.com/96028811/234714776-144f16b8-4b2c-4467-a043-bb7b09006b3f.jpg)

Note that behind the secnd SPF5189Z, a -20 dB attenuator is inserted to protect the power meter (max: +5 dBm), but this is entered as compensation offset in the power meter, yielding this reading:

![747cropped](https://user-images.githubusercontent.com/96028811/234714845-9b88c02a-7465-4523-9645-3394ab1bc7b1.jpg)

it varies between 7 and 9 dBm, however, this is prone to error because the power meter also measures the fundamental mode. It has no means of frequency selection and the entered frequency is only a calibration factor. However, 9 dBm would corrrespond to somewhat above the expected switching threshold of my modified booster. The nominal value is 3 .. 5 dBm and with the newly added port in parallel it is expected to be some dB higher.

The Tx branch spectrum shows that the output level is as expected, peaking at - 4 dB, corresponding to + 20 dB of the two SPF5189Z, + 16 dB the booster, and - 40 dB of the output attenuators. If the input is assumed to be 9 dBm, we would have 25 dBm output power equaling 316 mW. This is in agreement with the observation that the heatsink of the first output attenuator does not warm up remarkably during some minutes of measurement. The spectrum clearly shows, in contrast to the widespread belief, that the Tx branch indeed does include filtering, although not comparable to the RX branch with its steep SAW filter. It is likely reducing higher harmonics but since the nanoVNA stops at 3 GHz, i cannot determine whether this is sufficient to comply with regulations.


Conclusion:

The WLAN booster contains a surprisingly performant SAW filter in its receive branch. Filtering is also present in the transmit branch although less steep. The characteristics listed in the specs (manual card in the box) can be confirmed, except for the output power that is 5W instead of 8W, and the noise figure that we cannot measure. It is also emphasized that the design of such an amplifier involves difficulties beyond the scope of this review. The price performance ratio seems unbeatabe at a price between 40 and 50 euro. In agreement with other authors, i would not recommend the "always Tx" modification by shorting the pins 4 and 5 of the OpAmp because if used in SSB mode with an appropriate input level, the carrier detection/autoswitching reliably commutes to Tx fast enough and there should be no risk of commuting into Rx mode accidentally during the minima of the audio signal. My sample of the booster is at least the 3rd version that i'm aware of and possibly there are even more. Some time ago, users claimed that they needed to inject as much as 500 mW on the "router" port to obtain the maximum output power of 5 W, but it seems that the later versions have more Tx gain due to a suppressed attenuator and the 5 W output is readily obtained at an input power of 100 mW.

