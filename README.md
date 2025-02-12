# spectrum2nk

- <a href="#introduction">Introduction</a>
- <a href="#installation">Installation</a>
- <a href="#usage">Usage</a>
- <a href="#limitations">Limitations and future improvements</a>

## [Introduction](#introduction)

This repository provides a JupyterLab interface for converting a reflectance spectrum to its corresponding optical constants. The resulting optical constants are useful for tasks that involve quantitative mineralogical studies, e.g., spectral unmixing.

The code was written in Python 3.13.1 and JupyterLab 4.3.5.

## [Installation](#installation)

- Install Python and JupyterLab, if not already installed.
- Copy all files in this repository to a new project folder.
- Browse to the new folder using a command prompt or terminal.
- (Optional) Set up a virtual environment to avoid package version conflicts (see [here](https://www.zainrizvi.io/blog/jupyter-notebooks-best-practices-use-virtual-environments/) for more details).
- In the command prompt or terminal, type `pip install -r requirements.txt` to install all of the external packages associated with this project.
- Launch JupyterLab from the command prompt (optionally within the newly created virtual environment), and open the notebook file (ending in `.ipynb`).

## [Usage](#usage)

`vnir2nk.ipynb` derives the optical constants from a visible and near-infrared (VNIR) spectrum (~0.4-2.6 μm), plots the input/output spectra, and allows the user to save the results to `.txt` files. 

To begin, highlight the first code cell and press the Run button. Several input fields will be displayed, allowing the user to select a spectrum to be converted as well as to change various parameters. 

If a reflectance spectrum is supplied as the input (e.g., sourced from [RELAB](https://sites.brown.edu/relab/), it will first be converted to a single-scattering albedo (SSA) spectrum, and then to an optical constants spectrum, using the equations of [Hapke (1981)](https://doi.org/10.1029/JB086iB04p03039). If an SSA spectrum is used as the input instead, then it will be converted directly to its optical constants spectrum.

After choosing a spectrum using the Select button, the type of spectrum must then be specified (either reflectance or SSA). In the case of a reflectance spectrum, the following four fields apply. The incidence and emergence angles describe the measurement geometries of the instrument that collected the spectrum. These values are typically 30° and 0° for RELAB specimens, so they were kept as defaults. The phase function and opposition effect values are placeholders pending future implementations of these functions. For now, an isotropic phase function (i.e., 1) and no opposition effect (i.e., 0) was assumed.

Finally, the real index of refraction (n) and grain size (in microns) of the specimen must be provided. In general, the value of n for most minerals can be found in the online [Handbook of Mineralogy](https://handbookofmineralogy.org/), with the grain size of a specimen usually given as metadata in RELAB. Note that n is assumed to be constant. In actuality, its value changes as a function of wavelength, but for simplicity, the conventions of [Lucey (1998)](https://doi.org/10.1029/97JE03145) were followed, where n may be approximated as the average visible refractive index for VNIR wavelengths.

Once all of the fields have been filled out, pressing Convert will calculate the conversions, plot the input/output spectra, and reveal an option to output the results to `.txt` files. After choosing a save location and pressing the Save button, the optical constants spectrum will be outputted in column format, with the first column consisting of wavelengths (in microns), the second column containing the values of n, and the third column containing the values of k (the imaginary index of refraction). SSA spectra will be outputted as two columns, with the second column containing the SSA values.

The `sample_data` folder contains a hematite reflectance spectrum from RELAB (`r_hematite_c1cy11.txt`) which can be used as a benchmark to test the procedure. For this test, the default values in the input fields should be kept the same. The outputs may then be compared with `r_hematite_c1cy11_ssa.txt` and `r_hematite_c1cy11_nk.txt` for consistency.

## [Limitations and future improvements](#limitations)

- The input spectrum file must be two columns wide, with the first column containing wavelengths (in microns) in increasing order, and the second column containing the reflectance (or SSA) values for the associated wavelength. The two columns must be separated by whitespace.
- The phase function and opposition effect values are placeholders. Full implementations (e.g., the Henyey-Greenstein phase function) are forthcoming.
- The bidirectional radiance coefficient is used to implement the reflectance function, according to [Hapke (1981)](https://doi.org/10.1029/JB086iB04p03039). Other methods of computing the reflectance are planned as additional options.
- The shadowing function has not been incorporated into the reflectance function, and is also planned.
- The code is only valid for VNIR spectra (~0.4-2.6 μm). A different method is required for mid-infrared spectra (i.e., Kramers-Kronig dispersion relations), with a future implementation planned.
- Make error checking/handling more robust.
