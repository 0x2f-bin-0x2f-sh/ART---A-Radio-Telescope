# ART - A Radio Telescope
Meet Art, he is A (simple) Radio Telescope. Art will be able to look into the galaxy and observe large hydrogen clouds undergoing quantum effects emitting radiation at a wavelength of approximatley 21 cm. Art will also measure the relative velocity of various parts of the Milky Way compared to the velocity of the Earth.

# Art is a work in progress, here is my research log!

**Note:** Art is built on an ARM architecture Linux kernel, if you would like to use any of my work, you may need to run it on an ARM kernel, however, the code work should run on any architecture. The biggest problem I have come across is in the installation of librtlsdr and pyrtlsdr on an ARM processor. I ended up doing this in a ARM Linux VM running within an M1 MacBook Air.

#### OBJECTIVE

Creating a homemade radio telescope using a parabolic dish to detect electromagnetic emission from hydrogen atoms undergoing a quantum spin-flip. This photon will be detectable at a frequency of 1.420e9 Hz (Helmut Hellwig et al. 1970).

#### REQUIREMENTS:

1) Parabolic reflector
2) RTL-SDR (software defined radio) USB attachment for computer
3) Coaxial cable (SMA)
4) Low Noise Amplifier to detect the hydrogen line

#### CONFOUNDING VARIABLES:

1)	There is likely to be other forms of electromagnetic emission from Earth based sources operating around the 1.420e9 Hz frequency. This could cause spikes or interference with the results, so strong calibration and testing is required in built up areas, and remote areas to establish a baseline.
2)	Doppler shift – due to the size of the galaxy, there is likely to be some doppler shift recorded due to the fact the galaxy is moving – at this time I do not know how significant the effect of the doppler shift will be on my results, but this should be obvious with small (and linear?) changes in the frequency around the line of the hydrogen emission.

#### PRE-SETUP:
1)	To remove the problem of the device being in use, enter the following command to a linux terminal: echo 'blacklist dvb_usb_rtl28xxu' | sudo tee – append /etc/modprobe.d/blacklist-dvb_usb_rtl28xxu.conf

#### METHOD:

N.b. work in progress.. It is my intention to produce an engineered moving platform for Art so that the telescope may remain fixed on a certain region of space, or to map a sector of the milky way outside of the natural rotation of the Earth.

1)	Point telescope at the zenith to confirm accuracy, and let the natural rotation of the Earth act as the rotational 'arm' of Art.

#### REMOVAL OF ERRORS / CALIBRATION:
1) To do 

#### IDENTIFYING OTHER RF INTERFERENCE:
1) Test in rural area
2) Test in urban area
3) Test in home
4) Test with phone on and in use by telescope
5) Test with phone off / airplane mode


#### UPDATE 1 – 16/05/2022
I have now successfully run my script and tested it across various frequencies of known UK radio stations to check it is working as intended. This is without the LNA connected. Results as below:

<img width="742" alt="image" src="https://user-images.githubusercontent.com/49762827/169140533-6d98b437-35eb-494a-9c9e-95cc47e7b50c.png">

<img width="694" alt="image" src="https://user-images.githubusercontent.com/49762827/169140808-4eb6261a-5660-4031-a0d5-6076f598fa59.png">

The spikes in the data indicate where a radio station is transmitting, hence a higher power density.

When viewing the hydrogen line, it produces this graph (important re update 2)

<img width="427" alt="image" src="https://user-images.githubusercontent.com/49762827/169140873-6d11a38b-4f35-4f3c-9d3b-88a28a12ec38.png">


#### UPDATE 2 – 17/05/2022
The correct size male to male connector has arrived for the LNA, so I have added this to my overall circuit, between the dish and the RTL-SDR dongle. However, this is presenting a problem. When I configure my script to examine the hydrogen line, the results are as follows:

<img width="427" alt="image" src="https://user-images.githubusercontent.com/49762827/169140932-5df3503d-7ad0-4df6-968f-f6a79a100260.png">

Clearly, something is wrong. There is a similar shape to the graph produced with no LNA filter in place, however there is one notable difference in that the graph has less spikes, it is smoother. Why this is, I do not currently know.

I have attempted to connect USB power to the LNA filter. When doing this with it not plugged into the RTL-SDR dongle, a white LED illuminates. However, as soon as it is plugged into my laptop via the RTL-SDR, the white light disappears, which may be an indication the device does not like two sources of power, so defaults to the power output of my laptop. I have also experimented with plugging the dongle (connected to the dish via the LNA) into a USB port and then plugging the USB power cable in – this produces a single ‘flash’ of the LED which is active for less than a second before turning off.

The solution to this problem was to turn on the bias tee via: ./rtl_biast -b 1 (incorporated into my script now). This has produced the following graph, the next step will be to point my telescope at the sun and check there is a peak in the graph at the expected point.

<img width="550" alt="image" src="https://user-images.githubusercontent.com/49762827/169141052-d3fb0a59-8220-4bb9-9a47-19f129cd4a61.png">
 
From GQRX to compare results:

<img width="728" alt="image" src="https://user-images.githubusercontent.com/49762827/169141110-a27029a9-96b4-4112-aed5-f5de9e2155a9.png">

Analysing the two results, this is correct. The large peak on the left hand side up to approximately -52 dB Hz<sup>-1</sup> coincides with the peak at -70 dB Hz<sup>-1</sup> (frequency is approximately 1 419 900 kHz).
 
 
#### REFERENCES

HELMUT HELLWIG, ROBERT F. C. VESSOT, MARTIN W. LEVINE, PAUL W. ZITZEWITZ, DAVID W. ALLAN, AND DAVID J. GLAZE (1970) ‘Measurement of the Unperturbed Hydrogen Hyperfine Transition Frequency’. Available at: ‘https://tf.nist.gov/general/pdf/13.pdf’ (page 1 [200]).
