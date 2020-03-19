.. _ddipy

ddipy: Python package
===========================

An `Python package <https://github.com/OmicsDI/ddipy>`_ to obtain data from the `Omics Discovery Index <http://www.omicsdi.org>`_. It uses the RESTful Web Services at `OmicsDI WS <http://www.omicsdi.org/ws/>`_ for that purpose.

Installation
--------------------

we need to install `ddipy`:

.. code-block:: python
   :linenos:

    pip install ddipy

.. csv-table:: Client Documents
 :header: "Client", "Method","Result Structure","Description"
 :widths: 20,20,20,20

 "`DatasetClient`_", "search","`DataSetResult`_","Search for datasets in the resource"
 "", "get_dataset_details", "`DatasetSummary`_", "Retrieve an Specific Dataset"
 "", "get_dataset_files", "array[string]", "Retrieve the list of dataset's file using positions"
 "", "batch", "`BatchDataset`_", "Retrieve a batch of datasets"
 "", "latest", "`DataSetResult`_", "Retrieve the latest datasets in the repository"
 "", "most_accessed", "`DataSetResult`_", "Retrieve an Specific Dataset"
 "", "get_file_links", "array[string]", "Retrieve all file links for a given dataset"
 "", "get_similar", "`DataSetResult`_", "Retrieve the related datasets to one Dataset"
 "", "get_similar_by_pubmed", "array[`DatasetSummary`_]", "Retrieve all similar dataset based on pubmed id"
 "`DatabaseClient`_", "get_database_all", "array[`DatabaseDetail`_]", "Get details of all databases"
 "`SeoClient`_", "get_seo_home", "`StructuredDataGraph`_", "Retrieve JSON+LD for home page"
 "", "get_seo_search", "`StructuredData`_", "Retrieve JSON+LD for browse page"
 "", "get_seo_api", "`StructuredData`_", "Retrieve JSON+LD for api page"
 "", "get_seo_database", "`StructuredData`_", "Retrieve JSON+LD for databases page"
 "", "get_seo_dataset", "`StructuredData`_", "Retrieve JSON+LD for dataset page"
 "", "get_seo_about", "`StructuredData`_", "Retrieve JSON+LD for about page"
 "`TermClient`_", "get_term_by_pattern", "`DictWord`_", "Search dictionary Terms"
 "", "get_term_frequently_term_list", "`Term`_", "Retrieve frequently terms from the Repo"
 "`StatisticsClient`_", "get_statistics_organisms", "array[`StatRecord`_]", "Return statistics about the number of datasets per Organisms"
 "", "get_statistics_tissues", "array[`StatRecord`_]", "Return statistics about the number of datasets per Tissue"
 "", "get_statistics_omics", "array[`StatRecord`_]", "Return statistics about the number of datasets per Omics Type"
 "", "get_statistics_diseases", "array[`StatRecord`_]", "Return statistics about the number of datasets per dieases"
 "", "get_statistics_domains", "array[`DomainStats`_]", "Return statistics about the number of datasets per Repository"
 "", "get_statistics_omics_by_year", "array[`StatOmicsRecord`_]", "Return statistics about the number of datasets By Omics type on recent 5 years"


Examples
---------------

DatasetClient
>>>>>>>>>>>>>>>

This example shows how retrieve details of one dataset by using the Python package ddipy.

.. code-block:: python
   :linenos:

    from ddipy.dataset_client import DatasetClient

    if __name__ == '__main__':
        client = DatasetClient()
        res = client.get_dataset_details("pride", "PXD000210", False)


This example shows a search for 20 the datasets for cancer human.

.. code-block:: python
   :linenos:

    from ddipy.dataset_client import DatasetClient

    if __name__ == '__main__':
       client = DatasetClient()
       res = client.search("cancer human", "publication_date", "ascending")

This example shows a search for 30 the datasets for cancer human and skip first 1200 datasets

.. code-block:: python
   :linenos:

    from ddipy.dataset_client import DatasetClient

    if __name__ == '__main__':
       client = DatasetClient()
       res = client.search("cancer human", "publication_date", "ascending", 1200, 30, 20)


This example is a query to retrieve all the datasets that reported the UniProt protein P21399 as identified.

.. code-block:: python
   :linenos:


   from ddipy.dataset_client import DatasetClient

   if __name__ == '__main__':
       client = DatasetClient()
       res = client.search("UNIPROT:P21399")


