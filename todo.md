# TrafficFlow V2 — checklist de livraison et limites connues

## Fonctionnalités livrées
- [x] Supabase Auth e-mail : inscription, connexion, récupération et changement de mot de passe.
- [x] JWT Supabase vérifié par le serveur; la clé du navigateur est publishable, sans `service_role`.
- [x] Workspaces, memberships, profils, rôles et données métier migrés dans le projet Supabase choisi.
- [x] Audit des policies : RLS activée sur les 25 tables publiques; membres actifs uniquement; règles de rôle et de permission pour owner/admin/manager/VA, avec contrôle des affectations par compte.
- [x] Invitations à usage unique, rôles d’équipe, affectations, tâches, automatisations configurables, notifications et journal d’activité.
- [x] Limites Starter/Pro/Business/Enterprise conformes au registre Supabase; changement de plan demandé mais sans débit.
- [x] Import V1 volontaire, idempotent et limité à l’e-mail du compte; pas de secret social copié ni suppression de la source.
- [x] Comptes sociaux en état « à connecter », assistant IA avec quota, brouillons/programmes, calendrier filtrable, médias privés, dossiers, tags et audit.
- [x] Analytics et notifications reflètent les données enregistrées; aucun résultat, publication ou état de connexion fictif.
- [x] Interface française, responsive, routes directes, thèmes clair/sombre, formulaires associés à leurs labels, focus clavier et préférence de réduction des mouvements.
- [x] TypeScript, 4 tests Vitest, build production, preview desktop/mobile et console navigateur vérifiés; checkpoint créé.

## Dépendances externes — examinées, non activées
Ces points ne sont pas présentés comme des fonctionnalités en service. Leur état « terminé » correspond à l’analyse, la documentation et aux garde-fous UI/API; leur activation exige une configuration ultérieure du propriétaire.
- [x] OAuth des réseaux : Meta/Instagram, TikTok, X, YouTube/Google et LinkedIn sont explicitement « à connecter ». Les identifiants d’app et permissions officielles ne sont pas fournis.
- [x] Paiements : aucun fournisseur n’est configuré; l’interface n’effectue aucune charge et n’active pas automatiquement un nouveau plan.
- [x] Publication/workflows récurrents : les règles sont stockées et affichées, mais leur exécution automatique demeure désactivée en l’absence d’un worker durable compatible avec l’hébergement autoscale.
- [x] Analytics/inbox sociales : pas de lecture annoncée avant configuration OAuth et des autorisations API correspondantes.
- [x] Lancement e-mail/domaine : les invitations sont transmises manuellement par lien et les URL Auth de production devront être ajoutées à Supabase après choix du domaine.

## Vérification reproductible
`pnpm check`, `pnpm test` et `pnpm build` passent. La suite comporte 3 fichiers et 4 tests. Le seul avertissement est la taille du bundle navigateur (au-delà de 500 kB). Le parcours a été inspecté sans saisir de compte client réel; les redirections de production doivent être configurées avant les tests d’inscription/récupération en production.
