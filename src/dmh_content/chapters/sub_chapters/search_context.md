Searching data in the Catalog Service for the Web (CSW) interface
=================================================================

Using OpenSearch
----------------

### Local test machines

The [`vagrant-s-enda`](https://github.com/metno/vagrant-s-enda) environment found at [vagrant-s-enda](https://github.com/metno/vagrant-s-enda) provides OpenSearch support through [PyCSW](https://github.com/geopython/pycsw). To test OpenSearch via the browser, start the `vagrant-s-enda` vm (`vagrant up`) and go to the following address:

-   `http://10.10.10.10/pycsw/csw.py?mode=opensearch&service=CSW&version=2.0.2&request=GetCapabilities`

### Online catalog

For searching the online metadata catalog, the base url (`http://10.10.10.10/`) must be replaced by `https://csw.s-enda.k8s.met.no/`:

-   `http://csw.s-enda.k8s.met.no/?mode=opensearch&service=CSW&version=2.0.2&request=GetRecords&elementsetname=full&typenames=csw:Record&resulttype=results`

### OpenSearch examples

To find all datasets in the catalog:

-   `https://csw.s-enda.k8s.met.no/?mode=opensearch&service=CSW&version=2.0.2&request=GetRecords&elementsetname=full&typenames=csw:Record&resulttype=results`

Or datasets within a given time span:

-   `http://csw.s-enda.k8s.met.no/?mode=opensearch&service=CSW&version=2.0.2&request=GetRecords&elementsetname=full&typenames=csw:Record&resulttype=results&time=2000-01-01/2020-09-01`

Or datasets within a geographical domain (defined as a box with parameters min\_longitude, min\_latitude, max\_longitude, max\_latitude):

-   `https://csw.s-enda.k8s.met.no/?mode=opensearch&service=CSW&version=2.0.2&request=GetRecords&elementsetname=full&typenames=csw:Record&resulttype=results&bbox=0,40,10,60`

Or, datasets from any of the Sentinel satellites:

-   `https://csw.s-enda.k8s.met.no/?mode=opensearch&service=CSW&version=2.0.2&request=GetRecords&elementsetname=full&typenames=csw:Record&resulttype=results&q=sentinel`

Advanced geographical search
----------------------------

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

Web portals
-----------

**GeoNorge.no**

**TODO:** describe how to search in geonorge, possibly with screenshots

QGIS
----

MET Norway’s S-ENDA CSW catalog service is available at `https://csw.s-enda.k8s.met.no`. This can be used from QGIS as follows:

1.  Select `Web > MetaSearch > MetaSearch` menu item

2.  Select `Services > New`

3.  Type, e.g., `csw.s-enda.k8s.met.no` for the name

4.  Type `https://csw.s-enda.k8s.met.no` for the URL

Under the `Search` tab, you can then add search parameters, click `Search`, and get a list of available datasets.
