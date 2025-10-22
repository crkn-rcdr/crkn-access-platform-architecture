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
- There is no change in the schemas between environments for the data stores.
---

### Canadiana
**URL:** https://www.canadiana.ca/

| Service | Docker Container | Data Stores |
|---------|------------------|-------------|
| [Blacklight Search](https://github.com/crkn-rcdr/crkn_canadiana_blacklight) | blacklight-canadiana, internal port: 3000, [.env (access protected)](https://start.1password.com/open/i?a=PTZ4LGIASRD7LAT3GQWIQBPHGU&v=yb4563uqdpjn7xdgzw4f7tp3qy&i=qw7bpykrrk6ylk4hh2pngnswwe&h=my.1password.com) | Solr core: [blacklight_marc_canadiana](https://github.com/crkn-rcdr/crkn_canadiana_blacklight/tree/master/data/data/blacklight_marc/conf) |
| [IIIF Content Search](https://github.com/crkn-rcdr/crkn_iiif_content_search) | iiif-cs-canadiana, internal port: 3000, [.env (access protected)](https://start.1password.com/open/i?a=PTZ4LGIASRD7LAT3GQWIQBPHGU&v=yb4563uqdpjn7xdgzw4f7tp3qy&i=ozakedjbvm62ygkdvertamb3ky&h=my.1password.com) | Solr core: [content_search_canadiana](https://github.com/crkn-rcdr/crkn_iiif_content_search/tree/main/solr/config) |
| [IIIF Presentation](https://github.com/crkn-rcdr/crkn-IIIF-presentation-api) | iiif-p-canadiana, internal port: xxxx, [.env (access protected)](https://start.1password.com/open/i?a=PTZ4LGIASRD7LAT3GQWIQBPHGU&v=yb4563uqdpjn7xdgzw4f7tp3qy&i=zacrecf22qajihjousdawnqpua&h=my.1password.com) | Swift container: iiif_pres_canadiana, Redis cache: iiif_pres_canadiana |
| [IIIF Image](https://github.com/crkn-rcdr/cihm-cantaloupe) | (Cantaloupe/Loris/etc.) | Swift container: iiif_image_canadiana |
| PDF | N/A | Swift container: pdf_canadiana |
| [ARK](https://github.com/crkn-rcdr/noid-ark) | ark-canadiana, internal port: xxxx, [.env (access protected)](https://start.1password.com/open/i?a=PTZ4LGIASRD7LAT3GQWIQBPHGU&v=yb4563uqdpjn7xdgzw4f7tp3qy&i=uez5nqx4shqzhxdqlg7u7johoq&h=my.1password.com)  | Solr core: ark_map_canadiana |

### Heritage
**URL:** https://heritage.canadiana.ca/

| Service | Docker Container | Data Stores |
|---------|------------------|-------------|
| Blacklight Search | blacklight-heritage, port: xxxx, env (access protected)  | Solr core: blacklight_marc_heritage |
| IIIF Content Search | iiif-cs-heritage, port: xxxx, env (access protected)  | Solr core: content_search_heritage |
| IIIF Presentation | iiif-p-heritage, port: xxxx, env (access protected)  | Swift container: iiif_pres_heritage, Redis cache: iiif_pres_heritage  |
| IIIF Image | (Cantaloupe/Loris/etc.) | Swift container: iiif_image_heritage |
| PDF | N/A | Swift container: pdf_heritage |
| ARK | ark-heritage, port: xxxx, env (access protected)  | Solr core: ark_map_heritage |

### Parl
**URL:** https://parl.canadiana.ca/

| Service | Docker Container | Data Stores |
|---------|------------------|-------------|
| Blacklight Search | blacklight-parl, port: xxxx, env (access protected)  | Solr core: blacklight_marc_parl |
| IIIF Content Search | iiif-cs-parl, port: xxxx, env (access protected)  | Solr core: content_search_parl |
| IIIF Presentation | iiif-p-parl, port: xxxx, env (access protected)  | Swift container: iiif_pres_parl, Redis cache: iiif_pres_parl  |
| IIIF Image | (Cantaloupe/Loris/etc.) | Swift container: iiif_image_parl |
| PDF | N/A | Swift container: pdf_parl |
| ARK | ark-parl, port: xxxx, env (access protected)  | Solr core: ark_map_parl |

### GAC
**URL:** https://gac.canadiana.ca/

| Service | Docker Container | Data Stores |
|---------|------------------|-------------|
| Blacklight Search | blacklight-gac, port: xxxx, env (access protected)  | Solr core: blacklight_marc_gac |
| IIIF Content Search | iiif-cs-gac, port: xxxx, env (access protected)  | Solr core: content_search_gac |
| IIIF Presentation | iiif-p-gac, port: xxxx, env (access protected)  | Swift container: iiif_pres_gac, Redis cache: iiif_pres_gac  |
| IIIF Image | (Cantaloupe/Loris/etc.) | Swift container: iiif_image_gac |
| PDF | N/A | Swift container: pdf_gac |
| ARK | ark-gac, port: xxxx, env (access protected)  | Solr core: ark_map_gac |

### NRCan
**URL:** https://nrcan.canadiana.ca/

| Service | Docker Container | Data Stores |
|---------|------------------|-------------|
| Blacklight Search | blacklight-nrcan, port: xxxx, env (access protected)  | Solr core: blacklight_marc_nrcan |
| IIIF Content Search | iiif-cs-nrcan, port: xxxx, env (access protected)  | Solr core: content_search_nrcan |
| IIIF Presentation | iiif-p-nrcan, port: xxxx, env (access protected)  | Swift container: iiif_pres_nrcan, Redis cache: iiif_pres_nrcan  |
| IIIF Image | (Cantaloupe/Loris/etc.) | Swift container: iiif_image_nrcan |
| PDF | N/A | Swift container: pdf_nrcan |
| ARK | ark-nrcan, port: xxxx, env (access protected) | Solr core: ark_map_nrcan |

## Storage Requirements - Portal Aggregation Summary (2025-10-22)

### Canvas-Level Totals

| Portal           | Canvases  | Image Source      | OCR PDF     | TOTAL      |
|------------------|------------:|-------------:|-------------:|-------------:|
| canadiana        | 21,271,334  | 18.62 TiB   | 11.63 TiB   | 30.25 TiB   |
| heritage         | 43,195,033  | 8.61 TiB    | 4.49 TiB    | 13.11 TiB   |
| parl             | 4,728,789   | 4.28 GiB    | 2.40 TiB    | 2.40 TiB    |
| gac              | 1,389,764   | 1.82 TiB    | 322.51 GiB  | 2.13 TiB    |
| nrcan            | 261,403     | 651.74 MiB  | 4.11 GiB    | 4.74 GiB    |

---

### Manifest-Level (PDF Only)

| Portal           | Manifests | Manifest PDF |
|------------------|-----------:|--------------:|
| canadiana        | 518,672    | 11.87 TiB    |
| heritage         | 24,834     | 4.20 TiB     |
| parl             | 6,640      | 2.39 TiB     |
| gac              | 35,584     | 270.42 GiB   |
| nrcan            | 688        |  4.17 GiB     |

## Container Communications
[Edit here](https://lucid.app/lucidchart/b1827ce9-7d55-43ab-a504-9797e6b07d24/edit?viewport_loc=289%2C520%2C810%2C928%2C0_0&invitationId=inv_3c583eea-eea8-4eb8-b5e9-f2afd1d8c0c3)

The end user goes to the Blacklight docker container to search the access platform. Blacklight calls the other web server docker containers to receive content to show the end user.
<img width="800" height="800" alt="Discovery Platform Infrastructure" src="https://github.com/user-attachments/assets/b2206568-9e93-4f67-ab33-9917b75604bd" />

## Project Planning
To see a list of Issues for the implementation of this plan, [click here](https://github.com/orgs/crkn-rcdr/projects/16/views/5?pane=issue&itemId=79315305&issue=crkn-rcdr%7Csystems-administration-public%7C5).

