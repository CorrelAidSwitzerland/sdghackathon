# SDG Hackathon 2021
## _The Swiss hackathon on the visualisation of Sustainable Development Goals_

## Overview
This Hackathon aims at creating visualisations that provide insights on the amount and quality of research in the direction of achieving the 17 Sustainable Development Goals (SDG) established by the United Nations at the Earth Summit in Rio de Janeiro in 1992.

The data provided for this hackathon is a subset of the P3 database (Projects-People-Publications) organised and made publicly available by the Swiss National Science Foundation (SNSF). The P3 database contains the projects that have been approved for funding by SNSF between 1975 and the present.

The data has already been downloaded by us from https://p3.snf.ch/Pages/DataAndDocumentation.aspx, and then prepared for the Hackathon with the following changes:
- the dataset was filtered to projects that contain an abstract; this eliminated all the projects funded prior to 2006; 
- non-English abstracts were translated to English using the [EasyNMT](https://pypi.org/project/EasyNMT/) Python package;
- SDGs were detected in all the abstracts (once translated if needed) using the [text2sdg](https://cran.r-project.org/web/packages/text2sdg/index.html) R package

The detailed changes carried out on the original dataset are described in the Appendix.
The next section outlines the process used to detect SDGs in the abstracts.

## SDG detection process
The [text2sdg](https://github.com/dwulff/text2sdg) R package was used to detect SDGs in the project abstracts. It implements five query-based systems that are used to detect SDGs in text.

From a first analysis performed by our team, they differ in a few aspects, including:
- the logic behind the definition of the queries and keywords used;
- the effort devoted to the development of the system;
- the expertise of the team responsible of their design.

There is likely an intrinsic trade-off between quality and quantity of the query hits delivered by each system. To the best of our knowledge, a rigourous validation of their performance and quality does not yet exist.

The five systems are shortly described below, listed in alphabetical order:
* **Aurora**: These queries were developed by the [Aurora Universities Network](https://aurora-network.global/activity/sustainability/). The Aurora queries were designed to be precise rather than sensitive. To this end, queries have been designed to make use of complex keyword-combinations using several different logical search operators (e.g. 'and', 'or', etc.). [Version 5.0](https://github.com/Aurora-Network-Global/sdg-queries) is used in the 'text2sdg' package.
_All SDGs (1-17) are covered_
* **Elsevier**: A dataset containing the SDG queries developed by [Elsevier](https://www.elsevier.com/connect/sdg-report). The queries are available from [data.mendeley.com](https://data.mendeley.com/datasets/87txkw7khs/1). The Elsevier queries were developed to maximize SDG hits on the [Scopus database](https://dev.elsevier.com/documentation/ScopusSearchAPI.wadl). A detailed description of how each SDG query was developed can be found [here](https://elsevier.digitalcommonsdata.com/datasets/87txkw7khs/1). Version 1 is used in the 'text2sdg' package. 
_SDGs 1-16 are covered (i.e. SDG-17 is not covered)_
* **Ontology**: A dataset containing the SDG queries based on the keyword ontology of Bautista-Puig and Maule?n (2019)[1]. The queries are available from [figshare.com](https://figshare.com/articles/dataset/SDG_ontology/11106113/1). The authors of these queries first created an ontology from selected keywords present in the SDG descriptions and expanded them with keywords from publications from the Web of Science which included the phrases "Millennium Development Goals" and "Sustainable Development Goals".
_All SDGs (1-17) are covered_
* **SDSN**: A dataset containing SDG-specific keywords compiled from several universities from the Sustainable Development Solutions Network (SDSN) Australia, New Zealand & Pacific Network. The authors used UN documents, Google searches and personal communications as sources for keywords.
_All SDGs (1-17) are covered_
* **Siris**: These queries were developed by [SIRIS Academic](http://www.sirislab.com/lab/sdg-research-mapping/). The queries are available from Zenodo.org. The SIRIS queries were developed by extracting key terms from the UN official list of goals, targets and indicators as well from relevant literature around SDGs. The query system has subsequently been expanded with a pre-trained word2vec model and an algorithm that selects related words from Wikipedia.
_SDGs 1-16 are covered (i.e. SDG-17 is not covered)_


## Description of datasets
Two datasets are made available in the repository:
- **Main dataset**: `main_dataset_with_sdg.csv`, which contains all the relevant information.
- **Extended dataset**: `additional_dataset_without_sdg.zip`, which contains additional columns --see descriptions in item (2) below--. This dataset can be joined with the main dataset on the `project_number`, if one is interested in using this data.

### Reading and joining the datasets in R and pyhton

#### R

```
library(readr)
main_df <- read_csv("main_dataset_with_sdg.csv")
additional_df <- read_csv("additional_dataset_without_sdg.zip")

#join on main dataset
library(dplyr)
main_df <- main_df %>% 
  left_join(additional_df)
```

#### Python

```python
import pandas as pd

main_df = pd.read_csv("main_dataset_with_sdg.csv")
additional_df = pd.read_csv("additional_dataset_without_sdg.zip")

#join on main dataset
main_df = main_df.merge(additional_df, left_on = "project_number", right_on = "project_number", how = "left")
```


### 1) Detailed description of the main dataset (`main_dataset_with_sdg.csv`)
The dataset contains the following columns:

|Column #|Name|Type|Description|
| ------ | ------ | ------ | ------ |
|1| project_number | numeric |Project identifier.|
|2| University(*) | text |Institution where the project will largely be carried out, based on a list to pick at the moment of the application.|
|3| Funding Instrument | text |Research funding scheme as defined by https://www.snf.ch/en/9o5ezhuSlHENVQxr/page/overview-of-funding-schemes|
|4| Discipline Name(*) | text |Discipline name defined by the SNSF. Defined for the main discipline.|
|5| sdg | text | SDG that has been detected, NA if no SDG has been detected in this document by the given system |
|6| system | text | Query system used to detect SDG (see details in Section "SDG detection process") |
|7| hits | numeric | How many hits were produced for a given SDG for the given document by the given system |
(*) These columns are filled out from a drop-down list provided by the SNSF submission system.

### 2) Detailed description of the extended dataset (`additional_dataset_without_sdg.zip`)
The dataset contains the following columns:

|Column #|Name|Type|Description|Type of entry|
| ------ | ------ | ------ | ------ | ------ |
|1| project_number | numeric |Project identifier.|
|2| project_title | text |Short description of the project.|
|3| funding_instrument_hierarchy | text |Top level hierarchy of the research funding scheme.|
|4| institution | text |Institution where the project will largely be carried out.|Free text|
|5| institution_country(*) | text |The country of the institution. Most international locations are related to mobility fellowships.|
|6| discipline_number(*) | numeric |Discipline ID defined by the SNSF. Defined for the main discipline.|
|7| discipline_name_hierarchy | text |The hierarchy of the main discipline.|
|8| all_disciplines | text |List of all the discipline IDs involved in the project.|
|9| start_date | text |Date at which the project starts (dd.mm.yyyy).|
|10| end_date | text |Actual date at which the project ends. Updated if necessary (dd.mm.yyyy).|
|11| approved_amount | text |Approved funding amount. Updated if modified. Format is text because not always a number is stored. Ex: it may say "not included in P3".|
|12| keywords | text |Keywords related to the project.|
|13| abstract_translated| text |Flag indicating whether the abstract has been translated to English.|Free text|
|14| abstract | text | The scientific abstract of the research project in English, either the original one or the translated one.|
(*) These columns are filled out from a drop-down list provided by the SNSF submission system.

## Appendix
#### Changes carried out to the original P3 database
- All the projects that did not have an abstract were deleted.
- The following columns have been deleted:
-- Project Title English | text 
-- Responsible Applicant | text 
-- Lay Summary Lead (English) | logical 
-- Lay Summary (English) | text 
-- Lay Summary  Lead (German) | logical 
-- Lay Summary (German) | text 
-- Lay Summary Lead (French) | logical 
-- Lay Summary (French) | text 
-- Lay Summary Lead (Italian) | logical 
-- Lay Summary (Italian) | text 
-- Project Number String | text
-- Abstract | text --> This is the original abstract of the research project. It differs from `abstract_english` (provided in the main dataset) in that it is written in the original language when it is not English.

#### References
[1] Bautista-Puig, N.; Maule?n E. (2019). Unveiling the path towards sustainability: is there a research interest on sustainable goals? In the 17th International Conference on Scientometrics & Informetrics (ISSI 2019), Rome (Italy), Volume II, ISBN 978-88-3381-118-5, p.2770-2771.
