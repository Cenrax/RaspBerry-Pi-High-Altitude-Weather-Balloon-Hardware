# Raspberry Pi Powered High Altitude Weather Balloon:

**The hardware of the weather balloon or the payload section is structured as a six layered stack of Raspberry Pi hats.**

---

<img align="right" width="500" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Pictures/InkedLRM_EXPORT_825743803905478_20190919_195242457_LI.jpg">

- [x] The first layer is a _**Raspberry Pi**_ which manages operation of all other stacks and undertakes necessary processing tasks.
- [x] To power the entire stack we have a high current DC-DC converter as a _**power delivery unit**_ which accepts 1S LiPo or LiIon batteries as input and outputs a steady 5V and allows a maximum current draw of 7A.
- [x] For telemetry and other communication purposes we are using a _**CC1125 based RF board**_.
- [x] The next stack comprises of a _**GSM board**_ for non LOS communication at lower altitudes and a _**GPS board**_ for tracking the balloon and for easy retrieval. Since both of these are using the same serial channel of the Raspberry Pi, multiplexing and demultiplexing were necessary. This was facilitated by a _**Mux Demux board**_ which had the ability to select one peripheral among the two.
- [ ] Finally on top of the GPS hat we have the _**sensor hat**_.

---

## 1. Power Delivery Board (PiPo):

To run the essential hardware stacks a power delivery system more capable than a Raspberry Pi's internal power distribution was required. A design of a power distribution stack which could supply a high enough current with stability was adopted. The board was powered with a 3.7V LiIon or LiPo cell, its output was a constant 5V with a maximum allowed current of 7A. Our required peak current was 5A so, a 2A headroom was kept. The design was built around the _**LTC1871-1**_ - a wide input range, current mode, boost, flyback or SEPIC controller that drives an N-channel power MOSFET and requires very few external components.

### 1.1. Schematic:

<p align="center">
  <img width="800" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Power_(Pi_Po)/Output/schematic.png">
</p>

[Link to PDF](https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Power_(Pi_Po)/Output/schematic.pdf)

### 1.2. Composite:

<p align="center">
  <img width="800" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Power_(Pi_Po)/Output/composite.png">
</p>

### 1.3. Assembly:

<img align="left" width="300" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Power_(Pi_Po)/Output/assembly.png">

**Comments on Revision 1.0 -** Assistance regarding _PCB layout, component placing & the schematic design_ was taken from the corresponding datasheets. Output voltage was as expected, as well as the ability to supply high currents. However, on probing the output with and oscilloscope it was found that the output waveform was quite noisy and had significant switching noise at the frequency of the used DC-DC converter. 

<br>

It was concluded that PCB layout and component placement were the culprits for induction of switching noise _(To be addressed in rev 2.0)_. Hence, I would not recommend following the component placement and PCB layout guides present in _**LTC1871-1's**_ datasheet. Instead, Understand the guidelines given and apply common mutual induction principles while designing the output section. 

<br>

<br>

---

## 2. Transceiver Board (LinkBerry):

For telemetry, real time location update and live image/video feed a point to point long distance RF link was designed and fabricated which is based on _Texas Instrument's_ _**CC1125**_ - a single chip, fully integrated narrow band RF transceiver. Coupled with a front end module (FEM) - _Qorvo's **RFFM6403**_, the board has a maximum output power level of **30dBm**. The FEM has inbuilt selectable power amplifier (PA) for transmission and a low noise amplifier (LNA) for reception. This communication stack acts as a bridge between the Pi's SPI bus and the unguided RF encoded & modulated data. In other words this encodes, modulates & transmits data received over the SPI bus (from the Pi). Also it decodes data received over its RF link and sends it through the SPI bus.
This is a 4 layer board since this includes mixed signal components, hence, strict PCB layout requirements. Also a thinner dielectric (prepreg) between the 50 Ohm RF traces and the GND plane allows for narrower RF microstrip lines for a given impedance. So, the layer stackup is important to keep in mind while designing.

### 2.1. Schematic:

