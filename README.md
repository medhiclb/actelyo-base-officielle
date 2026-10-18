# Base officielle Actelyo

Ce dépôt ne contient que le **paquet de la base officielle** que le logiciel Actelyo télécharge lors de
l'installation d'un serveur de cabinet : des **textes publics**, rien d'autre. Aucun code, aucune donnée d'un cabinet,
aucune donnée personnelle ajoutée par Actelyo.

**Source : DILA / Légifrance** (et DGFiP pour le BOFiP), réutilisée sous **Licence Ouverte / Open Licence 2.0**.
Ni la DILA ni la DGFiP ne cautionnent cette réutilisation.

## Dernière édition

| | |
|---|---|
| Édition | **2026-10** (Release [`base-2026-10`](https://github.com/medhiclb/actelyo-base-officielle/releases/tag/base-2026-10)) |
| À jour au | **17/10/2026** (dernière publication de la DILA appliquée) |
| Publiée le | 18/10/2026 |
| Taille | 10,2 Go en 123 morceaux de 256 Mo au plus |

## Contenu

| Fonds | Textes |
|---|---|
| Articles des 28 codes installés (LEGI) | 382 130 |
| Jurisprudence administrative (JADE) | 725 435 |
| Bibliothèque : lois et décrets non codifiés, conventions collectives, CNIL, BOFiP, réponses ministérielles | 1 771 152 |

S'y ajoutent les vecteurs de la recherche par le sens, calculés par Actelyo : nomic-embed-text-v1.5 (1 737 890 vecteurs), solon-embeddings-large-0.1 (678 582 vecteurs). Les comptes exacts par table
sont dans `manifeste.json`.

## Fichiers d'une Release

- `derniere.json` : le pointeur de l'édition (date, empreinte du manifeste, adresses des morceaux) ;
  `derniere.json.sig` : sa signature Ed25519 par Actelyo (base64).
- `manifeste.json` : liste des tables et des morceaux, avec l'empreinte SHA-256 de chacun ; `manifeste.sha256`.
- `tables.<table>.<n>.copy.gz` : les données (format COPY de PostgreSQL, compressé).
- `NOTICE.txt` : mentions de la Licence Ouverte et règles sur les données personnelles.

La dernière édition est toujours à l'adresse
`https://github.com/medhiclb/actelyo-base-officielle/releases/latest/download/derniere.json`.

## Vérifier la signature

La clé publique d'Actelyo est dans [`cle-publique.pem`](cle-publique.pem) (la même est intégrée au logiciel) :

```
-----BEGIN PUBLIC KEY-----
MCowBQYDK2VwAyEAPZYzIDFvElgUYX0mSoDWH/vNvf2bhrwH3KLVVLXFtOU=
-----END PUBLIC KEY-----
```

Avec OpenSSL 3 :

```sh
curl -sSLO https://github.com/medhiclb/actelyo-base-officielle/releases/latest/download/derniere.json
curl -sSLO https://github.com/medhiclb/actelyo-base-officielle/releases/latest/download/derniere.json.sig
curl -sSLO https://raw.githubusercontent.com/medhiclb/actelyo-base-officielle/main/cle-publique.pem
base64 -d -i derniere.json.sig -o derniere.sig
openssl pkeyutl -verify -pubin -inkey cle-publique.pem -rawin -in derniere.json -sigfile derniere.sig
# « Signature Verified Successfully »
```

Puis l'empreinte du manifeste (champ `manifeste_sha256` du pointeur) et celle de chaque morceau (dans le manifeste) :
`shasum -a 256 <fichier>`. Le logiciel fait ces contrôles lui-même et refuse tout fichier qui ne correspond pas.

## Mentions (NOTICE.txt)

```
Paquet « Base officielle Actelyo », édition 2026-10.

Contient des informations publiques réutilisées sous Licence Ouverte / Open Licence version 2.0
(https://www.etalab.gouv.fr/licence-ouverte-open-licence) :
- Direction de l'information légale et administrative (DILA) : bases LEGI, JORF, JADE, KALI, CNIL et
  questions-réponses parlementaires (https://echanges.dila.gouv.fr/OPENDATA/), dernière publication
  appliquée : 2026-10-17 ;
- Direction générale des finances publiques (DGFiP) : BOFiP-Impôts (https://data.economie.gouv.fr),
  à la date de constitution du paquet (2026-10-18).

Ces informations ont été transformées par Actelyo SAS : conversion en tables PostgreSQL et ajout de
vecteurs calculés avec les modèles text-embedding-nomic-embed-text-v1.5, text-embedding-solon-embeddings-large-0.1 (nomic-embed-text-v1.5 : licence Apache-2.0 ;
Solon-embeddings-large-0.1 : licence MIT). Ni la DILA ni la DGFiP ne cautionnent cette réutilisation.

Données à caractère personnel : la réutilisation reste soumise à la loi n° 78-17 du 6 janvier 1978, à
l'article L. 322-2 du code des relations entre le public et l'administration et à l'article L. 111-13
du code de l'organisation judiciaire (article L. 10 du code de justice administrative). Toute
réidentification des personnes est interdite, de même que toute réutilisation de l'identité des
magistrats et des membres du greffe ayant pour objet ou pour effet d'évaluer, d'analyser, de comparer
ou de prédire leurs pratiques professionnelles.
```
