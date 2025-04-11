# Restrict all publications in a Dataverse and Deaccession V1

## Background

All publications in the GenO Archive Dataverse (<https://dataverse.nl/dataverse/GenO_Archive>) need to be set to restricted access in a newer version, and the previous version deaccessioned, due to potential copyright issues.

## Prerequisites

- Admin access to a dataverse
- Python installation

## ⚠️ Current issues with this script ⚠️

⚠️ At the moment it is not yet possible in the DataverseNL instance of Harvard Dataverse to restrict files via the API - or at least I have not found a solution to do so. The function `restrict_file` in this notebook therefore does not work; you have to indicate the terms of use/that you want to enable access requests, which is not possible via the API (but will be, I think: <https://github.com/IQSS/dataverse/pull/11349>).

## Structure of this document

- Import libraries
- Find all DOIs and dataset IDs of the GenO Archive dataverse, and put them together in a pandas dataframe (function `get_dois_and_ids`)
- Look for the file ID(s) per dataset and add them to the pandas dataframe (function `retrieve_file_ids`)
- ⚠️ Set all files to restricted access > Does not work (yet?) (function `restrict_file`)
- Change the publication date to the Journal publication date (function `update_citation_date`)
- Publish as new version of the dataset (function `publish_dataset`)
- Deaccession version 1 of each dataset (function `deaccession_dataset`)