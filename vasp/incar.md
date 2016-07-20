

|parameters|decription|
|:--|:--|
|NGX, NGY, NGZ|FFT mesh for orbitals (Sec. 6.3,6.11)|
|NGXF,NGYF,NGZF|FFT mesh for charges (Sec. 6.3,6.11)|
|NBANDS|number of bands included in the calculation| (Sec. 6.5)|
|NBLK|blocking for some BLAS calls (Sec. 6.6)|
|SYSTEM|name of System|
|NWRITE|verbosity write-flag (how much is written)|
|ISTART|startjob: 0-new 1-cont 2-samecut|
|ICHARG|charge: 1-file 2-atom 10-const|
|ISPIN|spin polarized calculation (2-yes 1-no)|
|MAGMOM|initial mag moment / atom|
|INIWAV|initial electr wf. : 0-lowe 1-rand|
|ENCUT|energy cutoff in eV|
|PREC|precession: medium, high or low|
|PREC|VASP.4.5 also: normal, accurate|
|NELM, NELMIN and NELMDL|nr. of electronic steps|
|EDIFF|stopping-criterion for electronic upd.|
|EDIFFG|stopping-criterion for ionic upd.|
|NSW|number of steps for ionic upd.|
|NBLOCK and KBLOCK|inner block; outer block|
|IBRION|ionic relaxation: 0-MD 1-quasi-New 2-CG|
|ISIF|calculate stress and what to relax|
|IWAVPR|prediction of wf.: 0-non 1-charg 2-wave 3-comb|
|ISYM|symmetry: 0-nonsym 1-usesym|
|SYMPREC|precession in symmetry routines|
|LCORR|Harris-correction to forces|
|POTIM|time-step for ion-motion (fs)|
|TEBEG, TEEND|temperature during run|
|SMASS|Nose mass-parameter (am)|
|NPACO and APACO|distance and nr. of slots for P.C.|
|POMASS|mass of ions in am|
|ZVAL|ionic valence|
|RWIGS|Wigner-Seitz radii|
|NELECT|total number of electrons|
|NUPDOWN|fix spin moment to specified value|
|EMIN, EMAX|energy-range for DOSCAR file|
|ISMEAR|part. occupancies: -5 Blöchl -4-tet -1-fermi 
|0-gaus >0 MP|
|SIGMA|broadening in eV -4-tet -1-fermi 0-gaus
|ALGO|algorithm: Normal (Davidson) | Fast | Very_Fast |(RMM-DIIS)
|IALGO|algorithm: use only 8 (CG) or 48 (RMM-DIIS)
LREAL|non-local projectors in real space
|ROPT|number of grid points for non-local proj in real space
|GGA|xc-type: e.g. PE AM or 91
|VOSKOWN|use Vosko, Wilk, Nusair interpolation
|DIPOL|center of cell for dipol
|AMIX, BMIX|tags for mixing
|WEIMIN, EBREAK, DEPER|special control tags
|TIME|special control tag
|LWAVE,LCHARG, LVTOT, LVHAR|create WAVECAR/CHGCAR/LOCPOT
|LELF|create ELFCAR
|LORBIT|create PROOUT
|NPAR|parallelization over bands
|LSCALAPACK|switch off scaLAPACK
|LSCALU|switch of LU decomposition
|LASYNC|overlap communcation with calculations