This example is a query to find all the datasets where the gene ENSG00000147251 is reported as differentially expressed.

.. code-block:: python
   :linenos:

   from ddipy.dataset_client import DatasetClient

   if __name__ == '__main__':
       client = DatasetClient()
       res = client.search("ENSEMBL:ENSG00000147251")

DatabaseClient
>>>>>>>>>>>>>>>

This example is a query to retrieve all databases recorded in OmicsDI

.. code-block:: python
   :linenos:

   from ddipy.dataset_client import DatabaseClient

   if __name__ == '__main__':
       client = DatabaseClient()
       res = client.get_database_all()

SeoClient
>>>>>>>>>>>>>>>

This example is retriveing JSON+LD for dataset page

.. code-block:: python
   :linenos:

   from ddipy.dataset_client import SeoClient

   if __name__ == '__main__':
        client = SeoClient()
        res = client.get_seo_dataset("pride", "PXD000210")

This example is  retriveing JSON+LD for home page

.. code-block:: python
   :linenos:

   from ddipy.dataset_client import SeoClient

   if __name__ == '__main__':
        client = SeoClient()
        res = client.get_seo_home()

StatisticsClient
>>>>>>>>>>>>>>>>

This example is a query for statistics about the number of datasets per Tissue

.. code-block:: python
   :linenos:

   from ddipy.dataset_client import StatisticsClient

   if __name__ == '__main__':
        client = StatisticsClient()
        res = client.get_statistics_tissues(20)

This example is a query for statistics about the number of datasets per dieases

.. code-block:: python
   :linenos:

   from ddipy.dataset_client import StatisticsClient

   if __name__ == '__main__':
        client = StatisticsClient()
        res = client.get_statistics_diseases(20)

TermClient
>>>>>>>>>>>>>>>

This example for searching dictionary terms

.. code-block:: python
   :linenos:

   from ddipy.dataset_client import TermClient

   if __name__ == '__main__':
        client = TermClient()
        res = client.get_term_by_pattern("hom", 10)

This example for retrieving frequently terms from the repo

.. code-block:: python
   :linenos:

   from ddipy.dataset_client import TermClient

   if __name__ == '__main__':
        client = TermClient()
        res = client.get_term_by_pattern("pride", "description", 20)



Structure
---------------


DataSetResult
>>>>>>>>>>>>>>>>

.. csv-table:: DataSetResult Structure
 :header: "Name", "Type"
 :widths: 20,20

 "datasets", "array[`DatasetSummary`_]"
 "facets", "array[`Facet`_]"
 "count", "integer"

DatasetSummary
>>>>>>>>>>>>>>>>

.. csv-table:: DatasetSummary Structure
 :header: "Name", "Type"
 :widths: 20,20

 "accession", "string"
 "database", "string"
 "title", "string"
 "description", "string"
 "dates", "`Date`_"
 "scores", "`Score`_"
 "keywords", "array[string]"
 "omics_type", "array[string]"
 "organisms", "array[`Organism`_]"
 "cross_references", "any"
 "files", "array[string]"
 "additional","any"

Date
>>>>>>>>>>>>>>>>

.. csv-table:: Date Structure
 :header: "Name", "Type"
 :widths: 20,20

 "publication", "string"
 "submission", "string"
 "update", "string"

Score
>>>>>>>>>>>>>>>>

.. csv-table:: Score Structure
 :header: "Name", "Type"
 :widths: 20,20

 "citationCount", "integer"
 "reanalysisCount", "integer"
 "searchCount", "integer"
 "viewCount", "integer"
 "connectionsCount", "integer"
 "downloadCount", "integer"

Organism
>>>>>>>>>>>>>>>>

.. csv-table:: Organism Structure
 :header: "Name", "Type"
 :widths: 20,20

 "acc", "string"
 "name", "string"

Facet
>>>>>>>>>>>>>>>>

.. csv-table:: Facet Structure
 :header: "Name", "Type"
 :widths: 20,20

 "facet_values", "array[`FacetValue`_]"
 "label", "string"
 "total", "integer"
 "id", "string"


FacetValue
>>>>>>>>>>>>>>>>

.. csv-table:: FacetValue Structure
 :header: "Name", "Type"
 :widths: 20,20

 "label", "string"
 "count", "string"
 "value", "string"


