# TrafficFlow V5 — livraison et préparation du déploiement

Ce document complète le guide V2 sans le remplacer. La livraison fait évoluer le projet existant : les workspaces, utilisateurs, rôles, permissions, paiements et données V2 ne sont ni réinitialisés ni remplacés.

## Ajouts livrés

- Landing française avec sections problème/solution, fonctionnalités, Link Pages, analytics, automatisations, réseaux, offres/limites, FAQ, CTA et footer légal. Les chiffres d’illustration sont explicitement étiquetés comme fictifs.
- Inscription et connexion pointent vers les parcours Supabase Auth existants; les liens d’essai de sept jours créent un workspace au moyen du flux V2 existant.
- Routes légales `/privacy`, `/terms`, `/data-deletion`; `/legal/privacy` et `/legal/terms` restent compatibles.
- Dashboard workspace : membres, comptes, automatisations, Link Pages, vues/clics/CTR, statut d’abonnement/essai et dernière notification visible par l’utilisateur.
- Link Pages : créer, modifier, dupliquer, publier/dépublier, désactiver, retirer avec conservation de l’historique, slug unique, lien public, aperçu, modèles, profil, couleurs, image de fond, typographie, espacement, réordonnancement et QR PNG.
- Link Manager : créer, dupliquer, modifier, copier une URL courte active, activer/désactiver, retirer en conservant l’historique, associer/réutiliser un lien sur plusieurs pages, voir pages associées et dates de création/modification.
- Analytics de première partie : vues, visiteurs pseudonymisés, impressions, clics/CTR, évolution quotidienne, pages/liens, appareils, pays si fournis et variantes A/B. Périodes : aujourd’hui, 7/30 jours et dates personnalisées. Pas de fournisseur GeoIP requis; `country_code` reste nul par défaut.
- Redirection `/r/:code` : validation côté serveur, limitation de débit, `Referrer-Policy: no-referrer`, vérification DNS/IP contre les destinations privées et fournisseur GeoIP optionnel/non bloquant.
- Handler Vercel réutilisant les routes Express/tRPC V2 et la redirection; serveur local et mode serverless partagent `createApp()`.

## Préserver les données et appliquer la migration

1. **Sauvegardez** le projet Supabase selon votre procédure habituelle.
2. Confirmez que les migrations `0001` à `0009` sont déjà appliquées. Ne les réexécutez pas.
3. Appliquez une fois `supabase/migrations/0010_link_pages_v5.sql` avec le mécanisme de migration de votre projet. La migration ajoute les objets V5 et les colonnes de limites; elle ne contient pas de `DROP TABLE`, `TRUNCATE` ou réinitialisation des workspaces.
4. Vérifiez les nouvelles tables/RPC/RLS avant d’activer la publication de Link Pages.

La migration ajoute des limites configurables aux codes de plans existants; elle ne change ni prix, ni abonnement courant, ni code de plan :

| Plan existant | Pages | Liens réutilisables | Rétention analytics |
| --- | ---: | ---: | ---: |
| Starter | 1 | 10 | 7 jours |
| Pro | 5 | 50 | 30 jours |
| Business | 25 | 250 | 90 jours |
| Enterprise | 100 | 1 000 | 365 jours |

Le trial de sept jours concerne les nouveaux workspaces selon le trigger V2/V5 fourni. Aucun paiement réel n’est branché. Le nettoyage des vieux événements se fait lors d’une lecture analytics autorisée; aucun ordonnanceur de purge de fond n’est inclus.

Les nouvelles ressources s’appuient sur les rôles V2 (owner/admin/manager/VA), RLS et l’accès délégué par page. Ne placez jamais de clé `service_role` dans le navigateur.

## Déploiement Vercel

1. Importez le dossier projet dans Vercel et choisissez **Node.js 22** si l’option est proposée.
2. `vercel.json` définit `pnpm install --frozen-lockfile`, `pnpm build`, la sortie Vite `dist/public`, les replis SPA, le catch-all `/api/*` et le passage de `/r/:code` vers `/api/r/:code`.
3. Renseignez `VITE_SUPABASE_URL` et `VITE_SUPABASE_PUBLISHABLE_KEY` dans Production, Preview et Development selon le besoin. La clé navigateur doit être publishable/anon et protégée par RLS. **Aucune clé service-role dans `VITE_*`.** Toute clé privée reste côté serveur.
4. Dans Supabase Auth, autorisez l’URL du domaine et les retours `/dashboard`, ainsi que les URLs locales utiles.
5. Après déploiement, vérifiez `/`, `/signup`, `/login`, `/privacy`, `/terms`, `/legal/privacy`, `/legal/terms`, `/data-deletion`, `/dashboard`, `/link-pages`, `/p/:slug`, `/r/:code` et un appel authentifié `/api/trpc/...`.
6. Testez les emails de confirmation/récupération, l’expiration des liens et les politiques RLS avec chaque rôle sur le projet réel.

**Limite de vérification :** l’archive inclut le manifeste et l’adaptateur, et le serveur local a répondu aux routes statiques, de redirection et tRPC. Aucun projet Supabase, domaine, secret, déploiement Vercel ou migration externe n’a été fourni ou modifié. Le comportement final doit être validé sur vos propres environnements avant ouverture publique.

## Variables et démarrage locaux

Copiez `.env.example` vers `.env` **hors du dépôt** et renseignez les valeurs de votre projet. L’archive ne contient aucun secret. La clé Supabase publishable et la clé de carte `VITE_FRONTEND_FORGE_API_KEY` sont des valeurs client publiques; restreignez la clé cartographique par domaine et API. Les variables privées/API/JWT/DB doivent rester côté serveur. Le script Umami placeholder V2 a été retiré; aucun endpoint tiers d’analytics n’est chargé par défaut.

```bash
pnpm install --frozen-lockfile
pnpm dev
```

Vérification avant livraison :

```bash
pnpm check
pnpm test
pnpm build
```

Un test de santé Auth Supabase est ignoré si aucun projet/clé n’est configuré; lorsqu’ils existent, il vérifie la clé public et l’endpoint Auth. Les tests et le parseur SQL local ne se substituent pas à une exécution des migrations ni à des tests RLS sur le vrai projet.

## Intégrations et automatisations — état réel

Les workflows et historiques V2 restent en place; cette livraison ne les remplace pas et n’ajoute aucun worker ni tâche planifiée. Telegram, Discord, Zapier et les webhooks ne sont **pas connectés**. Les comptes sociaux existants peuvent contenir des références saisies manuellement; les flux OAuth officiels et la publication automatique ne sont pas configurés. Threads est prévu via l’écosystème Meta, mais pas connecté; X affiche « Bientôt disponible ». Aucun mot de passe de réseau social ne doit être demandé.

La migration réserve une table RLS `link_page_tracking_settings` avec des champs de validation pour Meta Pixel/TikTok Pixel. **Aucun formulaire de configuration, chargement de pixel, consentement ou envoi d’événements publicitaires n’est livré.** Aucun script tiers de pixel n’est donc exécuté. Il faudra ajouter consentement, contrôles serveur et tests de confidentialité avant de rendre ces intégrations actives.

## Pages légales à finaliser

Les pages `/privacy`, `/terms` (alias `/legal/privacy`, `/legal/terms`) et `/data-deletion` sont des projets transparents, **pas des avis juridiques publiables tels quels**. Avant lancement, renseignez identité de l’exploitant, adresse de contact vérifiée, région Supabase, délais de réponse/conservation, destinataires/prestataires et juridiction, puis ajoutez une vraie procédure d’effacement. Cette version n’expose pas la suppression autonome définitive d’un compte ou workspace.
