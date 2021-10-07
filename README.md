# SDG Hackathon 2021
## _The Swiss hackathon on Sustainable Development Goals visualisation_

## Overview
This Hackathon aims at creating visualisations that provide insights on the amount and quality of research in the direction of achieveng the Sustainable Development Goals established by the United Nations.

The data that is provided for this sutdy is the P3 database containing the projects that have been approved for funding by the Swiss National Science Foundation (SNSF) between 1980(?) and Aug-2021, downloaded from https://p3.snf.ch/Pages/DataAndDocumentation.aspx.

## Description of the datasets
### 1) SNSF_Projects.csv
The dataset contains the following columns:

_[Dan] I suggest removing from the dataset the columns whose description is N/A_

|Column #|Name|Type|Description|Type of entry|
| ------ | ------ | ------ | ------ | ------ |
|1| Project Number | numeric |Project identifier.||
|2| Project Number String | text |Full-text project identifier.||
|3| Project Title | text |Short description of the project.||
|4| Project Title English | text |N/A||
|5| Responsible Applicant | text |N/A||
|6| Funding Instrument | text |Research funding scheme as defined by https://www.snf.ch/en/9o5ezhuSlHENVQxr/page/overview-of-funding-schemes||
|7| Funding Instrument Hierarchy | text |Top level hierarchy of the research funding scheme.||
|8| Institution | text |Institution where the project will largely be carried out.|Free text|
|9| Institution Country | text |The country of the institution. Most international locations are related to mobility fellowships.|List|
|10| University | text |Institution where the project will largely be carried out, based on a list to pick at the moment of the application.|List|
|11| Discipline Number | numeric |Discipline ID defined by the SNSF. Defined for the main discipline.|List|
|12| Discipline Name | text |Discipline name defined by the SNSF. Defined for the main discipline.|List|
|13| Discipline Name Hierarchy | text |The hierarchy of the main discipline.||
|14| All disciplines | text |List of all the discipline IDs involved in the project.||
|15| Start Date | text |Date at which the project starts (dd.mm.yyyy).|Free text|
|16| End Date | text |Actual date at which the project ends. Updated if necessary (dd.mm.yyyy).|Free text|
|17| Approved Amount | text |Approved funding amount. Updated if modified. Format is text because not always a number is stored. Ex: it may say "not included in P3".||
|18| Keywordstext | text |Keywords related to the project.|Free text|
|19| Abstracttext | text |The scientific abstract of the research project. May be edited by the applicants online. The researchers are responsible for the contents.|Free text|
|20| Lay Summary Lead (English) | logical |N/A||
|21| Lay Summary (English) | text |N/A||
|22| Lay Summary  Lead (German) | logical |N/A||
|23| Lay Summary (German) | text |N/A||
|24| Lay Summary Lead (French) | logical |N/A||
|25| Lay Summary (French) | text |N/A||
|26| Lay Summary Lead (Italian) | logical |N/A||
|27| Lay Summary (Italian) | text |N/A||
|28| abstract_translated| text |Abstract translated to English for the abstracts that were translated.|Free text|
|29| abstract_translated_yes_no| text |Flag indicating whether the abstract has been translated to English.|Free text|
|30| abstract_english| text |Abstract in English, either the original one or the translated one.|Free text|

### 2) SNSF_Projects_SDGS.csv
The dataset contains the following columns:

TBD