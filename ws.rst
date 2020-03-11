.. _ws

OmicsDI Restful Documentation
===============

Most data in the Datatsets Discovery Index can be accessed
programmatically using a [RESTful API](www.omicsdi.org/ws).
The API implementation is based on the Spring Rest Framework.

Web-browsable API
----------------------

The OmicsDI API is web browsable, which means that:

- The query results returned by the API are available in JSONformat and also XML. This ensures that they can be viewed by human and accessed programmatically by computer.
- The main [RESTful API](www.omicsdi.org/ws) page provides a simple web-based user
  interface, which allows developers to familiarize themselves with the API and get a
  better sense of the OmicsDI data before writing a single line of code.

many resources are hyperlinked so that it's possible to navigate the API in the browser.

As a result, developers can familiarize themselves with the API and get a better sense of the OmicsDI data.

API documentation
------------------------------

Responses containing multiple entries have the following fields:

- the count is the number of entries in the matching set.
- dataset is an array of datasets.
- facet is an array of facets.

Example

.. code-block:: json
   :linenos:

       http://www.omicsdi.org/ws/dataset/search?query=human
          {
             "count": 733,
             "datasets": [
                  {
                    "id": "PXD000456",
                    "source": "pride",
                    "title": "Human glomerular extracellular matrix analysed by LC-MSMS",
                    "description": "Extracellular matrix proteins were isolated from human glomeruli and analysed by LC-MSMS",
                    "keywords": [
                        "Human",
                        "kidney",
                        "glomerulus",
                        "extracellular matrix"
                    ],
                    "organisms": [
                       {
                         "acc": "9606",
                         "name": "Homo sapiens"
                       }
                    ],
                    "publicationDate": "20140122"
                  },
               // 19 more datasets
             ],
             "facets": [
               {
                   "id": "modification",
                   "label": "Modification",
                   "total": 181,
                   "facetValues": [
                  {
                     "label": "Unknown modification",
                     "value": "unknown modification",
                     "count": "5"
               },
               //other facet values
             ],
             },
           //other facets
           ]
         }


Responses containing just a single dataset have some extra navigation fields, and without the facets

{{< highlight json>}}

http://www.omicsdi.org/ws/dataset/get?acc=PXD001848&database=PRIDE
{
    "id": "PXD001848",
    "name": "Global Analysis of Protein Folding Thermodynamics for Disease State Characterization, MCF7 vs MDAMB231",
    "description": "Protein biomarkers can be used to characterize and diagnose disease states such as cancer. ....",
    "keywords": null,
    "publicationDate": "20150410",
    "publications": [
        {
            "id": "25825992",
            "publicationDate": "2015-04-09",
            "title": "Global analysis of protein folding thermodynamics for disease state characterization.",
            "pubabstract": "Current methods for the large-scale characterization of disease states ....",
            "cycle": "testcyclehere"
        }
    ],
    "related_datasets": null,
    "data_protocol": "Peak lists were extracted from the raw LC-MS/MS data files and the data were searched against t...."
}
{{< / highlight >}}

### Pagination

Responses containing multiple datasets are paginated to prevent accidental downloads
of large amounts of data and to speed up the ``API``. The ``page size`` is controlled by the size parameter. Its default value is 20 datasets per page, and the maximum number of datasets per page is 100.

Another parameter is start which indicates the numeric order (starting from 0, not 1) of the first dataset in this page. Its default value is 0.

Examples:

- http://www.omicsdi.org/ws/dataset/search?query=human&start=0&size=50

- http://www.omicsdi.org/ws/dataset/search?query=human&start=0&size=20

### Sort

The result datasets can be sorted using the title, description, publication date, accession id and the relevance of the query term.

Examples:

- http://www.omicsdi.org/ws/dataset/search?query=human&sort_field=id
- http://www.omicsdi.org/ws/dataset/search?query=human&sort_field=publication_date

### Filtering

The API supports several filtering operations that complement the main ``OmicsDI`` search functionality.

Filtering by search term, there is 1 URL parameter: query

Examples

- http://www.omicsdi.org/ws/dataset/search?query=human

- http://www.omicsdi.org/ws/dataset/search?query=cancer

#### Filtering by omics type:

The omics type can be specified by adding terms in the query url parameter with key: omics_type (possible values: Proteomics, Metabolomics, Genomics, Transcriptomics).

