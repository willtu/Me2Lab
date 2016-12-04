1. Bi1_Cs1_S2--GGA_optimize_structure_(2x)--6

> INCAR.orig = INCAR.relax1 = INCAR.relax2

```
ALGO = Fast
# algorithm: Normal (Davidson) | Fast | Very_Fast (RMM-DIIS)
EDIFF = 0.0002
# stopping-criterion for electronic upd.
ENCUT = 520
IBRION = 2
ICHARG = 1
ISIF = 3
ISMEAR = -5
ISPIN = 2
LORBIT = 11
LREAL = Auto
LWAVE = False
MAGMOM = 4*0.6
NELM = 100
NPAR = 2
NSW = 99
PREC = Accurate
SIGMA = 0.05
```

2. Bi1_Cs1_S2--GGA_static_v2--1835

> INCAR = INCAR.orig

```
ALGO = Normal
EDIFF = 4e-06
ENCUT = 520.0
IBRION = -1
ICHARG = 0
ISIF = 3
ISMEAR = -5
ISPIN = 2
ISTART = 1
LAECHG = True
LCHARG = True
LORBIT = 11
LREAL = Auto
LVHAR = True
LWAVE = False
MAGMOM = 4*0.6
NELM = 100
NPAR = 2
NSW = 0
PREC = accurate
SIGMA = 0.05
``
3. Bi1_Cs1_S2--GGA_Uniform_v2--1837

```
ALGO = Normal
EDIFF = 4e-06
ENCUT = 520.0
IBRION = -1
ICHARG = 11
ISIF = 3
ISMEAR = 0
ISPIN = 1
ISTART = 1
ISYM = 0
LAECHG = True
LCHARG = False
LORBIT = 11
LREAL = Auto
LVHAR = True
LWAVE = False
NBANDS = 22
NELM = 100
NPAR = 2
NSW = 0
PREC = accurate
SIGMA = 0.001
```
