# Design and dimensioning of the power supply stage

## Electrical requirements

* Three output rails:
    * 3.3V
    * 1.8V
    * 1.0V

* FPGA requirements
    * Manufacturer recommends a regulation accurate within 3% of the optimum voltage for the DC power supplies
        * 3.3V -> [3.2V - 3.4V], or deviation of maximum 99mV
        * 1.8V -> [1.746V - 1.854V], or deviation of maximum 54mV
        * 1.0V -> [0.97V - 1.03V], or deviation of maximum 30mV
    * An additional recommendations advices to target less than 0.25% peak noise for the PLLs supplies. In this project, only VCCPLL_ULC is connected and supplied with 1.0V. This implies a maximum voltage ripple on the 1.0V rail of 2.5mV.

    Thus the 1.0V must be particularly noise-free.

## Choice of components

It was decided to pick a buck converter to keep a good power efficiency. The triple-output buck TPS65261 from Texas Instrument was picked because it fills the constraint of being low quiescent current, input voltage range comprising 6V-8.4V (for a 2-cells lithium battery) and good light-load efficiency. However, at light load, the output voltage ripple can become quite important (about 20mV). This could cause problems for the 1.0V of the FPGA which requires an input voltage with an error margin of +-3%. Thus the 1.0V rail will be derived from the 1.8V rail using a high PSRR LDO.

## Buck-converter dimensioning

### Common

 * Switching frequency: 400kHz -> Rosc = 110kR
 * Soft start: ~1ms -> Css = 18nF

### Buck 1

 * Output: 3.3V -> R1 = 9.53 kR, R2 = 43 kR (0.1% tolerance)

    |   | Min.   | Nom.  | Max.  |
    |---|--------|-------|-------|
    | V | 3.269V | 3.307 | 3.346 |
    | % | -0.95  | 0.22  | 1.39  |

    N.B. The tolerance takes into account the tolerance of the feedback voltage reference (1%).

 * Output current: 500mA (max. for MCU is 290mA, max for Xcver is 80mA. Keep room for the rest of the system (audio, FPGA, ...))
 Switching 

 * Inductor choice 
    * Value: 33uH (LIR = 0.3)
        * Iripple = 150mA
        * ILrms = 501mA
        * ILpeak = 575mA
        * ILpeak(PSM mode) = 230mA
* Output capacitor
    * Based on modulator pole: Does not really influence loop compensation calculation.
    * Based on Vout ripple: Voripple of 2% of 3.3V -> 0.7uF, max Resr of 0.45 Ohm.
    * Based on load regulation: Max drop in Vout of 2% for an increase in current of 100mA (i.e. a bit more than the Xcvr transmitting) -> 7.5uF

    The minimum output capacitor value should therefore be 7.5uF. The Resr should be below 0.45 Ohm. Several capacitors should be placed in parallel to satisfy this requirement.

    \[WIP\]


 ### Buck 2

 * Output: 1.8V -> R1 = 10 kR, R2 = 20 kR (0.1% tolerance)

    |   | Min.   | Nom.  | Max.  |
    |---|--------|-------|-------|
    | V | 1.779V | 1.8   | 1.82  |
    | % | -1.13  | 0     | 1.13  |

    N.B. The tolerance takes into account the tolerance of the feedback voltage reference (1%).



-- Morgan ON4MOD