# VR-Phore Data Analysis

## Methodology

A key distinction between the synoptophore and VR-Phore measurement methodologies lies in how image alignment is achieved.  The synoptophore allows participants to adjust both left and right images simultaneously.  In contrast, VR-Phore uses one image as a stationary reference, allowing participants to manipulate only the other image to achieve alignment. This approach enables precise quantification of binocular alignment within the virtual environment by measuring the deviation of the moving image relative to the stationary reference.


## Experimental Data

The following table presents data from a single participant in the VR-Phore experiment.

| Testno | LeftMaterial             | RightMaterial            | Eye   | xDis       | yDis       | zDis       | MagDis     | Degree      |
|--------|--------------------------|--------------------------|-------|-------------|-------------|-------------|-------------|-------------|
| 1      | BlackCat1 (Instance)     | BlackCat2 (Instance)     | Left  | 0.092771    | 0.031805    | -0.00084    | 0.098075    | 1.123878    |
| 2      | BlackCat2 (Instance)     | BlackCat1 (Instance)     | Right | 0.328212    | -0.06004    | 0.048567    | 0.337174    | 3.864466    |
| 3      | BlackCat2 (Instance)     | BlackCat1 (Instance)     | Left  | -0.00591    | -0.01714    | -0.35197    | 0.352437    | 4.039469    |
| 4      | BlackCat2_fove (Instance)| BlackCat1_fove (Instance)| Right | 0.125868    | 0.08737     | 0.206699    | 0.257295    | 2.948712    |
| 5      | BlackCat2_fove (Instance)| BlackCat1_fove (Instance)| Left  | -0.02239    | 0.114287    | -0.22838    | 0.256356    | 2.937943    |
| 6      | cross2 (Instance)        | cross1 (Instance)        | Right | 0.08976     | -0.05714    | -0.02129    | 0.108512    | 1.243478    |
| 7      | cross2 (Instance)        | cross1 (Instance)        | Left  | -0.0387     | 0.071799    | -0.34198    | 0.351576    | 4.029599    |
| 8      | cross2_fov (Instance)    | cross1_fov (Instance)    | Right | 0.114046    | 0.042349    | 0.077183    | 0.144074    | 1.651018    |
| 9      | cross2_fov (Instance)    | cross1_fov (Instance)    | Left  | 0.068995    | -0.01573    | -0.25379    | 0.263474    | 3.019533    |
| 10     | bucket2 (Instance)       | bucket1 (Instance)       | Right | 0.163759    | 0.042849    | 0.170084    | 0.239961    | 2.750019    |
| 11     | bucket2 (Instance)       | bucket1 (Instance)       | Left  | -0.49431    | -0.08756    | -0.39334    | 0.637745    | 7.312979    |
| 12     | kit2 (Instance)         | kit1 (Instance)         | Right | 1.125022    | -2.51495    | 0.079412    | 2.756253    | 31.99856   |
| 13     | kit2 (Instance)         | kit1 (Instance)         | Left  | 0.939614    | -1.12586    | -0.42948    | 1.528034    | 17.57885   |


## Data Description

* **Testno:** Trial number. Test 1 is a familiarization trial and excluded from analysis.
* **LeftMaterial & RightMaterial:** Slides used, drawn from the synoptophore catalogue.
* **Eye:** Indicates which eye's image was movable (the other served as a reference).
* **xDis, yDis, zDis:**  x-axis, y-axis, and z-axis displacement of the moving image.  Note that z-axis displacement occurs due to the cylindrical movement of the image within the virtual environment, maintaining a fixed distance from the participant's eye, to simulate 2D movement.
* **MagDis:** Total magnitude of displacement.
* **Degree:** Degree of deviation.

## Slide Categorization

* **Tests 2-5:** Fusion slides (2 macular, 2 foveal).
* **Tests 6-9:** Simultaneous Perception slides (2 macular, 2 foveal).
* **Tests 10-13:** Stereopsis slides (4 macular).

