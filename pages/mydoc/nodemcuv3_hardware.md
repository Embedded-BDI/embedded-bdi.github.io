---
title: Hardware
sidebar: mydoc_sidebar
permalink: nodemcuv3_hardware.html
folder: mydoc
datatable: true
---

<br>

Based on the versions described in the [Vacuum Cleaner Application](./nodemcuv3_vacuum.html) page, the agent requires multiple inputs and outputs:

* Inputs: `dirty`, `button_start`, and `full_deposit`.
* Outputs: `pos_1`, `pos_2`, `pos_3`, `pos_3`, `suck`, and `empty` (empty dust deposit).

Although the NodeMCUv3 board has 16 GPIOs pins available, only a small portion of these can be used for standard digital input/output. Many GPIO pins are used for dedicated functions/signals, such as RX, TX, SPI_CLK, and others. For this reason, 5 pins are used for digital output of the `pos_1`, `pos_2`, `pos_3`, `pos_4`, and `suck` & `empty` beliefs and the analog input is used for the `dirty`, `button_start`, and `full_deposit` inputs.

The diagram below shows the circuit connected to the NodeMCUv3:

<center>{% include image.html file="nodeMCUv3_circuit.png" caption="Circuit Diagram" %}</center>

Where:

* **SW1**: `button_start`
* **SW2**: `dirty`
* **SW3**: `full_deposit`
* **LED1**: `pos_1`
* **LED2**: `pos_2`
* **LED3**: `pos_3`
* **LED4**: `pos_4`
* **LED5**: `suck` & `empty`

The LEDs 1~4 will emit light according to the location of the vacuum cleaner, while LED5 will blink when the vacuum is sucking or emptying the dust deposit.

When closed, the switches set the value of the corresponding belief to `true`. Similarly, the value is set to `false` if the switch is open. The following table displays the values read by the analog input for different combinations of switch states:

<table style="width:100%" align="center">
  <tr>
    <td align="center"><b>SW1</b> (<code>button_start</code>)</td>
    <td align="center"><b>SW2</b> (<code>dirty</code>)</td> 
    <td align="center"><b>SW3</b> (<code>full_deposit</code>)</td>
    <td align="center"><b>Value</b></td>
  </tr>
  <tr>
    <td align="center">0</td>
    <td align="center">0</td>
    <td align="center">0</td>
    <td align="center">10</td>
  </tr>
  <tr>
    <td align="center">0</td>
    <td align="center">0</td>
    <td align="center">1</td>
    <td align="center">116</td>
  </tr>
  <tr>
    <td align="center">0</td>
    <td align="center">1</td>
    <td align="center">0</td>
    <td align="center">368</td>
  </tr>
  <tr>
    <td align="center">0</td>
    <td align="center">1</td>
    <td align="center">1</td>
    <td align="center">423</td>
  </tr>
  <tr>
    <td align="center">1</td>
    <td align="center">0</td>
    <td align="center">0</td>
    <td align="center">589</td>
  </tr>
  <tr>
    <td align="center">1</td>
    <td align="center">0</td>
    <td align="center">1</td>
    <td align="center">615</td>
  </tr>
  <tr>
    <td align="center">1</td>
    <td align="center">1</td>
    <td align="center">0</td>
    <td align="center">693</td>
  </tr>
  <tr>
    <td align="center">1</td>
    <td align="center">1</td>
    <td align="center">1</td>
    <td align="center">715</td>
  </tr>
</table>

<!-- The following picture shows the circuit set up on an actual board.

<center>{% include image.html file="" caption="Circuit in Real Board" %}</center>

_____

The following pictures show the state of the board in specific scenarios:

* pos1
* pos2
* pos3
* pos4
* when cleaning a space, pos2 for example
* when detecting that the dust deposit is full and must be emptied -->