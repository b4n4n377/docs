---
title: JohnH Simple Attenuator 
parent: Hardware-Stuff
has_children: false
nav_order: 1
---
# JohnH Simple Attenuator Build

## Project abstract
At the end of 2024, I purchased a **Marshall SV20H**, the modern reissue of the legendary **Plexi tube amplifier**. 

The classic tone of this amplifier is achieved when the **power tubes are pushed into saturation**, causing them to overdrive. However, this requires **high volume** levels.

The overdrive is achieved by turning up the preamp, which generates a highly amplified signal that is passed on to the power amp, driving it into saturation. Originally, these amplifiers did not include a master volume, as they were often used to directly amplify sound for the audience, since PA systems were not available. Master volumes were introduced later, placed in the signal chain between the preamp and power amp. This allows control of the signal's volume after preamp amplification, without necessarily pushing the power amp into saturation.

To enjoy this classic circuit at moderate volumes, I first explored the concept of **attenuators**. This device is placed between the amplifier’s output and the speaker, reducing the amplifier’s output power. Attenuators work by converting excess power into heat, allowing the amplifier to operate as if it were driving a full load. There are two main types of attenuators: resistive and reactive.

- **Resistive attenuators** provide consistent impedance and are simple in design, but they can affect the amp's tone by not fully replicating the dynamic interaction between the amp and speaker.

- **Reactive attenuators** simulate the impedance curve of a speaker, preserving more of the amp's natural tone and feel, but they are often more complex and expensive.

At this point, I would like to document how I came across the [**reactive attenuator design from JohnH on the Marshall Forum**](https://marshallforum.com/threads/simple-attenuators-design-and-testing.98285/). All needed information can be found in the forum thread. Here, I aim to condense the now as of December 2024 >220 thread pages for my own reference, preparing and documenting the parts ordering and assembly process.

## Design overview
A summarized overview of the design, which is shown in the following graphic, can be found directly on the first page of the Marshall Forum thread.

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/b4n4n377/docs/main/img/M2_221024.gif" alt="Graphic showing the attenuator design" style="border: 2px solid white; padding: 5px; max-width: 100%; height: auto;">
    <p><em>[Source: <a href="https://marshallforum.com/threads/simple-attenuators-design-and-testing.98285/" target="_blank">First page Marshall Forum Thread</a>]</em></p>
</div>

## Parts List

### Stage 1
| Part | Description | Source Webshop |     
| --- | --- | --- |     
| L1 | Inductor 0.9 mH, 18 awg | --- |
| R1 | Resistor 15 Ω, 100 Watt | --- |
| R2A | Resistor 22 Ω, 50 Watt | --- |
| R2B | Resistor 18 Ω, 50 Watt | --- |

### Stage 2
| Part | Description | Source Webshop |     
| --- | --- | --- |  
| R5 | Resistor 4.7 Ω, 25 Watt | --- |
| R6 | Resistor 15 Ω, 25 Watt | --- |

### Stage 3
| Part | Description | Source Webshop |     
| --- | --- | --- |
| R3 | Resistor 15 Ω, 25 Watt | --- |
| R4 | Resistor 10 Ω, 25 Watt | --- |

### Stage 4 
| Part | Description | Source Webshop |     
| --- | --- | --- |
| R7 | Resistor 33 Ω, 25 Watt | --- |
| R8 | Resistor 5.6 Ω, 25 Watt | --- |

### General Parts
| Part | Description | Source Webshop |     
| --- | --- | --- |
| Case | Hammond | --- |
| R9 | Resistor 22 Ω, 25 Watt | --- |
| R10 | Resistor 68 Ω, 25 Watt | --- |
| R11 | Resistor 10 Ω, 25 Watt | --- |
| R12 | Resistor 5000 Ω, 25 Milliwatt | --- |
| R13 | Resistor 5000 Ω, 2 Watt | --- |
| Amp Input | Jack | --- |
| Speaker Output 1 | Jack | --- |
| Speaker Output 3 | Jack | --- |
| Speaker Output 3 | Jack | --- |
| Speaker Output 3 | Jack | --- |
| Line Output | Jack | --- |



