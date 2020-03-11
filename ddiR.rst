.. _ddiR

ddiR: R package to interact with OmicsDI RestFul API
==========================================

An `R package <https://github.com/OmicsDI/ddiR>`_ to obtain data from the Omics Discovery Index `OmicsDI <http://www.omicsdi.org>`_. It uses its RESTful Web Services at `OmicsDI WS <http://www.omicsdi.org/ws/>`_ for that purpose.

Currently, the following domain entities are supported:

* Dataset as S4 objects, including methods to get them from OmicsDI by accession and `as.data.frame`
* Publication as S4 objects, including methods to get them from OmicsDI by accession and `as.data.frame`
* Term as S4 objects, including methods to get them from OmicsDI by term and `as.data.frame`

Installation
-------------------------

First, we need to install `devtools`:

    install.packages("devtools")
    library(devtools)

Then we just call

    install_github("enriquea/ddiR")
    library(ddiR)

Examples
---------------------

This example retrives all dataset details given accession and database identifier

.. code-block:: R
   :linenos:

    library(ddiR)

    dataset = get.DatasetDetail(accession="PXD000210", database="pride")

    # print dataset full name
    get.dataset.name(dataset)

    # print dataset omics type
    get.dataset.omics(dataset)



Access to all datasets for NOTCH1 gene

.. code-block:: python
   :linenos:

    library(ddiR)

    datasets <- search.DatasetsSummary(query = "NOTCH1")

    sink("outfile.txt")
    for(datasetCount in seq(from = 0, to = datasets@count, by = 100)){

       datasets <- search.DatasetsSummary(query = "NOTCH1", start = datasetCount, size = 100)

       for(dataset in datasets@datasets){
             dataset = get.DatasetDetail(accession=dataset.id(dataset), database=database(dataset))
             print(paste(dataset.id(dataset), get.dataset.omics(dataset), get.dataset.link(dataset)))
            }
       }
    }
    sink()



This example shows how retrieve all the metadata similarity scores by using the R-package ddiR.

.. code-block:: python
   :linenos:

    library(ddiR)
    datasets <- search.DatasetsSummary(query = "*:*")
    i  = 0
    sink("outfile.txt")
    for(datasetCount in seq(from = 0, to = datasets@count, by = 100)){

        datasets <- search.DatasetsSummary(query = "*:*", start = datasetCount, size = 100)

        for(dataset in datasets@datasets){
              Similar = get.MetadataSimilars(accession = dataset@dataset.id, database = dataset@database)
              rank = 0
              for(similarDataset in Similar@datasets){
                 print(paste(dataset@dataset.id, similarDataset@dataset.id, similarDataset@score, dataset@omics.type, rank))
                 rank = rank + 1
              }
        }
     }
     sink()
