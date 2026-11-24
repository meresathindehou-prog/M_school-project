# UGENCY — fondation technique

Version de travail : 0.1 — 24 novembre 2026

## Architecture cible

```text
Clients web (Citoyen / Professionnel / Opérations / Admin)
                    |
             API Gateway / BFF
                    |
       NestJS — modules métier
       |       |       |       |
   Identity  Requests  Agenda  Operations
       |       |       |       |
       +-------+-------+-------+
                    |
       PostgreSQL (+ PostGIS si nécessaire)
                    |
             Redis / Workers
                    |
      Adaptateurs d'intégration externes
```

L'interface publique statique n'est pas le backend. Les applications authentifiées devront être déployées sur un environnement compatible serveur, avec une API séparée ou un BFF documenté.

## Modules et responsabilités

| Module | Responsabilité | Contrôle essentiel |
|---|---|---|
| Identity & Access | comptes, sessions, rôles, permissions | autorisation serveur par ressource |
| Profiles | profils citoyen et professionnel | consentement et minimisation |
| Service Directory | catalogue et orientation | validation des référentiels |
| Requests | création et suivi des demandes | propriétaire, statut et historique |
| Appointments | créneaux et rendez-vous | conflits et fuseaux horaires |
| Operations | affectation et coordination | journal des transitions |
| Notifications | email / SMS / push selon fournisseurs | consentement, reprise et idempotence |
| SOS | parcours prioritaire et escalade | disponibilité, accusé de réception, traçabilité |
| Administration | configuration et supervision | moindre privilège et audit |

## Contrats API initiaux

Préfixe versionné : /api/v1.

- POST /auth/register — création de compte avec validation.
- POST /auth/login — ouverture de session.
- POST /auth/refresh — rotation d'un jeton de renouvellement.
- POST /auth/logout — révocation de session.
- GET /me — identité et permissions effectives.
- GET /services — catalogue public.
- POST /requests — création d'une demande authentifiée.
- GET /requests/:id — lecture limitée au propriétaire ou à un rôle autorisé.
- POST /appointments — demande de rendez-vous.
- GET /appointments — rendez-vous accessibles au compte.
- POST /sos — déclenchement du parcours prioritaire, avec règles d'escalade à définir et tester.

Toutes les routes privées doivent vérifier l'identité, les permissions et la portée de la ressource côté serveur. Masquer un bouton dans l'interface ne constitue jamais une autorisation.

## Modèle de sécurité
- Mots de passe stockés sous forme de hash robuste (Argon2id recommandé), jamais en clair.
- Jetons de renouvellement stockés sous forme de hash, rotation et révocation.
- Cookies de session sécurisés en production : HttpOnly, Secure, SameSite adapté.
- Protection CSRF selon le mode d'authentification, limitation de débit et validation stricte des entrées.
- RBAC avec permissions granulaires et vérification au niveau des ressources.
- Journal d'audit append-only pour les actions sensibles ; ne pas y écrire de secrets.
- Secrets et clés uniquement via variables d'environnement / gestionnaire de secrets.
- Chiffrement en transit, sauvegardes chiffrées, politique de rétention et procédure de restauration.
- Collecte minimale des données ; aucune donnée sensible dans les logs applicatifs.

## Données
La migration backend/db/001_identity.sql pose le premier socle relationnel : utilisateurs, rôles, permissions, sessions et audit. Elle est une base de développement, pas une validation de conformité ni une base de production prête à recevoir des données réelles.

## Qualité et exploitation
Avant mise en service :
1. tests unitaires des règles métier ;
2. tests d'intégration PostgreSQL et Redis ;
3. tests d'autorisation négatifs (accès refusé entre comptes et rôles) ;
4. tests de contrat API et validation OpenAPI ;
5. tests E2E des parcours inscription, connexion, demande, rendez-vous et SOS ;
6. analyse de dépendances, lint, typecheck et build ;
7. monitoring, alertes, sauvegarde et exercice de restauration.

## Intégrations
Les fournisseurs de paiement, cartographie, SMS, email ou notifications ne seront branchés qu'après choix du fournisseur, vérification de ses API, gestion des secrets, traitement des erreurs et tests d'idempotence. Aucun fournisseur n'est déclaré intégré à ce stade.
