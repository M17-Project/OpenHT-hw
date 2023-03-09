# Design and dimensioning of the power supply stage

## Electrical requirements

* Three output rails:
    * 3.3 V
    * 1.8 V
    * 1.0 V

* FPGA requirements
    * Manufacturer recommends a regulation accurate within 3% of the optimum voltage for the DC power supplies
        * 3.3 V -> [3.2 V - 3.4 V], or deviation of maximum 99 mV
        * 1.8 V -> [1.746 V - 1.854 V], or deviation of maximum 54 mV
        * 1.0 V -> [0.97 V - 1.03 V], or deviation of maximum 30 mV
    * An additional recommendations advices to target less than 0.25% peak noise for the PLLs supplies. In this project, only VCCPLL_ULC is connected and supplied with 1.0 V. This implies a maximum voltage ripple on the 1.0 V rail of 2.5 mV.

    Thus the 1.0 V must be particularly noise-free.

## Choice of components

It was decided to pick a buck converter to keep a good power efficiency. The triple-output buck TPS65261 from Texas Instrument was picked because it fills the constraint of being low quiescent current, input voltage range comprising 6 V-8.4 V (for a 2-cells lithium battery) and good light-load efficiency. However, at light load, the output voltage ripple can become quite important (about 20 mV). This could cause problems for the 1.0 V of the FPGA which requires an input voltage with an error margin of +-3%. Thus the 1.0 V rail will be derived from the 1.8 V rail using a high PSRR LDO.

## Buck-converter dimensioning

### Common

 * Switching frequency: 400kHz -> Rosc = 110 kR
 * Soft start: ~1 ms -> Css = 18 nF
 * UVLO: when Vbat reaches 6 V (6.0 V falling, 6.2 V rising)
    * R1 = 200 kR
    * R2 = 47.58 kR -> 47.5kR
    * Reverse calculation gives 6.209 V rising, 6.009 V falling

### Buck 1

 * Output: 3.3 V -> R1 = 9.53 kR, R2 = 43 kR (0.1% tolerance)

    |        | Min.    | Nom.    | Max.    |
    |--------|---------|---------|---------|
    | V      | 3.269 V | 3.307 V | 3.346 V |
    | %      | -0.95   | 0.22    | 1.39    |
    | Margin | 68 mV   |         | 53 mV   |

    N.B. The tolerance takes into account the tolerance of the feedback voltage reference (1%).

 * Output current: 500 mA (max. for MCU is 290 mA, max for Xcver is 80 mA. Keep room for the rest of the system (audio, FPGA, ...))
 Switching 

 * Inductor choice 
    * Value: 33 uH (LIR = 0.3)
        * Iripple = 150 mA
        * ILrms = 501 mA
        * ILpeak = 575 mA
        * ILpeak(PSM mode) = 230 mA
    The reference NRS5030T330MMGJ is pre-picked. Max current is 800 mA, saturation current is > 1 A, series resistance is 300 mOhm.
* Output capacitor
    * Based on modulator pole: Does not really influence loop compensation calculation.
    * Based on Vout ripple: Voripple of 2% of 3.3 V -> 0.7uF, max Resr of 0.45 Ohm.
    * Based on load regulation: Max drop in Vout of 2% for an increase in current of 100 mA (i.e. a bit more than the Xcvr transmitting) -> 7.5 uF

    The minimum output capacitor value should therefore be 7.5 uF. The Resr should be below 0.45 Ohm. Several capacitors should be placed in parallel to help satisfy this requirement.
    Use 3 ceramic capacitors of 4.7 uF with X5R or better dielectric.

* Input capacitor
    * Using the same capacitor network as the output capacitor would lead to 22 mV of input ripple. 
    * It is recommended to use a large enough footprint so that this value can be increased later on if needed.

* Loop compensation
    * Cross over frequency fc = 1/10 of fsw or 40 kHz
    * Rc = 8713 Ohm -> rounded to 8660 Ohm
    * fp = 37913 Hz, the pole created by Cc must be lower than that. -> Cc = 1.14 nF, rounded up to 1.2 nF
    * Cb = 3.7 pF, rounded up to 3.9 pF (for a Resr of 1 mOhm)


 ### Buck 2

 * Output: 1.8V -> R1 = 10 kR, R2 = 20 kR (0.1% tolerance)

    |        | Min.    | Nom.  | Max.   |
    |--------|---------|-------|--------|
    | V      | 1.779 V | 1.8 V | 1.82 V |
    | %      | -1.13   | 0     | 1.13   |
    | Margin | 33 mV   |       | 34 mV  |

    N.B. The tolerance takes into account the tolerance of the feedback voltage reference (1%).
 * Output current: 250 mA. Picking a lower current would not make much sense as the devices goes into PSM when below 230 mA.

 * Inductor choice
    * Value: 47 uH (LIR = 0.3)
        * Iripple = 75 mA
        * ILrms = 251 mA
        * ILpeak = 288 mA
        * ILpeak (PSM) = 230mA

    The reference NRS5030T470MMGJ is pre-picked. Max current is 700 mA, saturation current is 0.95 A, series resistance is 325 mOhm.

* Output capacitor   
    * Based on modulator pole: Does not really influence loop compensation calculation.
    * Based on Vout ripple: Voripple of max 25mV -> 0.94uF, max Resr of 0.33 Ohm.
    * Based on load regulation: Max drop in Vout of 25mV for an increase in current of 30 mA -> 6uF

    The minimum output capacitor value should therefore be 6 uF. The Resr should be below 0.33 Ohm. Several capacitors should be placed in parallel to help satisfy this requirement.
    Use 2 ceramic capacitors of 4.7 uF with X5R or better dielectric.

* Input capacitor
    * Using the same capacitor network as the output capacitor would lead to 16 mV of input ripple. 
    * It is recommended to use a large enough footprint so that this value can be increased later on if needed.

* Loop compensation
    * Cross over frequency fc = 1/10 of fsw or 40 kHz
    * Rc = 3191 Ohm -> rounded to 3160 Ohm
    * fp = 37913 Hz, the pole created by Cc must be lower than that. -> Cc = 0.96 nF, rounded up to 1 nF
    * Cb = 4.5 pF (for a Resr of 1.5mOhm)

## 1.0 V LDO

* Divider bridge
    Output = 1.0V, feedback = 0.8V at 1% accuracy

    |        | Min.    | Nom. | Max.    |
    |--------|---------|------|---------|
    | V      | 0.989 V | 1    | 1.01 V  |
    | %      | -1.04   | 0    | 1.04    |
    | Margin | 19.6 mV |      | 19.6 mv |

-- Morgan ON4MOD