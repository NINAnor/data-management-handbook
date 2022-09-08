# 6. Practical Guides


This chapter includes how-to’s and other practical guidance for data producers.

## 6.1 Create a Data Management Plan (DMP)
-----------------------------------

The funding agency of your project will usually provide requirements, guidelines or a template for the DMP. If this is not the case or for datasets that are not part of a project use the template provided by your institution or the template based on the [recommendations by Science Europe](https://www.forskningsradet.no/contentassets/e4cd6d2c23cf49d4989bb10c5eea087a/se_rdm_practical_guide_final.pdf).

### 6.1.1 Using easyDMP

1.  Log in to [easyDMP](https://easydmp.sigma2.no/login/), use **Dataporten** if your institution supports that, otherwise pick one of the other login methods.

2.  Click on **+ Create a new plan** and pick a template

3.  By using the **Summary** button from page two and on, you can get an overview of all the questions.

### 6.1.2 Publishing the plan

Currently you can use the *export* function in easyDMP to download an HTML or PDF version of the DMP and use it further. This might change if "[Hosted DMP](https://github.com/hmpf/easydmp/issues/226)" gets implemented.

## 6.2 Submitting data as NetCDF-CF



### 6.2.1 Workflow

1.  Define your dataset (see [dataset](#glossary-dataset) and [???](#dataset))

2.  Create a NetCDF-CF file (see [Creating NetCDF-CF files](#data-as-netcdf-cf))

    -   Add discovery metadata as global attributes (see [global attibutes section](#acdd-elements))

    -   Add variables and [use metadata](#glossary-use-metadata) following the [CF conventions](https://cfconventions.org/)

3.  Store the NetCDF-CF file in a suitable location, and distribute it via thredds or another dap server (see, e.g., [How to add NetCDF-CF data to thredds](#add-to-thredds))

4.  Register your dataset in a searchable catalog (see [How to register your data in the catalog service](#register-dataset-in-catalog))

    -   Test that your dataset contains the necessary discovery metadata and create an MMD xml file (see [Generation of MMD xml file from NetCDF-CF](#local-mmd-xml-generation))

    -   Test the MMD xml file (see [Test the MMD xml file](#test-mmd-file))

    -   Push the MMD xml file to the discovery metadata catalog (see [Push the MMD xml file to the discovery metadata catalog](#push-mmd-file))

### 6.2.2 Creating NetCDF-CF files

By documenting and formatting your data using [NetCDF](#netcdf) following the [CF conventions](https://cfconventions.org/) and the [Attribute Convention for Data Discovery (ACDD)](https://wiki.esipfed.org/Attribute_Convention_for_Data_Discovery_1-3), [MMD](#mmd) files can be automatically generated from the [NetCDF](#netcdf) files. The [CF conventions](#cf) is a controlled vocabulary providing a definitive description of what the data in each variable represents, and the spatial and temporal properties of the data. The [ACDD](#acdd) vocabulary describes attributes recommended for describing a [NetCDF](#netcdf) dataset to data discovery systems. See, e.g., [netCDF4-python docs](https://unidata.github.io/netcdf4-python/), or [xarray docs](http://xarray.pydata.org/en/stable/user-guide/io.html) for documentation about how to create netCDF files.

The ACDD recommendations should be followed in order to properly document your netCDF-CF files. The below tables summarize required and recommended ACDD and some additional attributes that are needed to properly populate a discovery metadata catalog which fulfills the requirements of international standards (e.g., GCMD/DIF, the INSPIRE and WMO profiles of ISO19115, etc.).

#### 6.2.2.1 Notes

**Keywords** describe the content of your dataset following a given vocabulary. You may use any vocabularies to define your keywords, but a link to the keyword definitions should be provided in the \`\`keywords\_vocabulary\`\` attribute. This attribute provides information about the vocabulary defining the keywords used in the \`\`keywords\`\` attribute. Example:

:keywords\_vocabulary = "GCMDSK:GCMD Science Keywords:https://gcmd.earthdata.nasa.gov/kms/concepts/concept\_scheme/sciencekeywords, GEMET:INSPIRE Themes:http://inspire.ec.europa.eu/theme, NORTHEMES:GeoNorge Themes:https://register.geonorge.no/metadata-kodelister/nasjonal-temainndeling" ;

Note that the **GCMDSK**, **GEMET** and **NORTHEMES** vocabularies are required for indexing in [S-ENDA](https://adc.met.no/) and [Geonorge](https://www.geonorge.no/en/). You may find appropriate keywords at the following links:

-   [GCMDSK](https://gcmd.earthdata.nasa.gov/kms/concepts/concept_scheme/sciencekeywords)

-   [GEMET](http://inspire.ec.europa.eu/theme)

-   [NORTHEMES](https://register.geonorge.no/metadata-kodelister/nasjonal-temainndeling)

The keywords should be provided by the \`\`keywords\`\` attribute as a comma separated list with a short name defining the vocabulary used, followed by the actual keyword, i.e., \`\`short\_name:keyword\`\`. Example:

:keywords = "GCMDSK:Earth Science &gt; Atmosphere &gt; Atmospheric radiation, GEMET:Meteorological geographical features, GEMET:Atmospheric conditions, NORTHEMES:Weather and climate" ;

See <https://adc.met.no/node/96> for more information about how to define the ACDD keywords.

A data **license** provides information about any restrictions on the use of the dataset. To support a linked data approach, the \`\`license\`\` element should be supported by a \`\`license\_resource\`\` element, providing a link to the license definition. Example:

:license = "CC-BY-4.0" ;  
:license\_resource = "http://spdx.org/licenses/CC-BY-4.0" ;

#### 6.2.2.2 List of Attributes

This section provides lists of ACDD elements that are required and recommended, as well as some extra elements that are needed to fully support our data management needs. The right columns of these tables provide the [MET Norway Metadata Specification (MMD)](https://htmlpreview.github.io/?https://github.com/metno/mmd/blob/master/doc/mmd-specification.html) fields that map to the ACDD (and our extension to ACDD) elements. Please refer to [MMD](https://htmlpreview.github.io/?https://github.com/metno/mmd/blob/master/doc/mmd-specification.html) for definitions of these elements, as well as controlled vocabularies that should be used. Note that the below tables are automatically generated - check <https://github.com/metno/py-mmd-tools/blob/master/py_mmd_tools/mmd_elements.yaml> if anything is unclear.

In order to check your netCDF-CF files, and to create MMD xml files, you can use the nc2mmd.py script in the [py-mmd-tools](https://github.com/metno/py-mmd-tools) Python package.

The following ACDD elements are required:

| ACDD Attribute | MMD equivalent | Comment |
|----------------|----------------|----------|
| id | metadata_identifier | Required, and should be UUID. No repetition allowed. |
| naming_authority | metadata_identifier | Required. We recommend using reverse-DNS naming. No repetition allowed. |
| date_created | last_metadata_update>update>datetime | Format as ISO8601. |
| title | title>title | Use ACDD extension "title_no" for Norwegian translation. | 
| summary | abstract>abstract | Use ACDD extension "summary_no" for Norwegian translation. |
| time_coverage_start | temporal_extent>start_date | Comma separated list. | 
| geospatial_lat_max | geographic_extent>rectangle>north | No repetition allowed. | 
| geospatial_lat_min | geographic_extent>rectangle>south | No repetition allowed. | 
| geospatial_lon_max | geographic_extent>rectangle>east | No repetition allowed. |
| geospatial_lon_min | geographic_extent>rectangle>west | No repetition allowed. | 
| keywords | keywords>keyword | Comma separated list. | 
| keywords_vocabulary | keywords>vocabulary | Comma separated list. | 


The following ACDD elements are (highly) recommended:


| ACDD Attribute | Default| MMD equivalent | Comment |
|----------------|---------|---------------|---------|
| date_metadata_modified | | last_metadata_update>update>datetime | Format as ISO8601. Comma separated list if more than once. | 
| time_coverage_end | | temporal_extent>end_date | Comma separated list. | 
| geospatial_bounds | | geographic_extent>polygon | No repetition allowed. | 
| processing_level | | operational_status | No repetition allowed. See the MMD docs for valid keywords. | 
| license | | use_constraint>identifier | No repetition allowed. | 
| creator_role | Investigator | personnel>role | Comma separated list. | 
| contributor_role | Investigator | personnel>role | Comma separated list. | 
| creator_name | Not available | personnel>name | Comma separated list. | 
| contributor_name | Not available | personnel>name | Comma separated list. | 
| creator_email | Not available | personnel>email | Comma separated list. | 
| creator_institution | Not available | personnel>organisation | Comma separated list. | 
| institution |  | data_center>data_center_name>long_name | Comma separated list. | 
| publisher_url | | data_center>data_center_url | Comma separated list. | 
| project |  | project>long_name | Semicolon separated list. | 
| platform | | platform>long_name | Comma separated list. | 
| platform_vocabulary | | platform>resource | Comma separated list. | 
| instrument | | platform>instrument>long_name | Comma separated list. | 
| instrument_vocabulary | | platform>instrument>resource | Comma separated list.| 
| source | | activity_type | Semicolon separated list. | 
| creator_name | | dataset_citation>author | Comma separated list. | 
| date_created | | dataset_citation>publication_date | Comma separated list. | 
| title | | dataset_citation>title | | 
| publisher_name | | dataset_citation>publisher | Comma separated list. | 
| metadata_link | | dataset_citation>url | Comma separated list. | 
| references | | dataset_citation>other | Comma separated list. | 


The following elements are ACDD extensions that are needed to improve (meta)data interoperability. Please refer to the documentation of [MMD](https://htmlpreview.github.io/?https://github.com/metno/mmd/blob/master/doc/mmd-specification.html) for more details:

| Necessary non-ACDD Attribute | Default | MMD equivalent | Comment | 
|------------------------------|---------|-----------------|---------|
| spatial_representation | | spatial_representation | No repetition allowed. | 
| alternate_identifier | | alternate_identifier>alternate_identifier | Alternative identifier for the dataset (but not DOI). Comma separated list. | 
| alternate_identifier_type | | alternate_identifier>type | Identification of the type of identifier used. Comma separated list. | 
| date_metadata_modified_type | | last_metadata_update>update>type | E.g., major or minor modification. Comma separated list. | 
| date_created_type | Created | last_metadata_update>update>type | | 
| title_no | | title>title | Used for Norwegian version of the title. | 
| title_lang | en | title>lang | ISO language code. | 
| summary_no | | abstract>abstract | Used for Norwegian version of the abstract. | 
| summary_lang | en | abstract>lang | ISO language code. | 
| dataset_production_status | Complete | dataset_production_status | No repetition allowed. | 
| access_constraint | | access_constraint | No repetition allowed. | 
| license_resource | | use_constraint>resource | No repetition allowed. | 
| contributor_email | Not available | personnel>email | Comma separated list. | 
| contributor_institution | | personnel>organisation | | 
| contributor_organisation | | personnel>organisation | | 
| institution_short_name | | data_center>data_center_name>short_name | Comma separated list. | 
| related_dataset_id | | related_dataset>related_dataset | Comma separated list. | 
| related_dataset_relation_type | | related_dataset>relation_type | Comma separated list. | 
| iso_topic_category | | iso_topic_category | Comma separated list. | 
| project_short_name | | project>short_name | Semicolon separated list. | 
| quality_control | | quality_control | No repetition allowed. | 
| doi | | dataset_citation>doi | | 
	

### 6.2.3 How to add NetCDF-CF data to thredds

This section should contain institution specific information about how to add netcdf-cf files to thredds.

### 6.2.4 How to register your data in the catalog service

In order to make a dataset findable, a dataset must be registered in a searchable catalog with appropriate metadata. The (meta)data catalog is indexed and exposed through [CSW](https://en.wikipedia.org/wiki/Catalogue_Service_for_the_Web).

The following needs to be done:

1.  Generate an MMD xml file from your NetCDF-CF file (see [Generation of MMD xml file from NetCDF-CF](#local-mmd-xml-generation))

2.  Test your mmd xml metadata file (see [Test the MMD xml file](#test-mmd-file))

3.  Push the MMD xml file to the discovery metadata catalog (see [Push the MMD xml file to the discovery metadata catalog](#push-mmd-file))

#### 6.2.4.1 Generation of MMD xml file from NetCDF-CF

Clone the [py-mmd-tools](https://github.com/metno/py-mmd-tools.git) repo and make a local installation with eg `pip install .`. This should bring in all needed dependencies (we recommend to use a virtual environment).

Then, generate your mmd xml file as follows:

cd script ./nc2mmd.py -i &lt;your netcdf file&gt; -o &lt;your xml output directory&gt;&lt;/programlisting&gt;

See `./nc2mmd.py --help` for documentation and extra options.

You will find Extensible Stylesheet Language Transformations (XSLT) documents in the [MMD](https://github.com/metno/mmd.git) repository. These can be used to translate the metadata documents from MMD to other vocabularies, such as ISO19115:

./bin/convert\_from\_mmd -i &lt;your mmd xml file&gt; -f iso -o &lt;your iso output file name&gt;&lt;/programlisting&gt;

Note that the discovery metadata catalog ingestion tool will take care of translations from MMD, so you don’t need to worry about that unless you have special interest in it.

#### 6.2.4.2 Test the MMD xml file

Install the [dmci app](https://github.com/metno/discovery-metadata-catalog-ingestor), and run the usage example locally. This will return an error message if anything is wrong with your MMD file.

#### 6.2.4.3 Push the MMD xml file to the discovery metadata catalog

For development and verification purposes:

curl --data-binary "@&lt;PATH\_TO\_MMD\_FILE&gt;" https://dmci-\*.s-enda.k8s.met.no/v1/insert&lt;/programlisting&gt;

where `*` should be either dev or staging.

For production (the official catalog):

curl --data-binary "@&lt;PATH\_TO\_MMD\_FILE&gt;" https://dmci.s-enda.k8s.met.no/v1/insert&lt;/programlisting&gt;


## 6.3 Searching data in the Catalog Service for the Web (CSW) interface

### 6.3.1 Using OpenSearch

#### 6.3.1.1 Local test machines

The [`vagrant-s-enda`](https://github.com/metno/vagrant-s-enda) environment found at [vagrant-s-enda](https://github.com/metno/vagrant-s-enda) provides OpenSearch support through [PyCSW](https://github.com/geopython/pycsw). To test OpenSearch via the browser, start the `vagrant-s-enda` vm (`vagrant up`) and go to the following address:

-   `http://10.10.10.10/pycsw/csw.py?mode=opensearch&service=CSW&version=2.0.2&request=GetCapabilities`

#### 6.3.1.2 Online catalog

For searching the online metadata catalog, the base url (`http://10.10.10.10/`) must be replaced by `https://csw.s-enda.k8s.met.no/`:

-   `http://csw.s-enda.k8s.met.no/?mode=opensearch&service=CSW&version=2.0.2&request=GetRecords&elementsetname=full&typenames=csw:Record&resulttype=results`

#### 6.3.1.3 OpenSearch examples

To find all datasets in the catalog:

-   `https://csw.s-enda.k8s.met.no/?mode=opensearch&service=CSW&version=2.0.2&request=GetRecords&elementsetname=full&typenames=csw:Record&resulttype=results`

Or datasets within a given time span:

-   `http://csw.s-enda.k8s.met.no/?mode=opensearch&service=CSW&version=2.0.2&request=GetRecords&elementsetname=full&typenames=csw:Record&resulttype=results&time=2000-01-01/2020-09-01`

Or datasets within a geographical domain (defined as a box with parameters min\_longitude, min\_latitude, max\_longitude, max\_latitude):

-   `https://csw.s-enda.k8s.met.no/?mode=opensearch&service=CSW&version=2.0.2&request=GetRecords&elementsetname=full&typenames=csw:Record&resulttype=results&bbox=0,40,10,60`

Or, datasets from any of the Sentinel satellites:

-   `https://csw.s-enda.k8s.met.no/?mode=opensearch&service=CSW&version=2.0.2&request=GetRecords&elementsetname=full&typenames=csw:Record&resulttype=results&q=sentinel`

### 6.3.2 Advanced geographical search

PyCSW opensearch only supports geographical searches querying for a box. For more advanced geographical searches, one must write specific XML files. For example:

-   To find all datasets containing a point:

<!-- -->

    <?xml version="1.0" encoding="ISO-8859-1" standalone="no"?>
    <csw:GetRecords
        xmlns:csw="http://www.opengis.net/cat/csw/2.0.2"
        xmlns:ogc="http://www.opengis.net/ogc"
        xmlns:gml="http://www.opengis.net/gml"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        service="CSW"
        version="2.0.2"
        resultType="results"
        maxRecords="10"
        outputFormat="application/xml"
        outputSchema="http://www.opengis.net/cat/csw/2.0.2"
        xsi:schemaLocation="http://www.opengis.net/cat/csw/2.0.2 http://schemas.opengis.net/csw/2.0.2/CSW-discovery.xsd" >
      <csw:Query typeNames="csw:Record">
        <csw:ElementSetName>full</csw:ElementSetName>
        <csw:Constraint version="1.1.0">
          <ogc:Filter>
            <ogc:Contains>
              <ogc:PropertyName>ows:BoundingBox</ogc:PropertyName>
              <gml:Point>
                <gml:pos srsDimension="2">59.0 4.0</gml:pos>
              </gml:Point>
            </ogc:Contains>
          </ogc:Filter>
        </csw:Constraint>
      </csw:Query>
    </csw:GetRecords>

-   To find all datasets intersecting a polygon:

<!-- -->

    <?xml version="1.0" encoding="ISO-8859-1" standalone="no"?>
    <csw:GetRecords
        xmlns:csw="http://www.opengis.net/cat/csw/2.0.2"
        xmlns:gml="http://www.opengis.net/gml"
        xmlns:ogc="http://www.opengis.net/ogc"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        service="CSW"
        version="2.0.2"
        resultType="results"
        maxRecords="10"
        outputFormat="application/xml"
        outputSchema="http://www.opengis.net/cat/csw/2.0.2"
        xsi:schemaLocation="http://www.opengis.net/cat/csw/2.0.2 http://schemas.opengis.net/csw/2.0.2/CSW-discovery.xsd" >
      <csw:Query typeNames="csw:Record">
        <csw:ElementSetName>full</csw:ElementSetName>
        <csw:Constraint version="1.1.0">
          <ogc:Filter>
            <ogc:Intersects>
              <ogc:PropertyName>ows:BoundingBox</ogc:PropertyName>
              <gml:Polygon>
                <gml:exterior>
                  <gml:LinearRing>
                    <gml:posList>
                      47.00 -5.00 55.00 -5.00 55.00 20.00 47.00 20.00 47.00 -5.00
                    </gml:posList>
                  </gml:LinearRing>
                </gml:exterior>
              </gml:Polygon>
            </ogc:Intersects>
          </ogc:Filter>
        </csw:Constraint>
      </csw:Query>
    </csw:GetRecords>

-   Then, you can query the CSW endpoint with, e.g., python:

<!-- -->

    import requests
    requests.post('https://csw.s-enda.k8s.met.no', data=open(my_xml_request).read()).text

### 6.3.3 Web portals

**GeoNorge.no**

**TODO:** describe how to search in geonorge, possibly with screenshots

### 6.3.4 QGIS

MET Norway’s S-ENDA CSW catalog service is available at `https://csw.s-enda.k8s.met.no`. This can be used from QGIS as follows:

1.  Select `Web > MetaSearch > MetaSearch` menu item

2.  Select `Services > New`

3.  Type, e.g., `csw.s-enda.k8s.met.no` for the name

4.  Type `https://csw.s-enda.k8s.met.no` for the URL

Under the `Search` tab, you can then add search parameters, click `Search`, and get a list of available datasets.
