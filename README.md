# StAZH Transkribus API
The State Archives of the Canton of Zurich uses Transkribus to edit various collections. This API is based on the [TranskribusPyClient](https://github.com/Transkribus/TranskribusPyClient) and allows to execute some functions in Transkribus in batch, respectively to automate certain work steps.

**Note:** The program is still in progress.

## Table of content:
- [Usage](#usage)
- [Requirements](#requirements)
- [Login](#login)
- [Module Line detection](#module-line-detection)
- [Module Search and Replace](#module-search-and-replace)
- [Module Sampling](#module-sampling)
- [Module Export text region](#module-export-text-region)
- [Module Import](#module-import)

## Usage:

In order to run the program open the command prompt, navigate to the src folder and run the following command:

```
	python TranskribusAPI.py
```

## Requirements:

The following libraries need to be installed in order for the program to run:

```
xlwings==0.20.8

numpy==1.19.2

pandas==1.1.3

requests==2.24.0
```

You can install them by running the following command in the command line after navigating to the base folder:

```
	pip install -q -r requirements.txt
```
## Login:

Fill in your Transkribus user credentials. Normally no proxy information is needed. 

## Modules:
### Module Line detection 

#### Functionality:

This module provides line detection on specified pages and text regions. 
This can be useful in case the line detection of your trained P2PaLA Model is not sufficient and also in case you face lines crossing multiple text regions.

#### Parameters:
- Collection id:
	- Here the id of a collection has to be added.
- Document id:
	- The id of a document.
- Start Seite:
	- The starting page from which the line detection should start. (Counting from 1)
- End Seite:
	- The ending page of the line detection. If "-" is passed the process will go up to the last page.
- Textregionen (Komma separiert):
	- Here the text regions need to be specified for which line detection should be executed. If mulitple text regions are passed they need to be separated by ",".

#### Output:

None - The script submits the jobs directly to Transkribus and the results can be seen there.

### Module Search and Replace

#### Functionality:

Search and replace names of text regions or text in pagexml.

#### Parameters:
- Collection id:
	- Here the id of a collection has to be added.
- Document id:
	- The id of a document.
	
#### Output:

None - The script submits the jobs directly to Transkribus and the results can be seen there.

### Module Sampling

#### Functionality:

This module provides a way to evaluate a certain model on a document or an entire collection. It is especially useful to generate the CER and WER value of a model for all samples in a collection. The results will be written to an excel file.

#### Parameters:

- Collection id:
	- Here the id of a collection has to be added.
- Document id:
	- The id of a document (empty if you want to evaluate all samples in the collection) 
- Modelle:
	- Here a model can be selected. All models available in Transkribus should be listed.
	  Note that first one has to sync the models by clicking "Modelle abrufen".
- Zielordner:
	- Here you need to define where you want to save the excel file that is generated by the program.

#### Output:

- ModelEvaluation.xlsx - The program writes the results in an excel file in the defined path (Zielordner).
- Folders with images of the lines with best and worst error rate in the defined path (Zielordner).

### Module Export text region

#### Functionality:

With this module you can export text, tags and images of a specific textregion to an excel file.

#### Parameters:

- Collection id:
	- Here the id of a collection has to be added.
- Document id:
	- The id of a document.
- zu exportierende Textregion:
	- A text region name that defines the region which we want to export (only one region name).
- Checkbox:
	- Select if each line in a text region should form a separate line in the generated excel file.
- Start Seite:
	- The starting page. (Counting from 1)
- End Seite:
	- The ending page. If "-" is passed the process will go up to the last page.
- Zielordner:
	- Here you need to define where you want to save the excel file that is generated by the program.
#### Output:

RegionExtraction_docId_TextregionName_(lines or regions).xlsx - The program writes the exported text regions in an excel file.

### Module Import 

#### Functionality:

This module enables previously exported and afterwards edited texts and tags to be imported back into Transkribus.

#### Parameters:

- Collection id:
	- Here the id of a collection has to be added.
- Checkbox:
	- Select if text regions are to be imported. Only the tags of the text regions can be imported (no texts). This is especially useful for (re)naming text regions according to a certain rule.
- CSV mit Importdaten auswählen:
	- Select the CSV-File with the Data to be imported. 
	  	- If the checkbox is selected, the CSV file must have the fields *Document Id, PageNo, Text Region Id, Tag*
	  	- If the checkbox is not selected, the CSV file must have the fields *Document Id, PageNo, Text Region Id, Text, Tag*
#### Output:

None - The results can be seen in Transkribus.
