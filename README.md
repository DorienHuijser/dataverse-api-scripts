# Dataverse API scripts

[![DOI](https://zenodo.org/badge/964625618.svg)](https://doi.org/10.5281/zenodo.15198786)

This repository currently contains 2 Jupyter notebooks that interact with the Harvard Dataverse Native API.

- The script in `upload_papers_GenO` generates Dataverse-compliant metadata and creates new datasets in a specified dataverse.
- The script in `deaccession_GenO` retrieves DOIs, dataset IDs and file IDs, attempts (**but fails!!**) to restrict the file IDs, publishes a new version of the dataset, and deaccessions v1.0 of the dataset. The file restriction does not work due to (I think!) a limitation in the Dataverse API. 

Further documentation can be viewed in each folder's README and Jupyter notebook.

## Prerequisites

The notebooks both use the `requests` Python library to interact with the Dataverse API. I could not get the `pydataverse` package to work, because that package is not maintained and its metadata schema is too old so it is not compliant anymore with the current Dataverse metadata requirements.

You will need write and in some cases (e.g., when deaccessioning) admin access to the relevant Dataverse collection.

## Usage

It is not possible to plainly re-run all the code in these Jupyter notebooks, since they work with a specific Dataverse ("GenO_Archive") and use specific files which are not included in this repository. However, in both notebooks I have attempted to create separate functions that can easily be reused.

## License

The code is licensed under [GPL-3.0](https://github.com/DorienHuijser/dataverse-api-scripts#GPL-3.0-1-ov-file).

## Contact

If you have questions about this code, or suggestions for improvement, feel free to open an Issue or a Pull request.
