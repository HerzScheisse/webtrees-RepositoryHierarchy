<a name="Overview"></a>				
##  Repository Hierarchy Overview
A [weebtrees](https://webtrees.net) custom module to present the structure of a repository and its sources in a hierarchical manner. Based on the hierarchical structure, a finding aid document (HTML or PDF export) can be generated . The module also provides an **EAD XML export**, which enables data exchange and linking with an external archive management system. The EAD XML export is also compatible with apeEAD, which allows data exchange with an archive portal.

The module uses delimiters to cut call numbers (of sources) into sub-strings and extracts call number categories. Based on the extracted categories, a hierarchical tree of call number categories with the related sources is constructed and shown.

Example call numbers:
+ Fonds A / Record group X / Series 1 / Folder A23 / Source 11
+ Fonds A / Record group X / Series 1 / Folder A23 / Source 12
+ Fonds A / Record group X / Series 2 / Folder B82 / Source 51

Delimiter: 
" / "

Resulting repository hierarchy:
+ Fonds A
    + Record group X
        + Series 1
            + Folder A23
                + Source 11
                + Source 12
        + Series 2
            + Folder B82
                + Source 51

To specify delimiters, the module can handle single characters as well as complex [regular expressions](https://en.wikipedia.org/wiki/Regular_expression):
+ In the most simple case, delimiters are single characters or strings, e.g.: "/" or " - "
+ For more complex string pattern recognition, regular expressions can be used, which contain the delimiter in brackets, e.g.: "Series( )Nr" or "\[a-z\](, )\[0-9\]"
+ Also, a set of delimiters or regular expressions can be used separated by ";", e.g.: "/;-" or "- ;Series( )Nr"

<a name="Screenshot"></a>
##  Screenshot
![Screenshot](resources/img/screenshot.jpg)

<a name="Contents"></a>				
##  Table of Contents
This README file contains the following main sections:
*   [Overview](#Overview)
*   [Screenshot](#Screenshot)
*   [Table of Contents](#Contents)
*   [What are the benefits of using this module?](#Benefits)
*   [Installation](#Installation)
*   [Webtrees Version](#Version)
*   [Concepts of the Repository Hierarchy Module](#Concepts)
*   [**How to use the module?**](#Usage)
    *   [Using delimiters](#Using-delimiters)
    *   [Save and load options](#Using-save-load)
    *   [Rename a call number category](#Using-rename)
    *   [Generate and download a finding aid document](#Using-finding-aid)
    *   [Generate and download an EAD XML export](#Using-EAD-xml)
    *   [Settings for EAD XML exports](#Using-xml-settings)
    *   [Export data to an external archive management system](#Using-export)
    *   [Create links between webtrees and an external archive management system](#Using-links)
    *   [Show additional source and repository information in source citations](#Using-citations)
    *   [Preferences](#Preferences)
*   [Some more background about archive and library management](#Background)
    *   [Archival Arrangement](#Arrangement)
    *   [Library classification](#Classification)
    *   [Finding Aids](#Finding_aids)
    *   [Call numbers](#Call_numbers)
    *   [Relationship between Archival Arrangement and Call numbers](#Relationship)
    *   [EAD standard for XML export of archival records](#EAD)
    *   [apeEAD standard for XML export of finding aids](#apeEAD)
*   [How the module maps to Gedcom and to archive management concepts](#Mapping)
*   [Github Repository](#Github)

<a name="Benefits"></a>
##  What are the benefits of using this module?
+ Provide a better overview of sources in a repository
+ Improve the insight into repository structures and content
+ Find and remove inconsistencies between call numbers in repositories
+ Get additional features to rename call number categories (or groups of call numbers)
+ Get better support to design an archive arrangement/classification for your own archive and manage the corresponding call numbers
+ Generate a finding aid document (i.e. table of content or catalog) for a repository
+ Export the data of a repository to an external archive management system by using the EAD XML export
+ Create links between webtrees and an external archive management system
+ Generate a standardized archive EAD XML file to export the data of a repository to an archive portal

<a name="Installation"></a>
##  Installation
+ Download the [latest release](https://github.com/Jefferson49/RepositoryHierarchy/releases/latest) of the module
+ Copy the folder "repository_hierarchy" into the "module_v4" folder of your webtrees installation
+ Check if the module is activated in the control panel:
    + Login to webtrees as an administrator
	+ Go to "Control Panel/All Modules", and find the module called "Repository Hierarchy" (or corresponding translation)
	+ Check if it has a tick for "Enabled"

<a name="Version"></a>
##  Webtrees version  
The latest release of the module was developed and tested with [webtrees 2.1.7](https://webtrees.net/download), but should also run with any other webtrees 2.1 version.

<a name="Concepts"></a>
##  Concepts of the Repository Hierarchy Module
In the following, the concepts of the Repository Hierarchy module are described. 

###  Call number categories
In the module, a new concept "Call numbers category" is introduced. Call number categories are defined as hierarchical elements, which constitute the structure of an archival arrangement.

###  Relationship between call number categories, call numbers, and delimiters
Call number categories are extracted form call numbers. The module identifies sub-strings in call numbers as call number categories by using delimiters. A chosen delimiter (or a set of delimiters) cuts the full call number into sub-strings of call number categories.

Example call number structure:
"Fonds/Record group/Series/Folder/Source"

In this case, the module identifies the following strings as **call number categories**:
+ Fonds
+ Record group
+ Series
+ Folder
+ Source

Based on the identified call number categories, the module creates the following hierarchical structure for the archive:
+ Fonds
    + Record group
        + Series
            + Folder
                + Source
				
###  Delimiter expressions for call numbers
A delimiter is a sequence of one or more characters for specifying the boundary between separate, independent regions in a text. In the Repository Hierarchy module, delimiters are used to cut call numbers into sub-strings of call number categories. The call number categories will be used to construct a hierarchy of call numbers.

<a name="Usage"></a>
##  How to use the module?

<a name="Using-delimiters"></a>
###  Usage of a single delimiter
A single delimiter is used by providing a single character or a sequence of characters in the related input form ("delimiter expression"). 

Example:
+ Call numbers:
    + Fonds/Series/Item 1
    + Fonds/Series/Item 2
+ Delimiter expression: **/**
+ Repository Hierarchy:
    + Fonds/
        + Series/
            + Item 1
            + Item 2

###  Usage of a set of delimiters
A set of delimiters is used by providing the delimiters in the input form ("delimiter expression") separated by "**;**".

Example:
+ Call numbers:
    + Fonds/Series-Item 1
    + Fonds/Series-Item 2
+ Delimiter expression: **/;-**
+ Repository Hierarchy:
    + Fonds/
        + Series-
            + Item 1
            + Item 2

In a set of delimiters, the delimiters are evaluated from left to right, i.e. the most left delimiter is evaluated first. Delimiters will also be applied recursively for as many matches as possible. Only if no further matches of a delimiter are found, the next delimter is evaluated.

Example:
+ Call number:
    + Fonds A/Record-group/Series A-Nr. 7
+ Delimiter expression: **/;-**
+ Repository Hierarchy:
    + Fonds A
        + Record-group
            + Series A
                + Nr. 7		

I.e. the "-" delimiter in "Record-group" is not evaluated, because the "/" delimiter is evaluated first. After all matches of the "/" delimiter have been evaluated, the "-" delimiter is found in "Series A-Nr. 7".
	
###  Usage of a regular expression for the delimiter
A [regular expression](https://en.wikipedia.org/wiki/Regular_expression) is used by providing it in the input form ("delimiter expression"). The regular expression needs to contain the delimiter in brackets. This provides a much more powerful way to specify delimiters.

Please note, that the "full" regular expression will be used to find a certain pattern in the call numbers. However, **only the characters in the brackets** ("the match" of the regular expression) **will be used as the delimiter**.

Example:
+ Call numbers:
    + Film Number 5
    + Film Number 8
+ Delimiter expression: **Film( )Number**
+ Repository Hierarchy:
    + Film
        + Number 5
        + Number 8

In this example, the delimiter is the space character in the brackets, i.e. "**( )**". However, the full pattern "**Film( )Number**" is used to find corresponding strings. Therefore, only space characters, which match the pattern, are identified as delimiter. Other space characters, are NOT identified as delimiter.

###  Usage of a set of regular expressions for the delimiter
A set of regular expressions can be used by providing several regular expressions in the input form ("delimiter expression") separated by "**;**". It is also possible to mix simple delimiters (i.e. a single character or sequence of characters) with regular expressions.

Example:
+ Call numbers:
    + Fonds A, Biography Number 1
    + Fonds D, Photo Number 7
+ Delimiter expression: **Fonds \[A-D](, );( )Number**
+ Repository Hierarchy:
    + Fonds A,
        + Biography
            + Number 1
    + Fonds D,
        + Photo
            + Number 7

Like for a set of simple delimiters, the delimiter expressions are evaluated from left to right. Please refer to the description and example for a set of simple delimiters.

<a name="Using-save-load"></a>
###  Save and load options
By pressing certain radio buttons in the front end, certain load and save operations can be executed while pressing the "view" button. 

####  Save and load a repository
If the "save repository" radio button is activated while the "view" button is pressed, the currently selected repository will be stored for the active user. 

If the "load repository" radio button is activated while the "view" button is pressed, the module will load a stored repository of the user if already stored.

####  Save and load a delimiter expression
If the "save delimiter expression" radio button is activated while the "view" button is pressed, the current delimiter expression will be stored for the active user. If the user is administrator, the expression will also (parallely) be stored as adminstrators' delimiter expression.

If the "load delimiter expression" radio button is activated while the "view" button is pressed, the module will load a stored delimiter expression of the user if already stored.

If the "load delimiter expression from administrator" radio button is activated while the "view" button is pressed, the module will load a stored delimiter expression of the administrator(s) if available.

<a name="Using-rename"></a>
###  Rename a call number category
By opening the "Rename" link close to a call number category, a data fix page with a search/replace form is opened, where the name of the chosen call number can be modified.

<a name="Using-add-source"></a>
###  Add a new source to a call number category
By opening the "Add new source" link close to a call number category, a form is opened, which allows to add a new source to the chosen call number. When opening the form, a "{new}" placeholder is inserted, which should be modified by the user.

While the "{new}" placeholder should be modified, the rest of the call number, which consists of the call number category hierarchy should only be modified if the "route" or "path" of the call number category shall also be changed. If the intention is to simple add a new source to an existing call number category, only the "{new}" placeholder should be changed.

<a name="Using-finding-aid"></a>
### Generate and download a finding aid document
A [finding aid](https://en.wikipedia.org/wiki/Finding_aid) document contains detailed, indexed, and processed metadata and other information about a specific collection of source records within an archive. More simple, it is a (hiearchical) list of sources in an archive with additional metadata. 

The **benefit of a finding aid document is to provide a fast overview of the available sources for a user/visitor of an archive**. It also provides insights about the structure of the archive and the kind of sources, which can be found in the archive.

With the Repository Hierarchy module, webtrees can generate a finding aid document for a chosen repository. After selecting the Repository Hierarchy module from the list menu, a repository can be chosen and a [delimiter expression](#Using-delimiters) needs to be provided. The chosen delimiter expression will be used to generate and view a hierarchical structure for the repository.

Based on the generated repository and call number structure, a finding aid document can be generated and downloaded by **clicking** the **"Download" button** and selecting one of the options **"Finding aid as HTML"** or **"Finding aid as PDF"**. 

The generated finding aid document contains the following metadata for each of the sources:
+ Call number
+ Title
+ Author
+ Date range
+ Gedcom-ID and webtrees link (optional)

<a name="Using-EAD-xml"></a>
### Generate and download an EAD XML export
With the Repository Hierarchy module, webtrees can generate an [EAD XML](#EAD) export for a chosen repository. The export is provided in [apeEAD](#apeEAD) XML. 

The EAD XML export contains:
+ Metadata about the repository (i.e. name, address, ...)
+ Metadata about the repository structure (i.e. hierarchy, call numbers, fonds, collections, files, ...)
+ Metadata about the sources in the repository (i.e. title, author, date range, ...)

After selecting the Repository Hierarchy module from the list menu, a repository can be chosen and a [delimiter expression](#Using-delimiters) needs to be provided. The chosen delimiter expression will be used to generate and view a hierarchical structure for the repository.

Based on the generated repository and call number structure, an EAD XML export can be generated and downloaded by **clicking** the **"Download" button** and selecting the option **"EAD XML"**. 

<a name="Using-xml-settings"></a>
### Settings for EAD XML exports
In order to generate EAD XML exports, some settings need to be provided. 

The EAD XML settings can be provided by **clicking** the **"EAD XML settings" button**. A specific window will open to enter the values.

Within the EAD XML settings window, a button is available to load the settings from an administrator. Hence, if an administrator provided values for the EAD XML settings, they can be loaded and used.

The [apeEAD](#apeEAD) standard, which is used for the export, requires to provide at least the following values:
+ Finding aid title
+ Country code
+ Main agency code
+ Finding aid identifier

Additionally, the following values can be provided:
+ URL of the online finding aid
+ Publisher

The Repository Hierarchy module will provide a default proposal for the values.

The **main agency code** is an unique code identifying the archival institution maintaining the described collection; encoded according to ISO 15511 (ISIL). The main agency code is officially assigned to archives by a national authority. As a substitute value (e.g. for a private or inofficial archive), the country code and "-XXXXX" might be chosen, e.g. FR-XXXXX.

<a name="Using-export"></a>
### Export data to an external archive management system
The EAD XML exports described in the section "[Generate and download an EAD XML export](#Using-EAD-xml)" can be used to transfer data from webtrees to an external archive management system.

Some examples for archive management systems:
+ [AtoM](https://www.accesstomemory.org/)
+ [CollectiveAccess](https://collectiveaccess.org/)
+ [ArchivesSpace](https://archivesspace.org/)

The EAD export of the Repository Hierarchy module was tested with AtoM. Further details about AtoM import can be found in the [AtoM import documentation](https://www.accesstomemory.org/de/docs/2.6/user-manual/import-export/import-xml/#import-xml).

For re-importing EAD XML the same webtrees repository, the AtoM setting "Ignore matches and import as new" should be used. The re-import will generate a new (parallel) archival institution in AtoM. To add (newly imported) sources to the exiting archival institution, they can be selected from the imported structure and moved within AtoM to the available structure. Please note that AtoM does not offer features to sync and update existing records. For more details, please refer to the [AtoM documentation for XML imports](https://www.accesstomemory.org/en/docs/2.6/user-manual/import-export/import-xml#matching-critera-for-description-xml-imports).

<a name="Using-links"></a>
### Create links between webtrees and an external archive management system
When exporting webtrees data to an external archive system, linking between webtrees and the external system helps to connect the two systems and to keep redundancy at a mimimum.

The following concept was tested with the [AtoM](https://www.accesstomemory.org/) archive management system. If you are interested to interact with other archive management systems, please contact the [module author](http://www.familienforschung-hemprich.de/index.php/de/kontakt) for further clarifcation and discussion.

While exporting data from webtrees with an EAD XML export, a XML structure with URLs to each of the sources in webtrees is included. After importing the EAD XML into AtoM, the links are shown in the user interface and can be used to navigate to the related webtrees source.

![Screenshot](resources/img/screenshot_AtoM.jpg)

Within webtrees, the Repository Hierarchy module can provide links to the related AtoM records. In order to use this feature, the following steps need to be taken:
+ Settings in webtrees:
    + Open the webtrees preferences for the Repository Hierarchy module in the control panel 
    + Set the preferences for linking to external archive management tools, specifically for AtoM
    + Specify, whether to use call numbers or source titles to create AtoM REST links ("AtoM slugs")
    + Also activate the feature "Show additional source facts (REPO, REPO:CALN, REFN, NOTE) within source citations"
+ Settings in AtoM:
    + Open settings / global settings in AtoM and set the permalinks in AtoM to call numbers or source titles. It is important to use the same settings in AtoM like in webtrees. More information about the AtoM permalinks settings can be found in the [AtoM documentation](https://www.accesstomemory.org/en/docs/2.6/user-manual/administer/settings/#description-permalinks).
    + Depending on the settings in AtoM, it might also be necessary to re-genearte slugs (i.e. permalinks) in Atom. More information can bei found in the [AtoM documentation](https://www.accesstomemory.org/en/docs/2.6/admin-manual/maintenance/cli-tools/#generate-slugs).

![Screenshot](resources/img/screenshot_AtoM_link_in_webtrees.jpg)

<a name="Using-citations"></a>
### Show additional source and repository information in source citations
The Repository Hierarchy modules provides a feature to show extended information within source citations. If one of the following GEDCOM tags is available and contains content, it is shown in the user interface:
+ REPO (name of repository)
+ REPO:CALN (call number of the source in the repository)
+ REFN (user reference numbers)
+ NOTE (notes of the source)

![Screenshot](resources/img/screenshot_source_citation_in_webtrees.jpg)

In order to activate this feature, the setting "Show additional source facts (REPO, REPO:CALN, REFN, NOTE) within source citations" needs to be activated in the control panel.

Note:Additionally, it is possible to show links to AtoM (as an external archive management system) within the source citations. The details are described in the related [chapter](#Using-links).

<a name="Prefences"></a>
###  Preferences
The following preferences can be activated/deactivated by administrators in the control panel.

#### Preferences for the main Repository Hierarchy list
+ Show label before call number category.
+ Show help icon after label for delimiter expression.
+ Show help link after label for delimiter expression.
+ Use truncated categories. The call number categories will be truncated and shown without the trunk.
+ Use truncated call numbers. The call numbers will be truncated and shown without call number category.
+ Show the title of the sources.
+ Show the XREF of the sources.
+ Show the author of the sources.
+ Show the date range of the sources.
+ Allow users to load stored delimiter expressions from administrator.

#### Preferences for source citations
+ Show additional source facts (REPO, REPO:CALN, REFN, NOTE) within source citations.

#### Preferences for finding aid export
+ Include repository address within finding aid export.
+ Include links to webtrees sources within finding aid export.
+ Include table of contents within finding aid export.
+ Show links within table of contents in finding aid export (not available for PDF export).

#### Preferences for EAD XML exports
+ Allow users to load stored XML settings from administrator.

#### Preferences for linking to external archive management tools
+ Use call numbers to create AtoM REST links ("AtoM slugs")
+ Use source titles to create AtoM REST links ("AtoM slugs")
+ [AtoM](https://www.accesstomemory.org/) base ULR to be used for the generation of links to an [AtoM](https://www.accesstomemory.org/) archive management system.
+ Repositories, for which [AtoM](https://www.accesstomemory.org/) linking is used.

<a name="Background"></a>
##  Some more background about archive and library management
In archive (or library) management, archival arrangements, library classifications, finding aids, and call numbers are frequently used to:
+ define a structure for an archive
+ assign item numbers to the sources in the archive
+ provide a catalog or finding aid for the archive

In the following, some of the typical concepts are briefly described.

<a name="Arrangement"></a>
###  Archival Arrangement
[Wikipedia](https://en.wikipedia.org/wiki/Finding_aid): "Arrangement is the manner in which \[the archive] has been ordered \[...]. Hierarchical levels of arrangement are typically composed of record groups containing series, which in turn contain boxes, folders, and items."

<a name="Classification"></a>
###  Library classification
[Wikipedia](https://en.wikipedia.org/wiki/Library_classification): "A library classification is a system of knowledge distribution by which library resources are arranged and ordered systematically."

<a name="Finding_aids"></a>
###  Finding Aids
[Archive Portal Europe](https://www.archivesportaleurope.net/?show=help): "A finding aid is a structured description of archival materials per collection or fonds up to item level"

[Wikipedia](https://en.wikipedia.org/wiki/Finding_aid): "A finding aid for an archive is an organization tool, a document containing detailed, indexed, and processed metadata and other information about a specific collection of records within an archive."

<a name="Call_numbers"></a>
###  Call numbers
[Wikipedia](https://en.wikipedia.org/wiki/Library_classification): "\[...] a call number (essentially a book's address) based on the classification system in use at the particular library will be assigned to the work using the notation of the system."

<a name="Relationship"></a>
###  Relationship between Archival Arrangement and Call numbers
A lot of archives (and libraries) map the archival arrangement (or library classification) into the call numbers of the sources. 

For example, the archive might have the following arrangement:
+ Fonds
    + Record group
        + Series
            + Folder
                + Source

In this case, the call numbers might have the following structure:

**"Fonds/Record group/Series/Folder/Source"**

Therefore, the hierarchy of the archival arrangement is represented in the "route" or the "path" of the call number. 

<a name="EAD"></a>
###  EAD standard for XML export of archival records
Encoded Archival Description ([EAD](https://www.loc.gov/ead/)) is a standard for encoding descriptive information regarding archival records. The EAD standard provides a **XML export format**, which allows to describe and export the content and structure of archives and sources. It also provides data structures to describe and export finding aids.

See also: [Wikipedia](https://en.wikipedia.org/wiki/Encoded_Archival_Description)

<a name="apeEAD"></a>
###  apeEAD standard for XML export of finding aids
[apeEAD](https://www.archivesportaleurope.net/tools/for-content-providers/standards/apeead/) is a standard, which was designed and published by the [Archive Portal Europe](https://www.archivesportaleurope.net/). As a sub-set of [EAD](#EAD), apeEAD was specifically designed for encoding archival finding aids. As specification of the standard, the Archive Portal Europe provides a [table](http://apex-project.eu/images/docs/apeEAD_finding_aid_table_201210.pdf) with the used EAD XML tags and a [description and best practice guide](http://apex-project.eu/images/docs/apenet_ead_finding_aid_holdings_guide.pdf) for usind ape EAD.

The Archive Portal Europe also provides a validation tool. With the [apeEAD data preparation tool](https://github.com/ArchivesPortalEuropeFoundation/ape-dpt), EAD XML exports can be validated against the apeEAD standard. Details are described in a [manual](http://apex-project.eu/index.php/en/outcomes/tools-and-manuals/data-preparation-tool-manual).

The XML exports of the Repository Hiararchy module were developed and tested to pass the validation of the apeEAD data preparation tool.

<a name="Mapping"></a>
##  How the module maps to Gedcom and to archive management concepts
In order to manage archives and sources, Gedcom and webtrees basically provide the following data structures:
+ Repository
+ Source
+ Call number (of a source within a repository)

The following table describes how the concepts from archive and library management are mapped to Gedcom/webtrees and the Repository Hierarchy custom module:

|Archive/Library Concept|Gedcom/webtrees data structures|Repository Hierarchy Module|
|:------|:--------------|:---------------------------|
|Archive,<br>Library|Repository|Repository|
|Archival Arrangement,<br>Library Classification|-|Hierarchy of call number categories|
|Fonds,<br>Record group,<br>Series,<br>Folder|-|Call number category|
|Item,<br>file,<br>book|Source|Source|
|Call number|Call number|Call number|
|Finding aid|List of sources for a selected repository|List of sources in a hierarchy of call number categories for a selected repository|

<a name="Github"></a>
##  Github repository  
https://github.com/Jefferson49/RepositoryHierarchy
