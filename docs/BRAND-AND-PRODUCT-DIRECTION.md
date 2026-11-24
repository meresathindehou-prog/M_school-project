# UGENCY — direction de marque et principes produit

Version de travail : 0.1 — 24 novembre 2026

## Positionnement
UGENCY est conçu comme un écosystème de services et de coordination à dimension civique. Il doit rapprocher les citoyens, les professionnels et les équipes opérationnelles autour de parcours compréhensibles, traçables et utiles.

## Idée centrale
**Le service commence par les personnes.** La technologie reste en arrière-plan : elle facilite la relation, la coordination et la continuité du service, sans devenir le sujet de l'interface.

## Direction artistique
- Une identité éditoriale, chaleureuse et institutionnelle sans froideur administrative.
- Une palette inspirée de matières réelles : ivoire papier, encre profonde, bronze/cuivre discret, vert sauge et gris pierre.
- Typographie de lecture nette, hiérarchie assumée, marges généreuses et détails de composition précis.
- Photographies documentaires et scènes humaines contextualisées, jamais de portraits synthétiques génériques.
- Cartographie, repères, lignes de parcours et formes de liaison comme vocabulaire graphique.
- Mouvement discret, justifié par l'usage ; pas d'effets décoratifs permanents, de gradients artificiels ni de langage visuel « IA ».

## Emblème
Le signe associe une forme protectrice ouverte, un chemin et deux présences humaines. Il évoque la protection, l'orientation et la coordination. Il doit rester lisible en monochrome, en petit format et sans le logotype.

## Architecture de l'expérience
1. **Public / Citoyen** : découverte des services, demandes, suivi, rendez-vous, alertes et assistance.
2. **Professionnel** : profil, disponibilité, demandes reçues, rendez-vous et coordination.
3. **Opérations** : supervision des demandes, affectation, communication et suivi des incidents.
4. **Administration** : gestion des comptes, rôles, référentiels, règles, audit et configuration.

## Principes techniques non négociables
- Séparer interface, API, domaine métier et persistance.
- Authentification sécurisée, autorisations côté serveur et validation de chaque entrée.
- Journal d'audit pour les opérations sensibles ; minimisation des données personnelles.
- États explicites : chargement, vide, succès, erreur, indisponibilité et reprise.
- Aucun bouton décoratif : chaque action doit conduire à un parcours réel ou être clairement désactivée.
- Tests automatisés sur les règles métier, permissions et parcours critiques.
- Secrets exclusivement dans des variables d'environnement ; jamais dans le dépôt.
- Intégrations externes documentées, isolées derrière des adaptateurs et couvertes par des tests.

## Références étudiées
Les recherches Post Design ont surtout conforté trois principes : une identité propriétaire construite autour d'une idée humaine forte, une composition éditoriale plutôt qu'un kit d'interface générique, et l'usage de matière/photo/typographie avec retenue. Les références sont des sources d'inspiration, pas des modèles à recopier.

## Périmètre de cette première livraison
Cette étape pose les fondations de marque et la page d'entrée publique. Elle ne prétend pas encore fournir les services authentifiés, l'API, la base de données ni les intégrations opérationnelles : ces couches doivent être implémentées et testées avant toute mise en production.
