# README - Cloud Healthcare Unit Project (English)

## Authors
**Oussama ALI-BEY, Alen AMIRBEKYAN, Adam MAHRAOUI, Antonin RUDONI**  
**FISA INFO 23-26**

## Table of Contents
1. [Introduction](#introduction)
2. [Descriptions of JOBS in Talend](#descriptions-of-jobs-in-talend)
3. [Data Management in Tables](#data-management-in-tables)
    - a) [Data Creation and Loading](#data-creation-and-loading)
    - b) [Data Verification and Access](#data-verification-and-access)
    - c) [Table Population](#table-population)
4. [Performance Management](#performance-management)
    - a) [Partitioning and Buckets](#partitioning-and-buckets)
    - b) [Response Time Evaluation](#response-time-evaluation)
5. [Conclusion](#conclusion)
6. [Appendices](#appendices)

## Introduction
The healthcare sector is evolving rapidly, and medical institutions need to adapt to increasingly digital techniques. The CHU (Cloud Healthcare Unit) group aims to establish an efficient data warehouse to manage the continuous flow of data from its care management systems and FTP systems. This project aims to design an integrated solution that allows optimal extraction, storage, integration, exploration, and visualization of medical data.

## Descriptions of JOBS in Talend
### JOB Consultation
- Retrieves the consultation date from PostgreSQL, splits it into day, month, and year, and renames the columns according to the schema.

### JOB Patient
- Verifies the validity of the birth date and splits it into day, month, and year. It also checks and converts gender data.

### JOB ZoneGeo
- Links a department code to its region name. This table was created manually, considering the new 2016 regions.

### JOB Satisfaction
- Populates the Satisfaction table from CSV and Excel files, filters out rows without a global satisfaction rate, and creates specific columns.

### JOB Deaths
- Transforms death data, verifies dates, and adds an identifier. Ignores rows with extra columns due to commas in addresses.

### JOB Professional
- Links each professional with their specialty and establishment, enabling precise analyses.

## Data Management in Tables
### Data Creation and Loading
- Uses HIVE to load data. Example procedure:
```sh
hadoop fs -copyFromLocal /home/cloudera/Patient.txt /user/hive/data
CREATE EXTERNAL TABLE IF NOT EXISTS log_patient (
  id_patient INT,
  genre STRING,
  jour_naissance INT,
  mois_naissance INT,
  annee_naissance INT,
  taille INT,
  poids FLOAT,
  groupe_sanguin STRING
)
COMMENT 'intermediate table'
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/user/hive/data';
```

### Data Verification and Access
- Data verification in the HUE interface to ensure the integrity of loaded data.

### Table Population
- Uses scripts and Talend jobs to populate tables with precise and verified data.

## Performance Management
### Partitioning and Buckets
- Utilizes partitioning and clustering to optimize queries. For example, partitioning by diagnosis for the Consultation table facilitates analyses in PowerBI.

### Response Time Evaluation
- Measures response times for unoptimized SQL queries, repeated three times to ensure data consistency.

| Table          | Response Time 1 (sec) | Response Time 2 (sec) | Response Time 3 (sec) | Average (sec) |
|----------------|-----------------------|-----------------------|-----------------------|---------------|
| Patient        | 48                    | 25                    | 24                    | 32.33         |
| Diagnosis      | 25                    | 24                    | 24.05                 | 24.5          |
| Consultation   | 23.81                 | 25.1                  | 24                    | 24.3          |
| Establishment  | 26.56                 | 24.96                 | 25                    | 25.5          |
| ZoneGeo        | 25.77                 | 24                    | 23.2                  | 24.32         |
| Satisfaction   | 24                    | 23.3                  | 25                    | 24.1          |
| Deaths         | 68                    | 62                    | 64                    | 64.67         |
| Professional   | 30                    | 31                    | 25                    | 28.67         |
| Hospitalization| 25                    | 26                    | 24.4                  | 25.13         |

## Conclusion
After populating the tables with relevant data and observing the benefits of partitioning, it's time to create a PowerBI dashboard to visualize and exploit the data.
(VIEW IN BI FOLDER)

------------------------------------------------------------------------------------

# README - Projet Cloud Healthcare Unit (French)

## Auteurs
**Oussama ALI-BEY, Alen AMIRBEKYAN, Adam MAHRAOUI, Antonin RUDONI**  
**FISA INFO 23-26**

## Table des matières
1. [Introduction](#introduction)
2. [Descriptions des JOBS dans Talend](#descriptions-des-jobs-dans-talend)
3. [Gestion des données dans les tables](#gestion-des-données-dans-les-tables)
    - a) [Création et le chargement de données](#création-et-le-chargement-de-données)
    - b) [Vérification et accès aux données](#vérification-et-accès-aux-données)
    - c) [Peuplement des tables](#peuplement-des-tables)
4. [Gestion des performances](#gestion-des-performances)
    - a) [Partitionnement et buckets](#partitionnement-et-buckets)
    - b) [Evaluation du temps de réponse](#evaluation-du-temps-de-réponse)
5. [Conclusion](#conclusion)
6. [Annexes](#annexes)

## Introduction
Le domaine de la santé évolue rapidement, et les institutions médicales doivent s'adapter à des techniques de plus en plus digitales. Le groupe CHU (Cloud Healthcare Unit) cherche à établir un entrepôt de données efficace pour gérer le flux continu de données de ses systèmes de gestion des soins et des systèmes FTP. Ce projet vise à concevoir une solution intégrale permettant l'extraction, le stockage, l'intégration, l'exploration et la visualisation des données médicales de manière optimale.

## Descriptions des JOBS dans Talend
### JOB Consultation
- Récupère la date de consultation depuis PostgreSQL, la découpe en jour, mois et année, et renomme les colonnes selon le schéma.

### JOB Patient
- Vérifie la conformité de la date de naissance et la sépare en jour, mois et année. Vérifie et convertit les données de sexe.

### JOB ZoneGeo
- Relie un code de département à son nom de région. Création manuelle de la table en tenant compte des nouvelles régions de 2016.

### JOB Satisfaction
- Alimente la table Satisfaction à partir de fichiers CSV et Excel, filtre les lignes sans taux global de satisfaction et crée des colonnes spécifiques.

### JOB Décès
- Transforme les données de décès, vérifie les dates et ajoute un identifiant. Ne prend pas en compte les lignes avec des colonnes supplémentaires dues à des virgules.

### JOB Professionnel
- Lie chaque professionnel avec sa spécialité et son établissement, permettant des analyses précises.

## Gestion des données dans les tables
### Création et le chargement de données
- Utilisation de HIVE pour charger les données. Exemple de procédure de chargement :
```sh
hadoop fs -copyFromLocal /home/cloudera/Patient.txt /user/hive/data
CREATE EXTERNAL TABLE IF NOT EXISTS log_patient (
  id_patient INT,
  genre STRING,
  jour_naissance INT,
  mois_naissance INT,
  annee_naissance INT,
  taille INT,
  poids FLOAT,
  groupe_sanguin STRING
)
COMMENT 'table intermediaire'
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/user/hive/data';
```

### Vérification et accès aux données
- Vérification des tables dans l'interface HUE pour s'assurer de l'intégrité des données chargées.

### Peuplement des tables
- Utilisation de scripts et de jobs Talend pour peupler les tables avec des données précises et vérifiées.

## Gestion des performances
### Partitionnement et buckets
- Utilisation du partitionnement et du clustering pour optimiser les requêtes. Par exemple, le partitionnement par diagnostic pour la table Consultation facilite les analyses dans PowerBI.

### Evaluation du temps de réponse
- Mesure des temps de réponse pour des requêtes SQL non optimisées, répétées trois fois pour assurer la cohérence des résultats.

| Table          | Temps de réponse 1 (sec) | Temps de réponse 2 (sec) | Temps de réponse 3 (sec) | Moyenne (sec) |
|----------------|--------------------------|--------------------------|--------------------------|---------------|
| Patient        | 48                       | 25                       | 24                       | 32,33         |
| Diagnostique   | 25                       | 24                       | 24,05                    | 24,5          |
| Consultation   | 23,81                    | 25,1                     | 24                       | 24,3          |
| Etablissement  | 26,56                    | 24,96                    | 25                       | 25,5          |
| ZoneGeo        | 25,77                    | 24                       | 23,2                     | 24,32         |
| Satisfaction   | 24                       | 23,3                     | 25                       | 24,1          |
| Deces          | 68                       | 62                       | 64                       | 64,67         |
| Professionnel  | 30                       | 31                       | 25                       | 28,67         |
| Hospitalisation| 25                       | 26                       | 24,4                     | 25,13         |

## Conclusion
Après avoir alimenté les tables avec les données pertinentes et constaté les bénéfices du partitionnement, il est temps de créer un tableau de bord sur PowerBI pour visualiser et exploiter les données.
(VOIR DOSSIER BI)