[Link to PDF](https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Transceiver_(LinkBerry)/Output/Radio.pdf)
_because component labels are too small for an image here._

`CC1125's datasheet followed for 50 Ohm matching`

### 2.2. Layer Stackup: <img align="right" width="500" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Transceiver_(LinkBerry)/Output/layer%20stack.png">

- Top layer - separated digital and analog signals, most important signals are routed on this layer.
- Internal plane 1 - GND plane.
- Internal plane 2 - Power plane includes 3.3V, 3.6V & 5V polygons.
- Bottom layer - less important digital signals, traces which did not fit on the top layer, bridges and a GND polygon.

<br>

### 2.3. PCB layers:

<br>

<img align="left" width="250" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Transceiver_(LinkBerry)/Output/top.jpg">

<img align="right" width="250" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Transceiver_(LinkBerry)/Output/IP1.jpg">

<p align="center">
  <img width="250" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Transceiver_(LinkBerry)/Output/IP2.jpg">
</p>

<img align="right" width="250" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Transceiver_(LinkBerry)/Output/silk.jpg">

<img align="left" width="250" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Transceiver_(LinkBerry)/Output/bottom.jpg">

<p align="center">
  <img width="250" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/Transceiver_(LinkBerry)/Output/mask.jpg">
</p>

<br>

<br>

### 2.4. Images:

<br>

<img align="left" width="250" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Pictures/rf3.jpeg">

<img align="right" width="250" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Pictures/rf6.jpeg">

<p align="center">
  <img width="250" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Pictures/paste_closeup.jpeg">
</p>
<br>

<br>

<br>

---

## 3. Multiplexer Board (PiMux):

This is a rather minimal 2:1 MUX board built around _**ISL54220**_ - a single supply dual 2:1 multiplexer that can operate from a single 2.7V to 5.5V supply. This multiplexes the two serial peripherals - GSM & GPS boards and enables them to be used one at a time on the single serial bus available on the Raspberry Pi. Data rates aren't a bottle neck as this IC was designed for USB purposes (480 Mbps).

### 3.1. Functional Diagram:

<p align="center">
  <img width="700" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/MuxDemux_(Pi_Mux)/Output/Application.jpg">
</p>

<br>

### 3.2. Truth Table: 

<p align="center">
  <img width="500" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/MuxDemux_(Pi_Mux)/Output/Truth%20Table.png">
</p>

<br>

### 3.3. Schematic:

<p align="center">
  <img width="700" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/MuxDemux_(Pi_Mux)/Output/schematic_image.png">
</p>

[Link to PDF](https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/MuxDemux_(Pi_Mux)/Output/schematic.pdf)

---

## 4. GSM board:

We used an available Raspberry Pi GSM hat made _**SmartElex**_. The primary purpose of GSM in this project would be to keep the communication link intact even if LOS communication is not possible. For example - maintaining comm link very near to ground where LOS might not be available. Also, for retrieval purposes the GPS and GSM board works hand in hand to provide the exact location where it landed. The GSM board is based on _**Quectel's M66**_ SMD module. So, relevant AT commands will be functional.

<p align="center">
  <img width="700" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/GPS%2BGSM_Module/GSM/Component_labelled.png">
</p>

## 5. GPS board:  

Both GSM as well as the GPS boards used were purchased as a designed and fabricated board. This was because, in this particular application we did not require some special characteristic like we did in the case of the trancseiver board or the power distribution board. This GPS board is made by _**"Keyestudio"**_. Heart of the board is a _**"Ublox Neo-6M"**_ SMD GPS module.
Buying a ready made board instead of designing boards which already exist seemed practical and less costly.

<p align="center">
  <img width="600" src="https://github.com/MonkHelios/RaspBerry-Pi-High-Altitude-Weather-Balloon-Hardware/blob/master/Payload/Hardware/GPS%2BGSM_Module/GPS/Board.png">
</p>

---

## 6. Sensor board (PiSense):

_Details about PiSense will be updated once design and fabrication are complete._
