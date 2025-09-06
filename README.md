# 1Φ Full wave controlled rectifier
## Description
A 2-layer PCB for a full-wave controlled bridge rectifier to control the speed and direction of rotation of DC motors and loads of up to 3.5 A. The positive and negative half-cycles of the 220 V, 60 Hz supply can be independently controlled by varying the firing angle of the SCRs using two potentiometers.

## Block diagram
<img width="1999" height="875" alt="translated_image_en (3)" src="https://github.com/user-attachments/assets/068af9c5-dcf5-4faf-9c92-e34b611a9e2f" />

## Stages

### 5V Linear Regulator and Floating 5V Power Supplies

<img width="2108" height="394" alt="image" src="https://github.com/user-attachments/assets/92852eb5-7c5f-4ff6-a926-d006d1e8a66d" />

<img width="1369" height="1019" alt="image" src="https://github.com/user-attachments/assets/8dafbc29-4813-4f47-be4c-84f4cbbef76e" />


- LM7805
- 470 µF capacitors to reduce ripple
- Floating power supplies: B0505S

### Zero crossing detector

<img width="870" height="873" alt="image" src="https://github.com/user-attachments/assets/2d3982ec-494f-4258-95cf-394294b44a41" />

<img width="1465" height="517" alt="image" src="https://github.com/user-attachments/assets/ec7911cd-2ef0-457c-a296-103299760b70" />


- Detects positive and negative half-cycles:  
  - Negative half-cycle -> 3.3 V  
  - Positive half-cycle -> 0 V  
  - Zero crossings -> Edge change  

- ESP32 interrupts on edge change:  
  ```cpp
  attachInterrupt(digitalPinToInterrupt(zeroCrossingPin), ISR_pulse, CHANGE);
- Maximum current: Imax = 311/69k = 4.5 mA
- Power dissipated by 27k resistor: I^2*R = 0.55 W
- Reliability: 3 resistors in series, 1W each
- Imax optocoupler = 60 mA
- Maximum reverse voltage on optocoupler: 6 V -> Diode in parallel
- CTR (sat) = 20 %
- 8.2k resistor to ensure BJT saturation

### Potentiometers to control firing angle and triggering

<img width="1823" height="862" alt="image" src="https://github.com/user-attachments/assets/41eaf155-f8b8-466d-aa54-f3f7ada56c3c" />

<img width="424" height="1002" alt="image" src="https://github.com/user-attachments/assets/baa83777-918f-4068-b7a0-86c678348f6d" />

- In a conventional DCZ, it was not possible to know which half-cycle was positive or negative, so all 4 SCRs were triggered at the same time (while 2 of them were in reverse bias).  
- Unlike the conventional DCZ, the positive and negative half-cycles are detected, which allows for higher triggering efficiency.  
- Only 2 SCRs are triggered at the same time (when they are in forward bias).

<div style="display: flex; justify-content: center; gap: 20px;">

  <figure>
    <img src="https://github.com/user-attachments/assets/698dde33-620f-4cbb-8f0c-44bd86a81acd" alt="Image 1" width="300">
    <figcaption>Figure 1: First image caption</figcaption>
  </figure>


  <figure>
    <img src="https://github.com/user-attachments/assets/dc1cdfeb-fc80-48d8-b6a1-41a93b3a30a9" alt="Image 2" width="300">
    <figcaption>Figure 2: Second image caption</figcaption>
  </figure>

</div>



