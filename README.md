# 1Φ Full wave controlled rectifier
## Description
A 2-layer PCB for a full-wave controlled bridge rectifier to control the speed and direction of rotation of DC motors and loads of up to 3.5 A. The positive and negative half-cycles of the 220 V, 60 Hz supply can be independently controlled by varying the firing angle of the SCRs using two potentiometers.

## Block diagram
<img width="1999" height="875" alt="translated_image_en (3)" src="https://github.com/user-attachments/assets/068af9c5-dcf5-4faf-9c92-e34b611a9e2f" />

## Stages

### 5V Linear Regulator and Floating 5V Power Supplies
- LM7805
- 470 µF capacitors to reduce ripple
- Floating power supplies: B0505S

### Zero crossing detector
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
- Maximum reverse voltage on optocoupler:
- 6 V -> Diode in parallel
- CTR (sat) = 20 %
- 8.2k resistor to ensure BJT saturation

