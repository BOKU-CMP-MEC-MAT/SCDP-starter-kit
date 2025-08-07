Brief instructions for the SCDPv2 model in Abaqus
=================================================

This is version 2 of the SCDP (SHOTLEON) model with certain improvement over version 1 from 2017.
It has been published in
```
Neuner, M., M. Schreter, and G. Hofstetter. “Comparative Investigation of Constitutive Models for Shotcrete Based on Numerical Simulations of Deep Tunnel Advance.” Numerical Methods in Geotechnical Engineering: 9th European Conference on Numerical Methods in Geotechnical Engineering, CRC Press Taylor & Francis Group, 2018, 103–8.
```
and applications are reported in
```
Neuner, M., M. Schreter, P. Gamnitzer, and G. Hofstetter. “On Discrepancies between Time-Dependent Nonlinear 3D and 2D Finite Element Simulations of Deep Tunnel Advance: A Numerical Study on the Brenner Base Tunnel.” Computers and Geotechnics 119 (March 2020): 103355. https://doi.org/10.1016/j.compgeo.2019.103355.
```
and in much more detail in
```
Neuner, Matthias, Alexander Dummer, Magdalena Schreter, Peter Gamnitzer, Günter Hofstetter, and Tobias Cordes. “From Experimental Modeling of Shotcrete to Numerical Simulations of Tunneling.” Advances in Applied Mechanics 54 (2021): 205–84. https://doi.org/10.1016/bs.aams.2020.12.003.
```
except for the shrinkage part. In the latter two publications, the Bazant-Panula shrinkage model was used instead of the ACI model,
which is considered in the present model.

Usage
-----
```
*Material, name=SHOTLEONV2
*Depvar
     106
```
The number of state vars depends on the number of Kelvin units. Usually the number of Kelvin units does not need to be changed.

Parameters (exactly 8 per line!!! not more, not less!)
------------------------------------------------------
```
*User Material, constants=27, unsymm
**E28,          E1,         flowMag,        flowExponent,       flowDelay,      nu,         tTransition,        tDelay
28400,          18000,      14,             1.,                 0.0,            0.2,        0.5,                0.3
**vResidual,    nKelvin,    kelvinMin,      strainShrInf,       TauShr,         fcu28,      fcu1,               ratioFcy
1e-2,           12,         1e-5,           -0.0000,            360,            40.85,      18.56,              0.1
**ratioFbu,     ratioFtu,   eCPP1,          eCPP8,              eCPP24,         Gfi         enableDamage,       dTStatic,   
1.16,           0.1,        -0.03,          -0.0007,            -0.0007,        0.1,         1,                 1e-3
**castTime,     timeToDays, enableNonlinearCreep
0,              1.0,        1.0
```

Explanation of the parameters:
------------------------------

- E28, E1: Young's modulus at the age of 28 and 1 days.
- flowMag: Parameter q4 of the B3 model (in 1e-6/MPa!) -- responsible for the long term creep. Calibrate according to the B3 model based on the concrete composition.
- flowExponent: Parameter ef in Eq. (15), keep 1 for original B3 model (Bazant&Baweja).
- flowDelay: Keep zero!
- nu: Poisson's ratio
- tTransition, tDelay, vResidual: Calibrate Eq. (10)
- kelvinMin, nKelvin: Smallest retardation time and number of Kelvin Units; You can keep those for most applications
- strainShrInf, TauShr: Shrinkage parameters according to the ACI model
- fcu28, fcu1: Uniaxial compressive strength at 28 and 1 days
- ratioFcy, ratioFbu, ratioFtu: ratios of fcy, fbu and ftu to fcu
- eCPP1, eCPP8, eCPP24: plastic strain at peak strenhth in uniaxial compression and 1, 8, and 24 hours. Calibrate based on expirements or use these values.
- Gfi: specific mode I fracture energy at 28 days
- enableDamage: Activate damage; usually 1
- dTStatic: Time for estimating the Young's modulus; Keep at 1e-3 days for most applications
- castTime: The time "zero" for casting of the concrete; usually 0.
- timeToDays: Factor for convertig Abaqus time to days, 1 if you are simulating in days.
- enableNonlinearCreep: enable nonlinear creep effects according to Eq. (15). 1=on, 0=off.

Meaning of state variables:
---------------------------

- 1: ignore
- 2: E(t)*
- 3: fcu(t)*
- 4: alpha_p*
- 5: increment of alpha_p
- 6: alpha_d*
- 7: increment of alpha_d
- 8: omega (damage)*
- remaining ones: ignore

(*) ... relevant for users