BatchDataset
>>>>>>>>>>>>>>>>

.. csv-table:: BatchDataset Structure
 :header: "Name", "Type"
 :widths: 20,20

 "failure","array[`Failure`_]"
 "datasets","array[`DatasetSummary`_]"

Failure
>>>>>>>>>>>>>>>>

.. csv-table:: Failure Structure
 :header: "Name", "Type"
 :widths: 20,20

 "database", "string"
 "accession", "string"
 "name", "string"
 "source_url", "string"


DatabaseDetail
>>>>>>>>>>>>>>>>

.. csv-table:: DatabaseDetail Structure
 :header: "Name", "Type"
 :widths: 20,20

 "repository", "string"
 "orcid_name", "string"
 "url_template", "string"
 "accession_prefix", "array[string]"
 "title", "string"
 "img_alt", "string"
 "source_url", "string"
 "description", "string"
 "domain", "string"
 "image", "array[byte]"
 "icon", "string"
 "source", "string"
 "database_name", "string"


DictWord
>>>>>>>>>>>>>>>>

.. csv-table:: DictWord Structure
 :header: "Name", "Type"
 :widths: 20,20

 "total_count", "integer"
 "items", "array[`Item`_]"


Item
>>>>>>>>>>>>>>>>

.. csv-table:: Item Structure
 :header: "Name", "Type"
 :widths: 20,20

 "name", "string"


Term
>>>>>>>>>>>>>>>>

.. csv-table:: Term Structure
 :header: "Name", "Type"
 :widths: 20,20

 "frequent", "string"
 "label", "string"


StructuredDataGraph
>>>>>>>>>>>>>>>>

.. csv-table:: StructuredDataGraph Structure
 :header: "Name", "Type"
 :widths: 20,20

 "graph", "array[`StructuredData`_]"

StructuredData
>>>>>>>>>>>>>>>>

.. csv-table:: StructuredData Structure
 :header: "Name", "Type"
 :widths: 20,20

 "logo", "string"
 "alternateName", "string"
 "potentialAction", "`StructuredDataAction`_"
 "variableMeasured", "string"
 "sameAs", "string"
 "creator", "array[`StructuredDataAuthor`_]"
 "citation", "`StructuredDataCitation`_"
 "email", "string"
 "keywords", "string"
 "primaryImageOfPage", "`StructuredDataImage`_"
 "description", "string"
 "image", "string"
 "name", "string"
 "context", "string"
 "type", "string"
 "url", "string"


StructuredDataAction
>>>>>>>>>>>>>>>>

.. csv-table:: StructuredDataAction Structure
 :header: "Name", "Type"
 :widths: 20,20

 "query_input", "string"
 "type","string"
 "target","string"

StructuredDataAuthor
>>>>>>>>>>>>>>>>

.. csv-table:: StructuredDataAuthor Structure
 :header: "Name", "Type"
 :widths: 20,20

 "name", "string"
 "type", "string"

StructuredDataCitation
>>>>>>>>>>>>>>>>

.. csv-table:: StructuredDataCitation Structure
 :header: "Name", "Type"
 :widths: 20,20

 "author", "`StructuredDataAuthor`_"
 "publisher", "`StructuredDataAuthor`_"
 "name", "string"
 "type", "string"
 "url", "string"

StructuredDataImage
>>>>>>>>>>>>>>>>

.. csv-table:: StructuredDataImage Structure
 :header: "Name", "Type"
 :widths: 20,20

 "author", "string"
 "contentUrl", "string"
 "contentLocation", "string"
 "type", "string"


StatRecord
>>>>>>>>>>>>>>>>

.. csv-table:: StatRecord Structure
 :header: "Name", "Type"
 :widths: 20,20

 "label", "string"
 "name", "string"
 "value", "string"
 "id", "string"


DomainStats
>>>>>>>>>>>>>>>>

.. csv-table:: DomainStats Structure
 :header: "Name", "Type"
 :widths: 20,20

 "domain", "`StatRecord`_"
 "subdomains", "array[`DomainStats`_]"


StatOmicsRecord
>>>>>>>>>>>>>>>>

.. csv-table:: StatOmicsRecord Structure
 :header: "Name", "Type"
 :widths: 20,20

 "proteomics", "string"
 "transcriptomics", "string"
 "genomics", "string"
 "metabolomics", "string"
 "year", "string"