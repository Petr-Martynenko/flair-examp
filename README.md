# flair-examp
directory fluka\flair project
I would like to get the number of neutrons which are generated in the shielding detector ranging from
???cm to ???cm with a step of ???cm after irradiation by proton ions.
  You keep the outside medium of the detector as vacuum. So no scattered components will be produced from this region.
  Then, in the detector wall and other (except sensitive volume of your detector), set

        transport cut-off for photon = slightly less than incident beam enegy (i.e for 1.25 MeV incident photon set as 1.24999 MeV like that).
   This is to avoid transportation of low energy scattered photons and electron.

  In the sensitive volume (scorring region set as 1 keV). Then you will get primary photon fluence.

I tried computing and using
USRBIN card to obtain the energy deposited in the detector give me the scored result such as xxxx_.xxxx ???
1) Energy deposited (energy deposited whole, Gev/cm3)
2) Dose! I score the dose in GeV/g per primary and then transform it in Sv/h per primary.
3) the calculation of Dose-Equivalent (Sv per primary) using USRBIN and AUXSCORE 
The results from USRBIN are normalised per unit volume and per unit primary weight

I try to calculate the differential yield in energy and polar angle in order to obtain the yield 

I use USRBDX card to score (in units of fluence or flow) the number of particles that cross that  area (per GeV and primary)
or the number of particles that cross the area per cm^-2 (per GeV and primary)?

My second question arises when using the USRYIELD card:
