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
 :header: "Client", "Method", "Description"
 :widths: 20,20,50

 "DatasetClient", "search", "Search for datasets in the resource"
 "", "get_dataset_details", "Retrieve an Specific Dataset"
 "", "get_dataset_files", "Retrieve the list of dataset's file using positions"
 "", "batch", "Retrieve a batch of datasets"
 "", "latest", "Retrieve the latest datasets in the repository"
 "", "most_accessed", "Retrieve an Specific Dataset"
 "", "get_file_links", "Retrieve all file links for a given dataset"
 "", "get_similar", "Retrieve the related datasets to one Dataset"
 "", "get_similar_by_pubmed", "Retrieve all similar dataset based on pubmed id"
 "DatabaseClient", "get_database_all", "Get details of all databases"
 "SeoClient", "get_seo_home", "Retrieve JSON+LD for home page"
 "", "get_seo_search", "Retrieve JSON+LD for browse page"
 "", "get_seo_api", "Retrieve JSON+LD for api page"
 "", "get_seo_database", "Retrieve JSON+LD for databases page"
 "", "get_seo_dataset", "Retrieve JSON+LD for dataset page"
 "", "get_seo_about", "Retrieve JSON+LD for about page"
 "TermClient", "get_term_by_pattern", "Search dictionary Terms"
 "", "get_term_frequently_term_list", "Retrieve frequently terms from the Repo"
 "StatisticsClient", "get_statistics_organisms", "Return statistics about the number of datasets per Organisms"
 "", "get_statistics_tissues", "Return statistics about the number of datasets per Tissue"
 "", "get_statistics_omics", "Return statistics about the number of datasets per Omics Type"
 "", "get_statistics_diseases", "Return statistics about the number of datasets per dieases"
 "", "get_statistics_domains", "Return statistics about the number of datasets per Repository"
 "", "get_statistics_omics_by_year", "Return statistics about the number of datasets By Omics type on recent 5 years"


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
        dataset = client.get_dataset_details("pride", "PXD000210", False)


This example shows a search for all the datasets for human.

.. code-block:: python
   :linenos:

    from ddipy.dataset_client import DatasetClient

    if __name__ == '__main__':
       client = DatasetClient()
       res = client.search("human")


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



The normal result of any client should be an original response from [requests](https://pypi.org/project/requests/).
