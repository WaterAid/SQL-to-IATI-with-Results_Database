# SQL-to-IATI-with-Results-Database

## Contents

1. [Introduction](#intro)
2. [Database Table Schemas](#dts)
3. [Important Functions and Stored Procedures](#functions)
4. [Database Installation](#installation)

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

- __[IATISchema].[f_activitiesXMLFile_201]__ -  This function is used to return valid IATI 2.01 XML data from the IATI Schema database tables. The function takes in a number of different parameters to control what is contained in the returned XML; this is explained further in the documentation associated with the stored procedure. To view this information, right click on the function and select: Script function as -> Create to -> New Query Editor Window

- __[WaterAid].[p_WA_Populate]__ -  This stored procedure is the main driver of the WaterAid data generation process within the database. It extracts WaterAid’s activities data from the tables in the WaterAId schema and transform it, using the data in the Codelist and PublicationControl tables, before saving WaterAid's data into the IATISchema tables.

## <a name="installation"></a>Database Installation Guide 

The following instructions provide guidance on how to create the SQL-to-IATI-with-Results-Database tool from scratch.

The installation script provided has been tested with SQL Server 2012 Express Edition. You will need administrator access to both SQL Server and the database server in order to install the database successfully. 

- Log on to the database server and create a folder called ‘IATIv201’ on the C: drive (e.g. C:\ IATIv201). 

- Open SQL Server Management Studio and run the script ‘IATIv201 – Create Database Script.sql’ to create the database and an associated login.
