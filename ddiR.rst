.. _ddiR

ddiR: R package
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

.. code-block:: r
   :linenos:

    library(ddiR)

    dataset = get.DatasetDetail(accession="PXD000210", database="pride")

    # print dataset full name
    get.dataset.name(dataset)

    # print dataset omics type
    get.dataset.omics(dataset)



Access to all datasets for NOTCH1 gene

.. code-block:: r
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


Getting the dataset IDs and full link of 20 Genomics studies in Cancer

.. code-block:: r
   :linenos:
   
    datasets <- search.DatasetsSummary(query = "Cancer AND Genomics")

    for(dataset in datasets@datasets){
        dataset = get.DatasetDetail(accession=dataset.id(dataset), database=database(dataset))
        print(paste(dataset.id(dataset), get.dataset.link(dataset), sep = ' '))
    }
    
    
Print the dataset IDs and short description of 20 Proteomics studies for tumor supressor p53

.. code-block:: r
   :linenos:
   
    datasets <- search.DatasetsSummary(query = "p53 AND Proteomics")

    for(dataset in datasets@datasets){
        dataset = get.DatasetDetail(accession=dataset.id(dataset), database=database(dataset))
        print(paste(dataset.id(dataset), get.dataset.name(dataset), sep = ' '))
    }
    
    
Getting Proteomics studies in Heart tissue from PRIDE database

.. code-block:: r
   :linenos:
   
    datasets <- search.DatasetsSummary(query = "Heart")

    for(dataset in datasets@datasets){
        dataset = get.DatasetDetail(accession=dataset.id(dataset), database=database(dataset))
        if(database(dataset)=='pride')
         print(paste(dataset.id(dataset), get.dataset.tissues(dataset), get.dataset.omics(dataset), sep = ' '))
    }


This example shows how retrieve all the metadata similarity scores by using the R-package ddiR.

.. code-block:: r
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
