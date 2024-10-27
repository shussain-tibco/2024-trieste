# 1. Getting started: solutions <!-- omit in toc -->

- [1.2. Create a new egs++ application](#12-create-a-new-egs-application)
- [1.3. Add a dose scoring object](#13-add-a-dose-scoring-object)
- [1.4. Change the type of incident particles](#14-change-the-type-of-incident-particles)
- [1.5. Change the energy of incident particles](#15-change-the-energy-of-incident-particles)
- [1.6. Change the material of the plate](#16-change-the-material-of-the-plate)
- [1.7. Monitor a simulation in detail](#17-monitor-a-simulation-in-detail)

<!-- ## 1.1. -->

## 1.2. Create a new egs++ application

### Does the `myapp` application provide any useful information when it runs?

> Not really. By default when an EGSnrc application runs it echoes to the
> terminal some information about the simulation configuration, parameters, data
> initialization, media definitions, etc. Finally, the simulation reports
> progress and some final timing statistics. But in this first example the
> result is always zero (and the uncertainty 100%) because no physical quantity
> has been *scored* yet:
>
> ```text
> Running 1000 histories
>
>   Batch             CPU time        Result   Uncertainty(%)
> ==========================================================
>       1                0.08              0         100.00
>       2                0.16              0         100.00
>       3                0.23              0         100.00
>       4                0.31              0         100.00
>       5                0.40              0         100.00
>       6                0.49              0         100.00
>       7                0.56              0         100.00
>       8                0.64              0         100.00
>       9                0.72              0         100.00
>      10                0.80              0         100.00
>
>
> Finished simulation
>
> Total cpu time for this run:            0.80 (sec.) 0.0002(hours)
> Histories per hour:                     4.5e+06
> Number of random numbers used:          13938479
> Number of electron CH steps:            345747
> Number of all electron steps:           1.2252e+06
> ```

## 1.3. Add a dose scoring object

### How much energy is deposited inside the tantalum plate? What is the dose?

> With a dose scoring object, the application echoes additional data to the
> screen at the end of the simulation. The energy deposited in the tantalum
> plate is $\large (2.5506 \pm 1.828\\%)$ MeV. The dose is $\large
> (2.4535\times10^{-12} \pm 1.828\\%)$ Gy.
>
> ```text
> ==> Summary of region dosimetry (per particle)
> ir medium      rho/[g/cm3]  V/cm3      Edep/[MeV]              D/[Gy]
> -----------------------------------------------------------------------------
> 1 tantalum  16.654    10.0000 2.5506e+00 +/- 1.828  % 2.4535e-12 +/- 1.828  %
> -----------------------------------------------------------------------------
> ```

### Can you manually convert the value of deposited energy to dose?

> Dose is defined as the energy deposited per unit mass, usually reported in
> Gray (J/kg). The density of tantalum here is 16.654 g/cm³, and the volume of
> the plate is 10 cm³. Therefore, the mass of the plate is 166.54 g, and the
> dose is:
>
> $$\large  D = \frac{2.5506 \text{ MeV}}{166.54\text{ g}} = 0.015315 \text{\
> MeV/g} = 2.4538 \times 10^{-12} \text{\ J/kg}.$$

### What is the relative uncertainty on energy and dose, and why is it the same for both?

> The relative uncertainty on the deposited energy *and* the dose is 1.828%. It
> is exactly the same in both cases because dose is simply the energy divided
> the mass. The mass is derived from simulation inputs which are considered
> *exact* (the density and the volume), hence it does not contribute to the
> relative uncertainty.

### Why has the deposited energy not increased by a factor 10? What happened to the uncertainty?

> Changing the run control input to `ncase = 1e4` simulates 10⁴ incident
> electrons. The deposited energy is now $\large (2.5893 \pm 0.572\\%)$ MeV. It
> has not increased by a factor 10 because Monte Carlo results are reported *per
> incident particle*. The relative uncertainty has however decreased by a factor
> of about 3, which is typical of a random sampling process, for which the
> uncertainty scales like $\large 1/\sqrt{N}$.

## 1.4. Change the type of incident particles

### Is the simulation faster with electrons or with photons?

> Changing the charge of the source to `charge = 0` runs the simulation for
> incident photons instead of electrons. The simulation is about 10 times faster
> with photons because photons interact with matter much less than do electrons.

### How did the dose change compared to electrons?

> The dose decreased by a factor of about 10 compared to electrons, again
> indicating that photons interact less than do electrons. Correspondingly, the
> uncertainty is larger than with electrons, because fewer events are sampled
> inside the plate.

### Are positrons generated in this simulation? Is this expected?

> Yes, positrons are generated: they show up as blue tracks in the viewer. This
> is expected because the incident photons have an energy of 20 MeV, well above
> the pair production threshold of 1.022 MeV.

## 1.5. Change the energy of incident particles

### What is the biggest qualitative difference with the 20 MeV scenario?

> The most striking difference is that there are much fewer secondary charge
> particles. Note also that there are no positrons since the incident energy is
> now below the pair production threshold of 1.022 MeV.

### Did the dose increase or decrease?

> The dose is further decreased to $\large (5.5088\times 10^{-14} \pm
> 3.613\%)$ MeV per incident photon.

### How about simulation time?

> The simulation time is reduced. As a general rule, lower energy simulations
> run faster because there are fewer secondary particles to transport.

## 1.6. Change the material of the plate

### What is happening to the electrons hitting the lead plate?

> The electrons are either absorbed in the lead or back-scattered in the $\large
> -z$ direction.

### Is that consistent with the deposited energy?

> Yes, indeed. The energy deposited in the lead plate by 100 keV electrons is
> found to be $\large (0.058803 \pm 0.709\\%)$ MeV per incident electron. So on
> average only 60% of the incident energy is deposited in the lead plate, owing
> to the significant back-scatter.

### What is the file size of  `slab.ptracks`  on disk?

> About 5.3 MB. Track files grow very quickly, but by default the number of
> events saved to file is limited to 1024, even if you run more incident
> particles.

### Is deposited energy consistent with the known stopping power value of electrons in liquid water?

> Most electrons go through the water plate, depositing on average 175 keV in
> the process. This is certainly *less* than the energy loss of 245 keV expected
> from the nominal electron stopping power, because a lot of energy is carried
> away from the plate by secondary particles, mostly bremsstrahlung photons.

## 1.7. Monitor a simulation in detail

### What is the most common type of interaction overall in this simulation?

> The most common type of interaction is Møller scattering (159 events),
> followed by bremsstrahlung (25) and photoelectric interactions (14).

### How many incident electrons create an initial bremsstrahlung photon vs a knock-on electron?

> In only one out of ten histories the initial interaction is bremsstrahlung
> (history 5); the other nine initially interact via Møller scattering.

### What is the largest number of particles on the stack at once and how did it occur?

> The maximum number of particles found on the stack simultaneously is 4, and it
> happens twice: once in history 1, and again in history 7.
>
> In history 1 it occurs after a series of Møller interactions by the incident
> electron. At one point a Møller electron generates a bremsstrahlung photon,
> which immediately interacts via the photoelectric effect to release one
> electron. So now there are three electrons on the stack. Finally, this last
> electron generates a fourth electron on the stack via Møller scattering.
>
> In history 7 it occurs in a similar way, except that the last electron is
> generated via a second photoelectric interaction.

### Are most particles discarded because of the energy cutoff, or because they leave the geometry?

> Most particles are discarded because their energy falls below energy cutoff
> values, a total of 173 times. Only on 23 occasions are particles discarded
> because they exit the defined geometry.
