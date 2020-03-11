.. _ddipy


ddipy: A Python package to interact with OmicsDI Restful API
===========================

An `Python package <https://github.com/OmicsDI/ddipy>`_ to obtain data from the `Omics Discovery Index <http://www.omicsdi.org>`_. It uses the RESTful Web Services at `OmicsDI WS <http://www.omicsdi.org/ws/>`_ for that purpose.

Installation
--------------------

we need to install `ddipy`:

.. code-block:: python
   :linenos:

    pip install ddipy

Examples
---------------

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


The normal result of any client should be an original response from [requests](https://pypi.org/project/requests/).
when missing some of required parameters, the result shoud be like:

.. code-block:: python
   :linenos:
     {
       error: parameter is missing
     }
