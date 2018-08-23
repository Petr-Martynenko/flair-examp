# flair-examp
directory fluka\flair project
I would like to get the number of neutrons which are generated in the shielding detector ranging from
???cm to ???cm with a step of ???cm after irradiation by proton ions.
I created input file with cylindrical target filled with air to check
RCC цилиндрическая мишень

  You keep the outside medium of the shielded detector as vacuum. So no scattered components will be produced from this region.
  Then, in the detector wall and other (except sensitive volume of your detector), set

        transport cut-off for photon = slightly less than incident beam enegy (i.e for 1.25 MeV incident photon set as 1.24999 MeV like that).
   This is to avoid transportation of low energy scattered photons and electron.

  In the sensitive volume (scorring region set as 1 keV). Then you will get primary photon fluence.

I tried computing and using
USRBIN card to obtain the energy deposited in the detector give me the scored result such as xxxx_.xxxx ???
1) Energy deposited (energy deposited whole, GeV/cm3/primary)
USRBIN card to obtain the dose deposited in the detector with bin size 1mmx1mm, radial symmetry
2) Dose! I score the dose in GeV/g per primary and then transform it in Sv/h per primary.
3) the calculation of Dose-Equivalent (Sv per primary) using USRBIN and AUXSCORE 
The results from USRBIN are normalised per unit volume and per unit primary weight.
Fluence will be expressed in particles/GeV/cm2/sr/primary.
Flux will be expressed in (particles/cm^2/s) as a function of energy.

I try to calculate the differential yield in energy and polar angle in order to obtain the yield 

I use USRBDX card to score (in units of fluence or flow) the number of particles that cross that  area (per GeV and primary)
or the number of particles that cross the area per cm^-2 (per GeV and primary)?

To use a user routine and create your own executable the following commands from a terminal should work (FLAIR is an alternative):

$FLUPRO/flutil/fff source.f
Which will create an object file

Create a new executable with a name relevant for you

$FLUPRO/flutil/lfluka -m fluka -o YOURFLUKA source.o
???compile your code, i.e.  $FLUPRO/flutil/lfluka -m fluka -o ./flukahp source.f

Run FLUKA with the executable you created
(the path to the data file in your source routine should be coherent with the location from where you are running the code)
$FLUPRO/flutil/rfluka –e YOURFLUKA –M1 scint.par.inp

My second question arises when using the USRYIELD card:
USRBIN card always creates as a result files with .bnn extension.
Can You advise how to open and read this file to obtain numeric outcome of the simulation?
Or you can try to convert the *.bnn file to a ascii file in flair tab run->file, find the *.bnn file and right click and convert.
USRBDX gives the average fluence on a boundary surface (after dividing by the surface area, directly in FLUKA through the dedicated WHAT or at user
post-processing stage), while USRTRACK gives the average fluence in a region volume (after dividing by the volume as above).
