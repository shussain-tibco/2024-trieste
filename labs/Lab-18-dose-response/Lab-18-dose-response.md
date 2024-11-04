<!-- # 1. -->
<!-- # 2. -->
<!-- # 3. -->
<!-- # 4. -->
<!-- # 5. -->
<!-- # 6. -->
<!-- # 7. -->
<!-- # 8. -->
<!-- # 9. -->
<!-- # 10. -->
<!-- # 11. -->
<!-- # 12. -->
<!-- # 13. -->
<!-- # 14. -->
<!-- # 15. -->
<!-- # 16. -->
<!-- # 17. -->

# 18. Dosimetry applications <!-- omit in toc -->

- [18.1. Look at the tracks created by a pencil beam](#181-look-at-the-tracks-created-by-a-pencil-beam)
- [18.2. Look at the tracks created by a 10 MeV pencil beam](#182-look-at-the-tracks-created-by-a-10-mev-pencil-beam)
- [18.3. Calculate the dose response functions (DRFs) for the 1  MeV beam](#183-calculate-the-dose-response-functions-drfs-for-the-1--mev-beam)
- [18.4. Calculate the dose response functions (DRFs) for the 10  MeV beam](#184-calculate-the-dose-response-functions-drfs-for-the-10--mev-beam)
- [18.5. Cheat (highly recommended)](#185-cheat-highly-recommended)

The goal of this lab is to simulate dose response functions (DRFs) of
monoenergetic parallel beams using the 3C chamber model that was defined in
Lab 13. The water volume will be defined as the sensitive volume of the 3C
chamber, including the central electrode, but excluding the wall. To simplify
this example, the pencil beam will not move; instead, we will define a very wide
100 x 100 x 30 cm³ phantom and move the scoring volume while the pencil beam
remains on the z-axis. This will provide a quasi-exact evaluation of the DRFs
for a parallel photon beam.

## 18.1. Look at the tracks created by a pencil beam

### Instructions

1. Download the file  [`3C_pencil.egsinp`](./assets/3C_pencil.egsinp).

2. Run the simulation with this command:

   ```bash
   egs_chamber -i 3C_pencil.egsinp
   ```

3. Visualize the simulation with this command:

   ```bash
   egs_view 3C_pencil.egsinp -p 3C_pencil.ptracks
   ```

### Questions

1. Did you expect to see so many photons from a pencil beam?

2. Turn off the photons and just look at the electrons. How would you define the
   lateral electron range?

## 18.2. Look at the tracks created by a 10 MeV pencil beam

### Instructions

1. In the input file `3C_pencil.egsinp`, change the energy of the beam to 10
   MeV.

2. Run the simulation and view the results as you did earlier.

### Questions

1. Do you see how much the higher energy photons are forward scattered compared
   to the previous example?

2. Turn off the photons and just look at the electrons and positrons. How does
   the lateral electron range of the 10 MeV photon beam change compared to 1
   MeV?

3. Why does the lateral range of positrons appear smaller than for electrons?

## 18.3. Calculate the dose response functions (DRFs) for the 1  MeV beam

### Instructions

1. Download the files [`3C_drf_x_VRT.egsinp`](./assets/3C_drf_x_VRT.egsinp) and
   [`3C_drf_y_VRT.egsinp`](./assets/3C_drf_y_VRT.egsinp).

2. Download the bash script
   [`process_cavity_dose.sh`](./assets/process_cavity_dose.sh).

3. Ensure it can be executed on the terminal with this command:

   ```bash
   chmod u+x process_cavity_dose.sh
   ```

4. Run all input simulations with this command:

   ```bash
   egs_chamber -i filename.egsinp > filename.output
   ```

5. Convert the output with the command:
   ```bash
   ./process_cavity_dose.sh filename.output
   ```

6. View the output with the command:

   ```bash
   xmgrace filename.plotdat
   ```

### Questions

1. For the DRFs in the x direction, identify the position of the chamber
   components and compare with the plans of the 3C. Did you observe what was
   covered in class?

2. Repeat the previous question for the y direction.

3. Determine the minimal field size for the 3C chamber that should cause
   negligible perturbation based on your observations. You can use the cheat
   below.

## 18.4. Calculate the dose response functions (DRFs) for the 10  MeV beam

### Instructions

1. In the files `3C_drf_x_VRT.egsinp` and `3C_drf_y_VRT.egsinp`, change the beam
   energy to 10 MeV.

2. Increase ncase by a factor of 10, and set the cross-section enhancement
   option to 0 (i.e., cs enhancement = 0).

3. Run the simulations, prepare the output to be plotted in `xmgrace`, and
   visualize the results with the application.

### Questions

1. Repeat the questions from the previous section and discuss differences. Can
   you explain them based on the tracks observed in the first section of this
   laboratory?

## 18.5. Cheat (highly recommended)

You can use the script `quick_script_pertb.sh` to generate the perturbation
functions from the `.plotdat` file, which can help determine the smallest field
size suitable for the 3C chamber based on beam energy.

1. Download the bash script [`quick_script_pertb.sh`](./assets/quick_script_pertb.sh).

2. Ensure it can be executed on the terminal with this command:

   ```bash
   chmod u+x quick_script_pertb.sh
   ```

   Convert the output with the command:

   ```bash
   ./quick_script_pertb.sh filename.plotdat
   ```

6. View the output with the command:

   ```bash
   xmgrace filename.pertb
   ```

---

### [Solutions laboratory 18](Lab-18-solution.md)
