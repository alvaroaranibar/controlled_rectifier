# 1Φ Full wave controlled rectifier
## Description
A 2-layer PCB for a full-wave controlled bridge rectifier to control the speed and direction of rotation of DC motors and loads of up to 3.5 A. The positive and negative half-cycles of the 220 V, 60 Hz supply can be independently controlled by varying the firing angle of the SCRs using two potentiometers.

## Block diagram
<img width="1999" height="875" alt="translated_image_en (3)" src="https://github.com/user-attachments/assets/068af9c5-dcf5-4faf-9c92-e34b611a9e2f" />

## Stages

### 5V Linear Regulator and Floating 5V Power Supplies

<img src="https://github.com/user-attachments/assets/92852eb5-7c5f-4ff6-a926-d006d1e8a66d" alt="image" width="400" />

<img src="https://github.com/user-attachments/assets/8dafbc29-4813-4f47-be4c-84f4cbbef76e" alt="image" width="400" />

- LM7805
- 470 µF capacitors to reduce ripple
- Floating power supplies: B0505S

### Zero crossing detector

<img src="https://github.com/user-attachments/assets/2d3982ec-494f-4258-95cf-394294b44a41" alt="image" width="400" />

<img src="https://github.com/user-attachments/assets/ec7911cd-2ef0-457c-a296-103299760b70" alt="image" width="400" />

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

<img src="https://github.com/user-attachments/assets/41eaf155-f8b8-466d-aa54-f3f7ada56c3c" alt="image" width="400" />

<img src="https://github.com/user-attachments/assets/baa83777-918f-4068-b7a0-86c678348f6d" alt="image" width="300" />

- In a conventional DCZ, it was not possible to know which half-cycle was positive or negative, so all 4 SCRs were triggered at the same time (while 2 of them were in reverse bias).  
- Unlike the conventional DCZ, the positive and negative half-cycles are detected, which allows for higher triggering efficiency.  
- Only 2 SCRs are triggered at the same time (when they are in forward bias).

<div style="display: flex; justify-content: center; gap: 20px;">

| <img src="https://github.com/user-attachments/assets/698dde33-620f-4cbb-8f0c-44bd86a81acd" width="300"> | <img src="https://github.com/user-attachments/assets/dc1cdfeb-fc80-48d8-b6a1-41a93b3a30a9" width="300"> |
|:-------------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------:|
| Positive lobes                                                                           | Negative lobes                                                                       |

<img width="1575" height="264" alt="image" src="https://github.com/user-attachments/assets/b78cb651-6291-40b1-9490-0015a8e5deb4" />

<img width="894" height="1016" alt="image" src="https://github.com/user-attachments/assets/910cdc76-9b40-4073-a14c-b6bcd007a6b5" />

</div>



