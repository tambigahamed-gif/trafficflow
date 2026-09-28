# TrafficFlow V2 — guide de lancement

Cette archive correspond au **snapshot V2 validé** (`d2206ca50d4af7bab7e8d9722b4b422292af4498`). Elle est séparée des ajouts V3 en cours afin que les références manquantes de V3 ne bloquent pas cette version.

## Fonctionnalités V2

- Authentification par e-mail via Supabase Auth et JWT.
- Workspaces multi-tenant, rôles Owner/Admin/Manager/VA, invitations, affectations et permissions.
- RLS Supabase pour isoler les données des Workspaces.
- Comptes sociaux, contenus, brouillons, médiathèque, calendrier, analytics et notifications V2.
- Assistants IA et génération de contenu avec l’intégration IA existante du projet.
- Interface française, responsive, thèmes clair/sombre et accessibilité clavier.
- Plans et quotas applicatifs. **Aucun paiement réel n’est activé dans cette version.**

## Prérequis

- Node.js compatible avec le projet (Node 22 conseillé) et pnpm.
- Projet Supabase avec Auth e-mail et les migrations V2 présentes dans `supabase/migrations/`.
- Les clés de votre propre projet Supabase. Elles ne sont volontairement pas incluses dans l’archive.

## Installation locale

1. Décompressez l’archive et ouvrez un terminal dans le dossier `trafficflow`.
2. Installez les dépendances : `pnpm install --frozen-lockfile`.
3. Créez localement un fichier `.env` non versionné et renseignez au minimum les variables suivantes avec les valeurs de votre projet Supabase :

   ```dotenv
   VITE_SUPABASE_URL=https://your-project.supabase.co
   VITE_SUPABASE_PUBLISHABLE_KEY=your-supabase-publishable-or-anon-key
   ```
4. Dans Supabase SQL Editor, appliquez les migrations V2 dans l’ordre `0001` à `0009` (ne réappliquez pas une migration déjà exécutée).
5. Vérifiez et démarrez : `pnpm check`, `pnpm test`, `pnpm build`, puis `pnpm dev`.
6. Ouvrez l’URL locale affichée par le serveur, habituellement `http://localhost:3000`.

## Configuration et sécurité

- Ne partagez pas votre `.env` et ne commitez pas les clés. Utilisez les variables configurées par votre hébergeur en production.
- Les clés de connexion à Supabase et les données déjà hébergées dans votre projet Supabase ne sont pas exportées dans cette archive.
- V2 utilise le fournisseur IA déjà intégré au projet; Gemini/Groq et PayDunya sont des ajouts prévus pour V3, non compris ici.
- Les autorisations Meta, TikTok, X et YouTube nécessitent des applications développeur, des secrets, des URI de rappel publiques et l’approbation des scopes. La connexion OAuth de production n’est pas incluse dans V2.
- Pour déployer, configurez les variables d’environnement dans l’hébergeur puis vérifiez l’URL de retour Supabase Auth.

## Vérifications

Les scripts définis dans `package.json` sont `pnpm check`, `pnpm test` et `pnpm build`. Le snapshot V2 a été précédemment enregistré comme validé avec TypeScript, les tests, le build et des contrôles de prévisualisation. Exécutez ces commandes après avoir configuré votre environnement local.
