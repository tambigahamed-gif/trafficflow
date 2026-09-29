# TrafficFlow - SaaS Platform

Application SaaS moderne avec React, TypeScript, Express et MySQL.

## 🚀 Stack

- **Frontend**: React 19 + Vite + TypeScript
- **Backend**: Express + Node.js
- **Database**: MySQL + Drizzle ORM
- **API**: tRPC
- **UI**: Radix UI + Tailwind CSS
- **Auth**: Supabase
- **Deploy**: Vercel

## 📋 Prérequis

- Node.js 20+
- pnpm 10+
- MySQL 8+ (ou utiliser Supabase)

## 🔧 Installation locale

```bash
# 1. Cloner et installer
git clone https://github.com/tambigahamed-gif/trafficflow.git
cd trafficflow
pnpm install

# 2. Configurer les variables d'environnement
cp .env.example .env.local

# 3. Lancer en développement
pnpm run dev

# 4. Build
pnpm run build

# 5. Démarrer en production
pnpm run start
```

## 📁 Structure du projet

```
.
├── src/                    # Code frontend React
├── server/
│   └── _core/             # Backend Express
├── public/                # Assets statiques
├── dist/                  # Build output
├── vite.config.ts         # Configuration Vite
├── tsconfig.json          # TypeScript config
├── drizzle.config.ts      # Drizzle ORM config
└── vercel.json            # Vercel deployment config
```

## 🌐 Déploiement sur Vercel

### Étapes rapides :

1. **Push sur GitHub** ✅ (déjà fait)
2. **Aller sur** https://vercel.com
3. **New Project** → Sélectionner ce repo
4. **Environment Variables** (voir `.env.example`)
5. **Deploy**

### Variables d'environnement à configurer dans Vercel :

```
DATABASE_URL=mysql://...
VITE_SUPABASE_URL=https://...
VITE_SUPABASE_ANON_KEY=...
NEXT_PUBLIC_API_URL=https://votre-domain.vercel.app
AWS_ACCESS_KEY_ID=... (si nécessaire)
AWS_SECRET_ACCESS_KEY=... (si nécessaire)
```

## 🗄️ Base de données

### Option 1: Supabase PostgreSQL
- Créer un compte [Supabase](https://supabase.com)
- Copier les clés dans `.env.local`

### Option 2: PlanetScale MySQL
- Créer un account [PlanetScale](https://planetscale.com)
- Configurer DATABASE_URL

### Migrations
```bash
pnpm run db:push  # Applique les migrations
```

## 📚 Documentation

- [Vite docs](https://vitejs.dev)
- [React docs](https://react.dev)
- [Drizzle ORM](https://orm.drizzle.team)
- [tRPC](https://trpc.io)

## 📝 Scripts disponibles

```bash
pnpm run dev      # Dev mode
pnpm run build    # Production build
pnpm run start    # Run production server
pnpm run check    # TypeScript check
pnpm run test     # Run tests
pnpm run format   # Format code
```

## 🔐 Sécurité

- Toutes les variables sensibles en `.env`
- HTTPS en production
- CORS configuré sur Vercel
- Auth via Supabase

## 📞 Support

Pour les issues, ouvrez une [GitHub Issue](https://github.com/tambigahamed-gif/trafficflow/issues)

---

**Version**: 1.0.0  
**License**: MIT
