# Upload pdf files to DataverseNL

## Background

In February 2025, a researcher from the faculty of Social and Behavioural Sciences needed help with uploading over a hundred pdf files to DataverseNL.
Many articles from the Dutch journal "Gedrag en Organisatie" (in between its inception and 2004) were never published online. 
The PsychInfo database does however have a bunch of metadata available from some of these journal articles, and the researcher had a local archive of the pdf files (the papers) from  the journal. Although it is not preventable that some papers would have to be published manually (and providing manual metadata), the journals for which there is PsychInfo metadata available *can* be uploaded via a script.

In this file, the Python code can be found that uses the [Dataverse Native API](https://guides.dataverse.org/en/latest/api/native-api.html) to upload all papers to DataverseNL for which:

- There is a pdf file in the researcher's local `archief` folder
- There is metadata from the PsychInfo database
- There is no DOI (which means it already is available online) and the publication date is before 2004

## Prerequisites

- a folder called 'Archief' that contains all the pdf files to be uploaded. The Archief folder is not included in this repository, but there is a txt file that gives the folder tree (`sourcefiles/GenO_dataverse_foldertree.txt`).
- a `citation.xlsx` file, which is a PsychInfo export file. This folder is not included in this repository (due to possible copyright issues), but the column labels are included in the file `sourcefiles/citation_labels.csv`.
- a json file with dataverse metadata fields to be filled. Here this is `dataverse_metadata/metadata_GO_v1.json`
- admin (or at least write) access to the specified Dataverse collection, and thus the API token and Dataverse ID of that collection.
- a Python installation

## Structure of this document

- Import libraries
- Read in psychinfo export file (`citation.xlsx`) in a pandas dataframe
- For every pdf in the Archief folder, get the authors, year, volume and issue and put that in a pandas dataframe (`extract_info` function) > result: `results/01_extracted_path_information_archief_folder.csv`
- Merge the psychinfo export and the Archief results to see which files overlap: select only those that are present in both (meaning that there is a pdf file (from Archief folder) and sufficient metadata (from the Psychinfo export)) so that they can be uploaded to DataverseNL. > result: `results/02_full_merge_archief_and_psychinfo.csv` and subsets (`2a`, `2b`, `2c`).
- For each row in the merged dataframe for which there is both a pdf and psychinfo metadata:
  - Fill the Dataverse metadata JSON (function `create_dv_metadata`)
  - Check if a dataset with the same name has already been uploaded, skip that one then (functions `get_dataverse_dois`, `retrieve_titles`)
  - Create a draft Dataverse dataset with the metadata and retrieve its PID (function `create_dataset`) 
  - Change the publication date to the journal publication date (otherwise every entry gets 2025 as citation date; function `update_citation_date`)
  - Upload the accompanying pdf file to the dataset using the PID (function `upload_data`) > result: `results/03_Files_uploaded_to_Dataverse.csv`