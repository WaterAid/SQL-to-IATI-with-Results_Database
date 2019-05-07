# SQL-to-IATI-with-Results-Database

## Contents

What we want to achieve is to show people a way to instal and customise the tables themselves. 

WaterAid thas adapted the IATI tool to make it work for the specific requirements and other organisations can instal the tool and adapt it as well. We want to show the example of how WaterAid has done this; show spreadsheet, example of xml and WAIATI tables export 
To get the maximun value of the tool, it would be to integrate the WAIATI data model with the data mart and have automatic generation of the 



Add additional supporting documents such as the iati spreadsheet that is used to import data and an export of the WAIATI tables so people can understand the data model

1. [Introduction](#intro)
2. [Database Table Schemas](#dts)
3. [Important Functions and Stored Procedures](#functions)
4. [Database Installation](#installation)
5. [Additional Scripts](#scripts)
7. [Glossary](#glossary)

## <a name="intro"></a> Introduction

The SQL-to-IATI-with-Results-Database is a new version of DFID IATIv201 database which has been created and is used by WaterAid to generate IATI 2.01 standard data, **including results data**, in the XML file format. The tool is the result of extending DFID SQL-to-IATI-Database functionality using SQL Server. The code and further information about DFID SQL-to-IATI-Database can be found in [https://github.com/DFID/SQL-to-IATI-Database.](https://github.com/DFID/SQL-to-IATI-Database)

This document will briefly outline some of the Database’s main features and contains an installation guide to get you started.

## <a name="dts"></a> Database Table Schemas

The Database contains four schemas and the tables within each schema perform a particular purpose:

- __Codelist__ - 	The tables in this schema are used to hold the data from the IATI 2.01 codelist within the database structure.
- __IATISchema__ - 	The tables in this schema are used to hold IATI activity data.
- __Configuration__ -	The tables in this schema hold information relating to the generation of the IATI data (e.g. Configuration.Error logs any issues that arise when generating IATI data).
- __PublicationControl__ -	The tables in this schema are used to control the IATI data generation process and control what information is saved into the ‘IATISchema’ tables (e.g. PublicationControl.ExclusionDetails holds all of the IATI-activities that are excluded from publication).	
- __WAIATI__ - 	The tables in this schema hold aggregated data about WaterAid activities that will be published in IATI following WaterAid data model. These tables hold the source data for the IATI publication. They can be populated automatically from WaterAid DataMart by store procedures or manually importing the data from spreadsheets.

## <a name="functions"></a>Important Functions and Stored Procedures

- __[IATISchema].[p_Populate]__ - This stored procedure is the main driver of the DFID data generation process within the database. It extracts DFID’s financial information from a DataMart that is linked to the organisation’s Enterprise Resource Planning system and transforms it, using the data in the Codelist and PublicationControl tables, before saving DFID’s IATI data into the IATISchema tables.

- __[IATISchema].[f_activitiesXMLFile_201]__ -  This function is used to return valid IATI 2.01 XML data from the IATI Schema database tables. The function takes in a number of different parameters to control what is contained in the returned XML; this is explained further in the documentation associated with the stored procedure. To view this information, right click on the function and select: Script function as -> Create to -> New Query Editor Window

- __[WaterAid].[p_WA_Populate]__ -  This stored procedure is the main driver of the WaterAid data generation process within the database. It extracts WaterAid’s activities data from the tables in the WaterAId schema and transform it, using the data in the Codelist and PublicationControl tables, before saving WaterAid's data into the IATISchema tables.

## <a name="installation"></a>Database Installation Guide 

The following instructions provide guidance on how to create the SQL-to-IATI-with-Results-Database tool from scratch.

The installation script provided has been tested with SQL Server 2012 Express Edition. You will need administrator access to both SQL Server and the database server in order to install the database successfully. 

- Log on to the database server and create a folder called ‘IATIv201’ on the C: drive (e.g. C:\ IATIv201). 

- Open SQL Server Management Studio and run the script ‘IATIv201 – Create Database Script.sql’ to create the database and an associated login.

## <a name="scripts"></a>Additional Scripts

- The script **IATIv201 - Create WAIATI tables** is used to create WaterAid WAIATI schema and tables described in the [Database Table Schemas](#dts) section earlier in this document. It is a useful guide for organisations creating tables according to their own data model.
- The script **IATIv201 - WaterAid Data Population Function.sql** is used to create the WaterAid p_WA_populate function, which is described in the [Important Functions and Stored Procedures](#functions) section earlier in this document. It is a useful guide to show how to populate the IATISchema tables from the source tables holding the activity data (WAIATI tables as described earlier)
- The script **IATIv201 – DFID Data Population Function.sql** is used to create the DFID p_populate function, which is described earlier in this document. As this stored procedure is tightly coupled to DFID’s DataMart, it has been removed from the ‘IATIv201 – Create Database Script.sql’ script. However, as its functionality has been broken down into discrete SQL blocks, which have all been commented, it is a useful guide to show how DFID approaches the extraction and transformation of data from our DataMart. 

- **IATIv201 – Views on the DFID Database.sql** – these views are tightly bound to DFID’s Datamart and so cannot be created, but they may prove informative as they show what data is being extracted.

## <a name="glossary"></a>Glossary

There are some table/column names in the Database’s “PublicationControl” schema which relate to DFID specific terminology; these will be outlined below to avoid confusion:

- __Quest__ - This is the name of DFID’s document repository. A document’s Quest Number is its unique reference within this system.
- __Project__ - This is the name that is used within DFID to describe hierarchy one iati-activities.
- __Component__ - This is the name that is used within DFID to describe hierarchy two iati-activities. 
- __ARIES__ - This is the name of DFID’s Enterprise Resource Planner system, 
a project’s ARIES ID is effectively a hierarchy one iati-activity ID.

