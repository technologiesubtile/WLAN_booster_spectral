Spectral characterization af the EP-AB003 WLAN booster
heinrich.diesinger@gmx.net, F4HYZ

The EP-AB003 should make a nice radio frontend for 2.4 GHz (and possibly more!) ham radio both as transmitter and receiver. After the integrated SAW (?) filter has attracted my curiosity, i acquire some spectra using the nano VNA. In a previous post i have presented a modification consisting in adding an optional RX port for connecting the booster to a SDR transceiver with separate Rx/Tx ports. The spectral characterization presented here can be performed both on the modified and on the original version of the EP-AB003.


Reminder about the drawbacks and limitations of the nanoVNA: 

- the stimulus power of this VNA is not adjustable, not known and difficult to measure; it will empirically be taken to an appropriate level using a combination of attenuators and preamplifiers to test the RF paths in a realistic scenario.

- the fundamental mode is not attenuated even when working on harmonics, therefore two problems: 1) the stimulus power is not reliably measurable with a power meter, 2) S-parameters of active nonlinear components are likely overestimated if the DUT generates itself harmonics from the fundamental.

See the manual : https://qsl.net/g0ftd/other/nano-vna-original/docs/NanoVNA%20User%20Guide-English-reformat-Oct-2-19.pdf

Nevertheless some spectra will be recorded to determine qualitatively the response of the two amplification branches. I start by the receive path. 


foto setup receive path

Therefore the injection is on the "Antenna" side of the booster and the measurement is at the new optional Rx output (it could also be on the "router" port also on a non-modified booster). Typical signal levels for WiFi are between -100 .. -50 dBm. To not overdrive the amplifier, i start by inserting - 50 dB of attenuation between the stimulus and the antenna port of our booster. The stimulus in the GHz range is a harmonic and should be far below 1 mW. This is to prevent the booster from commuting into TX mod, because on the "Router" side, the port with its carrier detector is by default connected to the receive branch and when the threshold of 3 .. 5 dBm is exceeded, it would commute to Tx mode. Then, i progressively remove attenuators to inject more power.

foto -50 dB, foto schematic

With -50 dB attenuation, i obtain a nice spectrum showing on one hand the response of the SAW filter, and on the other hand the gain that is realistically close to 25 dB as on the manual.

foto -20 dB, schematic

With 20 dB of attenuation, i get the same spectrum at an output level increased proportionally to the input. Maybe sslightly less, but i would not consider the measurement as accurate enough to conclude on amplification compression.


foto -10 dB, schematic
With only 10 dB attenuation, the spectrum saturates, i dont know if it is the amplifier or the VNA. Therefore, i add - 10 db attenuation to the Opt Rx output and get this spectrum:

foto -10 dB, -10 dB Rx port, schematic

With both attenuators the spectrum is no longer cropped, so the previous saturation was on the side of the nanoVNA; however it shows that the gain maximumum is now only 22.5 dB versus the roughly 25 dB measured with 50 dB of attenuators. This clearly demonstrates a gain decrease. The manual does not state an absolute maximum rating for the antennna input signal but the input of roughly - 20 dBm is above all realistic scenarios and i do not want to test whether the inout is "bullet proof".
The SAW filtering should tremendously improve the reception in particular in presence of powerful transmitters outside the frequency of interest that otherwise could desensitize the receiver frontend.
I have no means of measuring the noise figure (homebrew NF measurements are just recently emerging with the tinySA, https://tinysa.org/wiki/pmwiki.php?n=Main.NoiseFactor). I therefore trust the 2.5 db from the specs, which I think is comparable to WiFi receiver frontends or slightly better. It is not something close to the 0.1 dB advertized on our satellite LNBs.



Next i measure the transmit path:

foto setup transmit path

Therefore, the stimulus of the VNA is preamplified by two SPF5189Z in series by about 20 dB before injecting into the"Router" port of the booster, and the "Antenna" port connected to the VNA input via 40 dB attenuation (two 20 dB attenuators). 

At first i have an additional 3 dB attenuator between the two SPF5189Z as a measure of precaution. The spectrum is this:

foto

It appears that the input signal is at the switching threshold of the booster and the status LED flickers between red and green and the spectrum has a discontinuity.

After taking out the 3 dB attenuator i get this spectrum

foto

and the status LED stays green showing the booster remains in Tx mode. 

In the end, i measure approximately the input power of the booster by this setup

foto

it varies between 7 and 9 dBm, however, this is prone to error because the power meter also measures the fundamental mode. It has no means of frequency selection and the entered frequency is only a calibration factor. However, 9 dBm would corrresponds to something above the expected switching threshold of my modified booster. The nominal value is 3 .. 5 dBm and with the additional port in parallel it is expected to be some dB higher.

foto display

The Tx branch spectrum shows that the output level is as expected, peaking at - 4 dB, corresponding to + 20 dB of the two SPF5189Z, + 16 dB the booster, and - 40 dB of the output attenuators. If the input is assumed to be 9 dBm, we would have 25 dBm output power equaling 316 mW. This is in agreement with the observation that the heatsink of the first output attenuator does not warm up remarkably during some minutes of measurement. The spectrum clearly shows, in contrast to the widespread belief, that the Tx branch indeed does include filtering, although not comparable to the RX branch with its steep SAW filter, but good enough for harmonic suppression.


Conclusion:

The WLAN booster contains a surprisingly performant SAW filter in its receive branch. Filtering is also present in the transmit branch although less steep. The characteristics listed in the specs (manual card in the box) can be confirmed, except for the output power that is 5W instead of 8W, and the noise figure that we cannot measure. It is also emphasized that the design of such an amplifier involves difficulties beyond the scope of this review. The price performance ratio seems unbeatabe at a price between 40 and 50 euro. In agreement with other authors, i would not recommend the "always Tx" modification by shorting the pins 4 and 5 of the OpAmp because if used in SSB mode with an appropriate input level, the carrier detection/autoswitching reliably commutes to Tx fast enough and there should be no risk of commuting into Rx mode accidentally during the minima of the audio signal. The present board is at least the 3rd version that i'm aware of and probably there ae more. Some time ago, users claimed that they needed to inject 500 mW from the router to obtain the maximum output power of 5 W, but it seems that the later version have more Tx gain due to a suppressed attenuator.

