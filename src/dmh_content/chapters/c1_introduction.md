# 1. Introduction

The purpose of the Data Management Handbook ([DMH](#dmh)) is threefold:

1.  to provide an overview of the principles for data management to be employed;

2.  to help personnel identify their roles and responsibilities for good data management; and

3.  to provide personnel with practical guidelines for carrying out good data management.

[*Data management*](#glossary-data-management) is the term used to describe the handling of data in a systematic and cost-effective manner. The data management regime should be continuously evolving, to reflect the evolving nature of data collection. Therefore this [DMH](#dmh) is a living document that will be revised and updated from time to time in order to maintain its relevance.

The [DMH](#dmh) is a strategic governing document and should be used as part of the quality framework the organisation is using.

The primary focus of this [DMH](#dmh) is on the management of [dynamic geodata](#glossary-dynamic-geodata). [Dynamic geodata](#glossary-dynamic-geodata) is weather, environment and climate-related data that changes in space and time and is thus descriptive of processes in nature. Examples are weather observations, weather forecasts, pollution (environmental toxins) in water, air and sea, information on the drift of cod eggs and salmon lice, water flow in rivers, driving conditions on the roads and the distribution of sea ice. [Dynamic geodata](#glossary-dynamic-geodata) provides important constraints for many decision-making processes and activities in society.

This introduction ([Introduction](#introduction)) lays forth the background and principles for the data management regime. Chapters 2-5 describe the implementation of the main building blocks: structuring and documenting data ([???](#structuring-and-documenting)), data services ([???](#data-services)), portals and documentation aimed at users ([???](#user-portals)), and governance issues ([???](#data-governance)). Each chapter starts with a brief statement of its purpose, followed by a description of what is implemented at present, as well as the planned developments for the short-term (&lt;2 years) and expected developments for the longer term (2-5 years).

Practical guidelines for carrying out good [data management](#glossary-data-management) are addressed in Chapter 6.

The intended audience for this DMH is any personell involved in the process of making data available for the end user. This process can be viewed as a value chain that moves from the producer of the data to the data consumer (i.e., the end user). The process is further described in [The data value chain](#value-chain) ([???](#img-value-chain)).

The handbook can be used in three ways:

1.  Read [Introduction](#introduction) to learn about the background and principles of [data management](#glossary-data-management);

2.  Read Chapters 2-5 to learn about how [data management](#glossary-data-management) is currently implemented and how it is expected to evolve in the next few years; and

3.  Read [???](#practical-guides) to find how-to’s and practical guidelines.

## 1.1 The principles of data management for dynamic geodata

Principles of standardised data documentation, publication, sharing and preservation have been formalised in the [FAIR](#glossary-fair-principles) Guiding Principles for scientific data management and stewardship \[[RD3](https://www.nature.com/articles/sdata201618)\] through a process facilitated by [FORCE11](#force11). FAIR stands for findability, accessibility, [interoperability](#glossary-interoperability) and reusability.

By following the [FAIR](#fair) principles it is easier to obtain a common approach to [data management](#glossary-data-management), or a [unified data management](#glossary-unified-data-management) model. One of the main motivations for implementing a [unified data management](#glossary-unified-data-management) is to better serve the users of the data. Primarily, this can be approached by making user needs and requirements the guide for determining what data we provide and how. For example, it will be described below how the specification of [datasets](#glossary-dataset) should be determined. By implementing the data management practices described here, it is expected that users will experience:

-   Ease of discovering, viewing and accessing [datasets](#glossary-dataset);

-   Standardised ways of accessing data, including downloading or streaming data, with reduced need for special solutions on the user side;

-   Reduced storage needs;

-   Simple and standard access to remote [datasets](#glossary-dataset) and catalogues, with own data visualisationl and analysis tools;

-   Ability to compare and combine data from internal and external sources;

-   Ability to apply common data transformations, like spatial, temporal and variables subsetting and reprojection, before downloading anything;

-   Possibility to build specialized metadata catalogues and data portals targeting a specific user community.

### 1.1.1 External data management requirements and forcing mechanisms

Any organisation that strives to implement [FAIR](#glossary-fair-principles) data management model has to relate to external forcing mechanisms concerning [data management](#glossary-data-management) at several levels. At the national level, the organisation must comply with national regulations as decided by the government. Some of these are indications of expected behaviour (e.g., [OECD](#oecd) regulations) and some are implemented through a legal framework. The Norwegian government has over time promoted free and open sharing of public data. Mechanisms for how to do this are governed by the [Geodataloven](#glossary-geodataloven) (implemented as [Geonorge](#glossary-geonorge)), which is a national implementation of the European [INSPIRE directive](#inspire) (to be amended in 2019). [INSPIRE](#inspire) defines a federated multinational [Spatial Data Infrastructure](#glossary-spatial-data-infrastructure) ([SDI](#sdi)) for the European Union, similar to [NSDI](#nsdi) in the USA or [UNSDI](#unsdi) under the United Nations. The goal is to provide a standardised access to data and provide the necessary tools to be able to work with the data in a unified manner. In short, these legal frameworks require standardised documentation (at discovery and use level; these concepts are described later) and access (through specified protocols) to the data identified.

Other external requirements and forcing mechanisms that are organisation-specific are provided in [???](#specialized-external-requirements).

### 1.1.2 The data value chain

The process of getting the data from the data producer to the consumer can be viewed as a value chain. An example of a data value chain is presented in [???](#img-value-chain). Typically, data from a wide variety of providers are used in the value chain. Traditionally, the data used have been transmitted on request from one [data centre](#glossary-data-centre) to another, and used in the specific processing chains that requested the data. The focus on reuse of data in various contexts has been missing.

![Value chain for data.](../assets/value_chain.png)

Datasets and metadata are what travels through the value chain. At the end of the [data management](#glossary-data-management) value chain are the data consumers.

### 1.1.3 Dataset

A [dataset](#glossary-dataset) is a collection of data. In the context of the [data management](#glossary-data-management) model, the storage mode of the [dataset](#glossary-dataset) is irrelevant, since access mechanisms can be decoupled from the storage layer as experienced by a data consumer. Typically, a [dataset](#glossary-dataset) represents a number of variables in time and space. A more detailed definition is provided in the [Glossary of Terms](#glossary-glossary). In order to best serve the data through [web services](#web-service), the following principles are useful for guiding the [dataset](#glossary-dataset) definition:

1.  A [dataset](#glossary-dataset) can be a collection of variables stored in, for example, a relational database or as flat files;

2.  A [dataset](#glossary-dataset) is defined as a number of spatial and/or temporal variables;

3.  A [dataset](#glossary-dataset) should be defined by the information content and not the production method;

4.  A good [dataset](#glossary-dataset) does not mix [feature types](#glossary-feature-type), i.e., trajectories and gridded data should not be present in the same [dataset](#glossary-dataset).

Point 3 implies that the output of, e.g., a numerical model may be divided into several [datasets](#glossary-dataset) that are related. This is also important in order to efficiently serve the data through [web services](#glossary-webservice). For instance, model variables defined on different vertical coordinates should be separated as [linked datasets](#glossary-linked-data), since some [OGC](#ogc) services (e.g., [WMS](#wms)) are unable to handle mixed coordinates in the same [dataset](#glossary-dataset). One important linked dataset relation is the parent-child relationship. In the numerical model example, the parent dataset would be the model simulation. This (parent) dataset encompasses all datasets created by the model simulation such as, e.g., two NetCDF-CF files (child datasets) with different information content.

Most importantly, a [dataset](#glossary-dataset) should be defined to meet the consumer needs. This means that the specification of a [dataset](#glossary-dataset) should follow not only the content guidelines just listed, but also address the consumer needs for data delivery, security and preservation.

### 1.1.4 Metadata

Metadata is a broad concept. In our [data management](#glossary-data-management) model the term "metadata" is used in several contexts, specifically the five categories that are briefly described in [table\_title](#table-metadata).

| Type | Purpose | Description | Examples |
|-------|--------|--------------|----------|
|Discovery metadata | Used to find relevant data | Discovery metadata are also called index metadata and are a digital version of the library index card. They describe who did what, where and when, how to access data and potential constraints on the data. They shall also link to further information on the data, such as site metadata. | [ISO 19115](https://metno.github.io/data-management-handbook/#ISO-19115), [GCMD](https://metno.github.io/data-management-handbook/#gcmd) |
| Use metadata | Used to understand data found | Use metadata describes the actual content of a dataset and how it is encoded. The purpose is to enable the user to understand the data without any further communication. They describe the content of variables using standardised vocabularies, units of variables, encoding of missing values, map projections, etc. | [Climate and Forecast](https://metno.github.io/data-management-handbook/#cf), [Convention](https://metno.github.io/data-management-handbook/#cf), [BUFR](https://metno.github.io/data-management-handbook/#bufr), [GRIB](https://metno.github.io/data-management-handbook/#grib) |
| Site metadata | Used to understand data found | Site metadata are used to describe the context of observational data. They describe the location of an observation, the instrumentation, procedures, etc. To a certain extent they overlap with discovery metadata, but also extend discovery metadata. Site metadata can be used for observation network design. Site metadata can be considered a type of use metadata. | [WIGOS](https://metno.github.io/data-management-handbook/#wigos), [OGC O&M](https://metno.github.io/data-management-handbook/#ogc-om) |
| Configuration metadata | Used to tune portal services for datasets intended for data consumers (e.g., WMS)| Configuration metadata are used to improve the services offered through a portal to the user community. This can, e.g., be how to best visualise a product. | |
| System metadata | Used to understand the technical structure of the data management system and track changes in it | System metadata covers, e.g., technical details of the storage system, web services, their purpose and how they interact with other components of the data management system, available and consumed storage, number of users and other KPI elements etc. | | 



The tools and facilities used to manage the information for efficient discovery and use are further described in [???](#structuring-and-documenting).

### 1.1.5 A data management model based on the FAIR principles

The [data management](#glossary-data-management) model is built upon the following principles:

-   **Standardisation** – compliance with established international standards;

-   **[Interoperability](#glossary-interoperability)** – enabling machine-to-machine interfaces including standardised documentation and encoding of data;

-   **Integrity** – ensuring that data and data access can be maintained over time, and ensuring that the consumer receives the same data at any time of request;

-   **Traceability** – documentation of the [provenance](#glossary-data-provenance) of a [dataset](#glossary-dataset), i.e., all actions taken to produce and maintain the [dataset](#glossary-dataset) and the usage of the data in downstream systems;

-   **Modularisation** – enabling replacement of one component of the system without necessitating other changes.

The model’s basic functions fall into three main categories:

1.  **Documentation of data** using [discovery](#glossary-discovery-metadata) and [use metadata](#glossary-use-metadata). The documentation identifies who, what, when, where, and how, and shall make it easy for consumers to find and understand data. This requires application of information containers and utilisation of [controlled vocabularies](#glossary-controlled-vocabulary) and [ontologies](#glossary-ontology) where textual representation is required. It also covers the topic of [data provenance](#glossary-data-provenance) which is used to describe the origin and all actions done on a [dataset](#glossary-dataset). [Data provenance](#glossary-data-provenance) is closely linked with [workflow management](#glossary-workflow-management). Furthermore, it covers the relationship between [datasets](#glossary-dataset). Application of [ontologies](#glossary-ontology) in data documentation is closely linked to the concept of [linked data](#glossary-linked-data).

2.  **Publication and sharing of data** focuses on making data accessible to consumers internally and externally. Application of standardised approaches is vital, along with cost effective solutions that are sustainable. Direct integration of data in applications for analysis through data streaming minimises the complexity and overhead in dissemination solutions. This category also covers persistent identifiers for data.

3.  **Preservation of data** includes short and long term management of data, which secures access and availability throughout the lifespan of the data. Good solutions in this area depend on expected and actual usage of the data. Preservation of data includes the concept of data life cycle, i.e., the documented flow of data from initial storage through to obsolescence and permanent archiving (or deletion) and preserving the metadata for the same data (even after deleting).

## 1.2. Human roles in data management


### 1.2.1 Context

Data is processed and interpreted to generate knowledge (e.g., about the weather) for end users. The knowledge can be presented as information in the form of actual data, illustrations, text or other forms of communication. In this context, an illustration is a representation of data, whereas data means the numerical values needed to analyse and interpret a natural process (i.e., calibrated or with calibration information; it must be possible to understand the meaning of the numerical value from the available and machine-readable information).

![Information to knowledge](../../assets/information-to-knowledge.png)

> **Definition**
>
> Data here means the numerical values needed to analyse and interpret a natural process (i.e., calibrated or with calibration information, provenance, etc.; it must be possible to understand the meaning of the numerical value from the available and machine-readable information).

Advanced users typically consume some type of data in order to process and interpret it, and produce new knowledge, e.g., in the form of a new dataset or other information. The datasets can be organised in different levels, such as the [WMO WIGOS definition for levels of data](http://codes.wmo.int/wmdr/_SourceOfObservation). Less advanced users apply information based on data (e.g., an illustration) to make decisions (e.g., clothing adapted to the forecast weather).


### 1.2.2 User definitions


We define two types of users:

1.  **Producers**: Those that create / produce data

2.  **Consumers**: Those that consume data

A consumer of one level of data is typically a producer of data at the next level. A user can both consume data and produce data, or just have one of these roles (e.g., at the start/end of the production chain).

![Information to knowledge](../../assets/user-definitions.png)

#### 1.2.2.1 Data consumers

We split between three types of data consumers: (1) advanced, (2) intermediate, and (3) simple. These are defined below.

##### 1.2.2.1.1 Advanced consumers

Advanced consumers require information in the form of data and metadata (including provenance) to gain a full understanding of what data exists and how to use it (discovery and use metadata), and to automatize the generation of derived data (new knowledge generation), verification (of information), and validation of data products.

Example questions:

-   Need all historical weather data, that can be used to model / predict the weather in the future

Specific consumers:

-   Researcher (e.g., for climate projections within the "Klima i Norge 2100" research project)

##### 1.2.2.1.2 Intermediate consumers

Intermediate consumers need enough information to find data and understand if it can answer their question(s) (discovery and use metadata). Also, they often want to cross reference a dataset with another dataset or metadata for inter-comparative verification of information.

Example questions:

-   Is this observation a record / weather extreme (coldest, warmest, wettest)?

-   What was the amount of rain in last month in a certain watershed?

Specific consumers:

-   Klimavakt (MET)

-   Developer (app, website, control systems, machine learning, etc.)

-   Energy sector (hydro, energy prices)

-   External partners

##### 1.2.2.1.3 Simple consumers

Simple consumers do not have any prior knowledge about the data. Information in the form of text or illustrations is sufficient for their decision making. They do not need to understand either data or metadata, and they are most likely looking for answers to simple questions.

Example questions:

-   Will it be raining today?

-   Can the event take place, or will the weather impeed it?

-   When should I harvest my crops?

Specific consumers:

-   Event organizer

-   Journalist

-   Farmer, or other people who work with the land like tree planters

> **Note**
>
> An advanced consumer may discover information pertaining a role as a simple consumer. Such a user may, for some reason, be interested in tracking the data in order to use it together with other data (interoperability) or to verify the information. Therefore, it is important to have provenance metadata pointing to the basic data source(s) also at the simplest information level.

#### 1.2.2.2 Data producers

A producer is an advanced consumer at one level of data that generate new information at a higher level. This new information could be in the form of actual data or simple information, such as an illustration or a text summary. It is essential that any information can be traced back to the source(s).

#### 1.2.2.3 Data Management Roles

Between the data providers and data consumers are the processes that manage and deliver the datasets (cf. [???](#img-value-chain)). A number of human roles may be defined with responsibilities that, together, ensure that these processes are carried out in accordance with the data management requirements of the organisation. The definition and filling of these roles depend heavily on the particular organisation, and each organisation must devise its own best solution.


## 1.3 Introduction to the data management at (insert organisation name here)

### 1.3.1 Background at (insert organsitation name here)


### 1.3.2 External data management requirements and forcing mechanisms specific to (insert organisation name here)

​
### 1.3.3 Data Management roles at (insert organisation name here)


## 1.4 Summary of data management requirements


The data management regime described in this DMH follows the Arctic Data centre model and shall ensure that:

1.  There are relevant metadata for all datasets, and both data and metadata are available in a form and in such a way that they can be utilised by both humans and machines

    -   There are sufficient metadata for each dataset for both discovery and use purposes

    -   Discovery metadata are indexed and can be retrieved from available services in a standard way and with standard protocols

    -   There are interfaces for discovery, visualisation and download, as well as portals for human access, that operate seamlessly across institutions

    -   The data are described in a relevant, standardised and managed vocabulary that supports machine-machine interfaces

    -   Datasets have attached a unique and permanent identifier that enables traceability

    -   Datasets have licensing that ensures free use and reuse wherever possible

    -   Datasets are available for download in a standard form according to the FAIR guiding principles and through standard protocols that are accepted and utilised in the user environment

    -   There are authentication and authorisation mechanisms that ensure access control to data with restrictions, and that are compatible with and coupled to relevant public authentication solutions (FEIDE, eduGAIN, Google, etc.)

2.  There is an organisation that provides for the management of each dataset throughout its lifetime (life cycle management)

    -   There is documentation that describes physical storage, lifetime of each dataset, degree of storage redundancy, metadata consistency methods, how dataset versioning is implemented and unique IDs to ensure traceability

    -   The organisation provides seamless access to data from distributed data centres through various portals

    -   The above and a business model at dataset level are described in a Data Management Plan (DMP)

3.  There are services or tools that provide the following functionalities on the datasets:

    -   Transformations

        -   Subsetting

        -   Slicing of gridded datasets to points, sections, profiles

        -   Reprojection

        -   Resampling

        -   Reformatting

    -   Visualisation (time series, mapping services, etc.)

    -   Aggregation

    -   Upload of new datasets (including enabling and configuring data access services)
