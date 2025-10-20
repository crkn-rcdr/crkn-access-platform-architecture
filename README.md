#  Access Platform Infrastructure

This document provides an overview of the core service environments within the Access Platform (Canadiana, Heritage, Parl, GAC, NRCan), including Docker service containers and associated Solr/Swift data stores.

## Table of Contents
- [Architecture Notes & Considerations](#architecture-notes--considerations)
- [Canadiana](#canadiana)
- [Heritage](#heritage)
- [Parl](#parl)
- [GAC](#gac)
- [NRCan](#nrcan)

## Architecture Notes & Considerations

- We can either have **1 Solr server with 3 cores per environment**, or **3 Solr servers with 1 core per environment**.  
- We have **one Swift cluster**, but we will try to break down content into **separate Swift containers**.  
- We can either **build a REST API over the PDF Swift container** (additional Docker container) or **expose it publicly**, since we serve PDFs directly to end users.
- We can swap to  different IIIF Image server from **Cantaloupe** - See: [Loris](https://github.com/loris-imageserver/loris), [IIPImage](http://iipimage.sourceforge.net/documentation/server/), [RAIS](https://github.com/uoregon-libraries/rais-image-server)
- We are entirely removing our use of CouchDB for access
- We are sequentially rolling out the following environments. We will first work on Canadiana.
- We can think about having an additional -test environment **per host environment**
---

### Canadiana
**URL:** https://www.canadiana.ca/

| Service | Docker Container | Data Stores |
|---------|------------------|-------------|
| [Blacklight Search](https://github.com/crkn-rcdr/crkn_canadiana_blacklight) | blacklight-canadiana | Solr core: blacklight_marc_canadiana |
| [IIIF Content Search](https://github.com/crkn-rcdr/crkn_iiif_content_search) | iiif-cs-canadiana | Solr core: content_search_canadiana |
| [IIIF Presentation](https://github.com/crkn-rcdr/crkn-IIIF-presentation-api) | iiif-p-canadiana | Swift container: iiif_pres_canadiana, Redis cache: iiif_pres_canadiana |
| [IIIF Image](https://github.com/crkn-rcdr/cihm-cantaloupe) | (Cantaloupe/Loris/etc.) | Swift container: iiif_image_canadiana |
| PDF | N/A | Swift container: pdf_canadiana |
| [ARK](https://github.com/crkn-rcdr/noid-ark) | ark-canadiana | Solr core: ark_map_canadiana |

### Heritage
**URL:** https://heritage.canadiana.ca/

| Service | Docker Container | Data Stores |
|---------|------------------|-------------|
| Blacklight Search | blacklight-heritage | Solr core: blacklight_marc_heritage |
| IIIF Content Search | iiif-cs-heritage | Solr core: content_search_heritage |
| IIIF Presentation | iiif-p-heritage | Swift container: iiif_pres_heritage, Redis cache: iiif_pres_heritage  |
| IIIF Image | (Cantaloupe/Loris/etc.) | Swift container: iiif_image_heritage |
| PDF | N/A | Swift container: pdf_heritage |
| ARK | ark-heritage | Solr core: ark_map_heritage |

### Parl
**URL:** https://parl.canadiana.ca/

| Service | Docker Container | Data Stores |
|---------|------------------|-------------|
| Blacklight Search | blacklight-parl | Solr core: blacklight_marc_parl |
| IIIF Content Search | iiif-cs-parl | Solr core: content_search_parl |
| IIIF Presentation | iiif-p-parl | Swift container: iiif_pres_parl, Redis cache: iiif_pres_parl  |
| IIIF Image | (Cantaloupe/Loris/etc.) | Swift container: iiif_image_parl |
| PDF | N/A | Swift container: pdf_parl |
| ARK | ark-parl | Solr core: ark_map_parl |

### GAC
**URL:** https://gac.canadiana.ca/

| Service | Docker Container | Data Stores |
|---------|------------------|-------------|
| Blacklight Search | blacklight-gac | Solr core: blacklight_marc_gac |
| IIIF Content Search | iiif-cs-gac | Solr core: content_search_gac |
| IIIF Presentation | iiif-p-gac | Swift container: iiif_pres_gac, Redis cache: iiif_pres_gac  |
| IIIF Image | (Cantaloupe/Loris/etc.) | Swift container: iiif_image_gac |
| PDF | N/A | Swift container: pdf_gac |
| ARK | ark-gac | Solr core: ark_map_gac |

### NRCan
**URL:** https://nrcan.canadiana.ca/

| Service | Docker Container | Data Stores |
|---------|------------------|-------------|
| Blacklight Search | blacklight-nrcan | Solr core: blacklight_marc_nrcan |
| IIIF Content Search | iiif-cs-nrcan | Solr core: content_search_nrcan |
| IIIF Presentation | iiif-p-nrcan | Swift container: iiif_pres_nrcan, Redis cache: iiif_pres_nrcan  |
| IIIF Image | (Cantaloupe/Loris/etc.) | Swift container: iiif_image_nrcan |
| PDF | N/A | Swift container: pdf_nrcan |
| ARK | ark-nrcan | Solr core: ark_map_nrcan |

## Container Communications
[Edit here](https://lucid.app/lucidchart/b1827ce9-7d55-43ab-a504-9797e6b07d24/edit?viewport_loc=289%2C520%2C810%2C928%2C0_0&invitationId=inv_3c583eea-eea8-4eb8-b5e9-f2afd1d8c0c3)

The end user goes to the Blacklight docker container to search the access platform. Blacklight calls the other web server docker containers to receive content to show the end user.
<img width="800" height="800" alt="Discovery Platform Infrastructure" src="https://github.com/user-attachments/assets/b2206568-9e93-4f67-ab33-9917b75604bd" />

