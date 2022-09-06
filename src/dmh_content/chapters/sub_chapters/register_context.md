How to add NetCDF-CF data to thredds
====================================

This section should contain institution specific information about how to add netcdf-cf files to thredds.

How to register your data in the catalog service
================================================

In order to make a dataset findable, a dataset must be registered in a searchable catalog with appropriate metadata. The (meta)data catalog is indexed and exposed through [CSW](https://en.wikipedia.org/wiki/Catalogue_Service_for_the_Web).

The following needs to be done:

1.  Generate an MMD xml file from your NetCDF-CF file (see [Generation of MMD xml file from NetCDF-CF](#local-mmd-xml-generation))

2.  Test your mmd xml metadata file (see [Test the MMD xml file](#test-mmd-file))

3.  Push the MMD xml file to the discovery metadata catalog (see [Push the MMD xml file to the discovery metadata catalog](#push-mmd-file))

Generation of MMD xml file from NetCDF-CF
-----------------------------------------

Clone the [py-mmd-tools](https://github.com/metno/py-mmd-tools.git) repo and make a local installation with eg `pip install .`. This should bring in all needed dependencies (we recommend to use a virtual environment).

Then, generate your mmd xml file as follows:

cd script ./nc2mmd.py -i &lt;your netcdf file&gt; -o &lt;your xml output directory&gt;&lt;/programlisting&gt;

See `./nc2mmd.py --help` for documentation and extra options.

You will find Extensible Stylesheet Language Transformations (XSLT) documents in the [MMD](https://github.com/metno/mmd.git) repository. These can be used to translate the metadata documents from MMD to other vocabularies, such as ISO19115:

./bin/convert\_from\_mmd -i &lt;your mmd xml file&gt; -f iso -o &lt;your iso output file name&gt;&lt;/programlisting&gt;

Note that the discovery metadata catalog ingestion tool will take care of translations from MMD, so you don’t need to worry about that unless you have special interest in it.

Test the MMD xml file
---------------------

Install the [dmci app](https://github.com/metno/discovery-metadata-catalog-ingestor), and run the usage example locally. This will return an error message if anything is wrong with your MMD file.

Push the MMD xml file to the discovery metadata catalog
-------------------------------------------------------

For development and verification purposes:

curl --data-binary "@&lt;PATH\_TO\_MMD\_FILE&gt;" https://dmci-\*.s-enda.k8s.met.no/v1/insert&lt;/programlisting&gt;

where `*` should be either dev or staging.

For production (the official catalog):

curl --data-binary "@&lt;PATH\_TO\_MMD\_FILE&gt;" https://dmci.s-enda.k8s.met.no/v1/insert&lt;/programlisting&gt;
