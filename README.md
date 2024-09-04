## TP - Prospection foncière - Automatisation d'un process de recherche de parcelle

*Toute ressemblance avec LE TP historique que tout stagiaire IDGEO a en mémoire n'est que pure coïncidence d'après Jean-François.*

Dans le cadre d'une recherche efficace de parcelles suceptibles d'acceuillir la construction de résidence,
un groupe de promotion immobilière cherche à trouver des parcelles de plus de 3000 m², qui se situe à
la fois à moins de 2 km d'une école (ECOLE ELEMENTAIRE), de 10 km d'un collège, à moins de 15 km d'une 
gare ferrofière, mais à plus de 500 m d'un tronçon d'autoroute.

L'outil d'automatisation a utiliser, peut aussi bien être le modeleur graphique de QGIS, le model builder de ArcGIS Pro 
ou FME Workbench. L'idée globale est de fournir sous format GPKG une couche des parcelles retenues en Lambert 93.

Certes, ce TP n'aborde pas les dispositions du PLU à la construction de résidence ou encore la construction de plusieurs 
étages, il s'agirait d'une question bonus. On pourrait également y ajouter des calculs d'itinéraires piéton ou vélo. On pourrait 
aussi y dédier une thèse ...

### Données en entrée

Puisque nous sommes à l'heure de l'open data, que le stockage et la réplication de données n'est pas ethiquement 
écologique (ne pas y voir d'action militante mais juste raisonnée !), des liens pour trouver les données sont suggérés.

- commune france, shp, 4326, from admin express mais ne l'avez-vous pas déjà !
- departement france, shp, 4326, from admin express mais ne l'avez-vous pas déjà !
- geoloc des etablissements scolaire france, geojson, 4326, [le lien](https://data.education.gouv.fr/explore/dataset/fr-en-adresse-et-geolocalisation-etablissements-premier-et-second-degre/export/?disjunctive.numero_uai&disjunctive.nature_uai&disjunctive.nature_uai_libe&disjunctive.code_departement&disjunctive.code_region&disjunctive.code_academie&disjunctive.code_commune&disjunctive.libelle_departement&disjunctive.libelle_region&disjunctive.libelle_academie&disjunctive.secteur_prive_code_type_contrat&disjunctive.secteur_prive_libelle_type_contrat&disjunctive.code_ministere&disjunctive.libelle_ministere&location=8,43.30539,1.77081&basemap=jawg.streets&dataChart=eyJxdWVyaWVzIjpbeyJjb25maWciOnsiZGF0YXNldCI6ImZyLWVuLWFkcmVzc2UtZXQtZ2VvbG9jYWxpc2F0aW9uLWV0YWJsaXNzZW1lbnRzLXByZW1pZXItZXQtc2Vjb25kLWRlZ3JlIiwib3B0aW9ucyI6eyJkaXNqdW5jdGl2ZS5udW1lcm9fdWFpIjp0cnVlLCJkaXNqdW5jdGl2ZS5uYXR1cmVfdWFpIjp0cnVlLCJkaXNqdW5jdGl2ZS5uYXR1cmVfdWFpX2xpYmUiOnRydWUsImRpc2p1bmN0aXZlLmNvZGVfZGVwYXJ0ZW1lbnQiOnRydWUsImRpc2p1bmN0aXZlLmNvZGVfcmVnaW9uIjp0cnVlLCJkaXNqdW5jdGl2ZS5jb2RlX2FjYWRlbWllIjp0cnVlLCJkaXNqdW5jdGl2ZS5jb2RlX2NvbW11bmUiOnRydWUsImRpc2p1bmN0aXZlLmxpYmVsbGVfZGVwYXJ0ZW1lbnQiOnRydWUsImRpc2p1bmN0aXZlLmxpYmVsbGVfcmVnaW9uIjp0cnVlLCJkaXNqdW5jdGl2ZS5saWJlbGxlX2FjYWRlbWllIjp0cnVlLCJkaXNqdW5jdGl2ZS5zZWN0ZXVyX3ByaXZlX2NvZGVfdHlwZV9jb250cmF0Ijp0cnVlLCJkaXNqdW5jdGl2ZS5zZWN0ZXVyX3ByaXZlX2xpYmVsbGVfdHlwZV9jb250cmF0Ijp0cnVlLCJkaXNqdW5jdGl2ZS5jb2RlX21pbmlzdGVyZSI6dHJ1ZSwiZGlzanVuY3RpdmUubGliZWxsZV9taW5pc3RlcmUiOnRydWV9fSwiY2hhcnRzIjpbeyJhbGlnbk1vbnRoIjp0cnVlLCJ0eXBlIjoibGluZSIsImZ1bmMiOiJBVkciLCJ5QXhpcyI6ImNvb3Jkb25uZWVfeCIsInNjaWVudGlmaWNEaXNwbGF5Ijp0cnVlLCJjb2xvciI6IiMxYjFiMzUifV0sInhBeGlzIjoibmF0dXJlX3VhaSIsIm1heHBvaW50cyI6IiIsInRpbWVzY2FsZSI6IiIsInNvcnQiOiIifV0sImRpc3BsYXlMZWdlbmQiOnRydWUsImFsaWduTW9udGgiOnRydWUsInRpbWVzY2FsZSI6IiJ9)
- gare ferrovière occitanie, shp, 4326, https://data.laregion.fr/explore/dataset/localisation-des-gares-et-haltes-ferroviaires-doccitanie/export/?location=9,43.2423,1.25574&basemap=jawg.streets
- cadastre archive zip 31, edigeo par commune, par chauvinisme j'ai pris la Haute-Garonne mais surtout parce qu'il n'y
 a plus de terrain disponible en Ariège, https://files.data.gouv.fr/cadastre/dgfip-pci-vecteur/2021-04-01/edigeo/departements/
- réseau autoroutier, https://data.laregion.fr/explore/dataset/trace-du-reseau-autoroutier-doccitanie/export/?refine.vocation=Type+autoroutier&location=8,43.60028,2.24121&basemap=jawg.streets

 ## Données bonus (pour produire un résultat plus fin)
 
 - les zones naturelles ZNIEFF, on exclue les ZNIEFF de type 1
 