Examples:

- [http://www.omicsdi.org/ws/dataset/search?query=human AND omics_type:"Proteomics"](http://www.omicsdi.org/ws/dataset/search?query=human%20AND%20omics_type:%22Proteomics%22)

#### Filtering by database:

The database can be specified by adding terms in the query URL parameter with key: repository (possible values: MassIVE, Metabolights, PeptideAtlas, PRIDE, GPMDB, EGA, Metabolights, Metabolomics Workbench, MetabolomeExpress, GNPS, ArrayExpress, ExpressionAtlas).

Examples:

- [http://www.omicsdi.org/ws/dataset/search?query=human AND repository:"Metabolights"](http://www.omicsdi.org/ws/dataset/search?query=human%20AND%20repository:%22Metabolights%22)


#### Filtering by Organism

The organism can be specified by adding terms in the query URL parameter with key: TAXONOMY (possible values must be the TAXONOMY id: 9606, 10090...).

Examples:

- [http://www.omicsdi.org/ws/dataset/search?query=human AND TAXONOMY:"9606"](http://www.omicsdi.org/ws/dataset/search?query=human%20AND%20TAXONOMY:%229606%22)


#### Filtering by Tissue

The tissue can be specified by adding terms in the query URL parameter with key: tissue (possible values: Liver, Cell culture, Brain, Lung...).

Examples:

- [http://www.omicsdi.org/ws/dataset/search?query=human AND tissue:"Brain"](http://www.omicsdi.org/ws/dataset/search?query=human%20AND%20tissue:%22Brain%22)

#### Filtering by Disease

The disease can be specified by adding terms in the query URL parameter with key: disease (possible values: Breast cancer, Lymphoma, Carcinoma, prostate adenocarcinoma...).

Examples

- [http://www.omicsdi.org/ws/dataset/search?query=human AND tissue:"Breast cancer"](http://www.omicsdi.org/ws/dataset/search?query=human%20AND%20tissue:%22Breast%20cancer%22)


#### Filtering by Modification (in proteomics)

The Modifications (in proteomics) can be specified by adding terms in the query URL parameter with key: disease (possible values: Deamidated residue, Deamidated, Monohydroxylated residue, Iodoacetamide derivatized residue...).

Examples:

- [http://www.omicsdi.org/ws/dataset/search?query=human AND modification:"iodoacetamide derivatized residue"](http://www.omicsdi.org/ws/dataset/search?query=human%20AND%20modification:%22iodoacetamide%20derivatized%20residue%22)

#### Filtering by Instruments & Platforms

The Instruments & Platforms can be specified by adding terms in the query URL parameter with key: instrument_platform (possible values: QSTAR, LTQ Orbitrap, Q Exactive, LTQ...).

Examples:

- [http://www.omicsdi.org/ws/dataset/search?query=human AND instrument_platform:"Q Exactive"](http://www.omicsdi.org/ws/dataset/search?query=human%20AND%20instrument_platform:%22Q%20Exactive%22)

#### Filtering by Publication Date

The Publication Date can be specified by adding terms in the query URL parameter with key: "publication_date" (possible values: 2015, 2014, 2013, 2014...).

Examples:

- [http://www.omicsdi.org/ws/dataset/search?query=human AND publication_date:"2015"](http://www.omicsdi.org/ws/dataset/search?query=human%20AND%20publication_date:%222015%22)

#### Filtering by Technology Type

The Technology Type can be specified by adding terms in the query URL parameter with key: "technology_type" (possible values: Mass Spectrometry, Bottom-up proteomics, Gel-based experiment, Shotgun proteomics...).

Examples:

- [http://www.omicsdi.org/ws/dataset/search?query=human AND technology_type:"Mass Spectrometry"](http://www.omicsdi.org/ws/dataset/search?query=human%20AND%20technology_type:%22Mass%20Spectrometry%22)

#### Combined filters

Any filters can be combined to narrow down the query using the AND operator. More logical operators will be supported in the future.

Examples:

- [http://www.omicsdi.org/ws/dataset/search?query=human AND technology_type:"Shotgun proteomics" and AND modification:"monohydroxylated residue"](http://www.omicsdi.org/ws/dataset/search?query=human%20AND%20technology_type:%22Shotgun%20proteomics%22%20and%20AND%20modification:%22monohydroxylated%20residue%22)