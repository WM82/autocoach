# AutoCoach - Documentation Complète

## Vue d'ensemble

AutoCoach est une plateforme SaaS complète pour la création d'annonces automobiles optimisées par IA. Elle permet aux utilisateurs de générer automatiquement des annonces professionnelles et attractives pour maximiser leurs chances de vente.

## Architecture

### Frontend (React)
- **Framework**: React 19 avec Vite
- **UI**: shadcn/ui + TailwindCSS
- **Routing**: React Router DOM
- **Authentification**: Supabase Auth
- **État**: React Context API

### Backend (Flask)
- **Framework**: Flask avec Python 3.11
- **Base de données**: Supabase (PostgreSQL)
- **IA**: OpenAI GPT-4 Turbo
- **Authentification**: JWT via Supabase
- **CORS**: Flask-CORS

### Base de données (Supabase)
- **Profiles**: Profils utilisateurs
- **Vehicle_ads**: Annonces de véhicules
- **Generation_logs**: Logs des générations IA
- **User_subscriptions**: Abonnements utilisateurs

## Fonctionnalités

### 🔐 Authentification
- Inscription avec email/mot de passe
- Connexion sécurisée
- Gestion des sessions
- Protection des routes

### 🚗 Génération d'annonces
- Formulaire multi-étapes intuitif
- Validation en temps réel
- Génération IA optimisée
- Prévisualisation interactive

### 📊 Gestion des annonces
- Historique complet
- Recherche et filtres
- Statistiques d'utilisation
- Copie et partage faciles

### 🎨 Interface utilisateur
- Design moderne et responsive
- Animations fluides
- Accessibilité optimisée
- Support mobile complet

## Installation et Configuration

### Prérequis
- Node.js 20+
- Python 3.11+
- Compte Supabase
- Clé API OpenAI

### Frontend Setup

```bash
cd annonces-auto
pnpm install
cp .env.example .env
# Configurer les variables d'environnement
pnpm run dev
```

### Backend Setup

```bash
cd autocoach-backend-v2
python -m venv venv
source venv/bin/activate  # Linux/Mac
pip install -r requirements.txt
cp .env.example .env
# Configurer les variables d'environnement
python src/main.py
```

### Variables d'environnement

#### Frontend (.env)
```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
VITE_API_URL=http://localhost:5000
```

#### Backend (.env)
```env
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_KEY=your-service-role-key
OPENAI_API_KEY=your-openai-api-key
SECRET_KEY=your-secret-key
CORS_ORIGINS=http://localhost:5173,http://localhost:3000
```

### Configuration Supabase

1. Créer un nouveau projet Supabase
2. Exécuter le script SQL dans `supabase-schema.sql`
3. Configurer l'authentification email
4. Récupérer les clés API

## Structure du projet

### Frontend
```
annonces-auto/
├── src/
│   ├── components/          # Composants réutilisables
│   │   ├── ui/             # Composants shadcn/ui
│   │   ├── VehicleAdForm.jsx
│   │   ├── AdPreview.jsx
│   │   ├── AdHistory.jsx
│   │   └── ProtectedRoute.jsx
│   ├── contexts/           # Contextes React
│   │   └── AuthContext.jsx
│   ├── lib/               # Utilitaires
│   │   └── supabase.js
│   ├── pages/             # Pages principales
│   │   ├── Login.jsx
│   │   ├── Signup.jsx
│   │   └── Dashboard.jsx
│   └── App.jsx
├── public/
└── package.json
```

### Backend
```
autocoach-backend-v2/
├── src/
│   ├── routes/            # Routes API
│   │   ├── ads.py
│   │   └── user.py
│   ├── services/          # Services métier
│   │   ├── supabase_service.py
│   │   └── openai_service.py
│   ├── utils/             # Utilitaires
│   │   ├── auth.py
│   │   └── validation.py
│   ├── models/            # Modèles de données
│   ├── config.py          # Configuration
│   └── main.py           # Point d'entrée
└── requirements.txt
```

## API Endpoints

### Authentification
- `POST /api/auth/login` - Connexion
- `POST /api/auth/signup` - Inscription
- `POST /api/auth/logout` - Déconnexion

### Annonces
- `POST /api/generate-ad` - Générer une annonce
- `GET /api/ads` - Lister les annonces utilisateur
- `GET /api/ads/{id}` - Récupérer une annonce
- `PUT /api/ads/{id}` - Modifier une annonce
- `DELETE /api/ads/{id}` - Supprimer une annonce

### Logs et statistiques
- `GET /api/logs` - Historique des générations
- `GET /api/subscription` - Informations d'abonnement

### Services IA
- `POST /api/analyze-histovec` - Analyser un document Histovec
- `POST /api/estimate-costs` - Estimer les coûts mensuels

## Schéma de base de données

### Profiles
```sql
CREATE TABLE profiles (
  id UUID PRIMARY KEY REFERENCES auth.users(id),
  email TEXT UNIQUE NOT NULL,
  full_name TEXT,
  avatar_url TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### Vehicle_ads
```sql
CREATE TABLE vehicle_ads (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES auth.users(id) NOT NULL,
  model TEXT NOT NULL,
  mileage INTEGER NOT NULL,
  fuel_type TEXT NOT NULL,
  owners INTEGER NOT NULL,
  condition TEXT NOT NULL,
  features TEXT,
  maintenance TEXT,
  price DECIMAL(10,2) NOT NULL,
  location TEXT NOT NULL,
  generated_title TEXT,
  generated_description TEXT,
  generated_suggestions TEXT,
  status TEXT DEFAULT 'active',
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### Generation_logs
```sql
CREATE TABLE generation_logs (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES auth.users(id) NOT NULL,
  ad_id UUID REFERENCES vehicle_ads(id),
  request_data JSONB NOT NULL,
  response_data TEXT,
  status TEXT NOT NULL,
  error_message TEXT,
  duration_ms INTEGER,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### User_subscriptions
```sql
CREATE TABLE user_subscriptions (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES auth.users(id) NOT NULL,
  plan_type TEXT DEFAULT 'free',
  ads_limit INTEGER DEFAULT 5,
  ads_used INTEGER DEFAULT 0,
  expires_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

## Sécurité

### Authentification
- JWT tokens via Supabase
- Row Level Security (RLS) activée
- Validation des tokens côté backend
- Protection CSRF

### Validation des données
- Validation côté client et serveur
- Sanitisation des entrées utilisateur
- Limites de taux d'API
- Gestion des erreurs sécurisée

### CORS
- Configuration restrictive
- Origins autorisées uniquement
- Headers sécurisés

## Performance

### Frontend
- Code splitting automatique
- Lazy loading des composants
- Optimisation des images
- Cache des requêtes

### Backend
- Connexions de base de données poolées
- Cache des réponses IA
- Limitation du taux de requêtes
- Monitoring des performances

## Déploiement

### Frontend (Vercel/Netlify)
```bash
pnpm run build
# Déployer le dossier dist/
```

### Backend (Railway/Heroku)
```bash
# Configurer les variables d'environnement
# Déployer avec Docker ou buildpack Python
```

### Base de données
- Supabase hébergé
- Sauvegardes automatiques
- Monitoring intégré

## Monitoring et Logs

### Logs d'application
- Toutes les générations IA loggées
- Erreurs trackées avec contexte
- Métriques de performance

### Monitoring utilisateur
- Utilisation des quotas
- Statistiques d'engagement
- Feedback utilisateur

## Tests

### Frontend
```bash
pnpm run test
```

### Backend
```bash
python -m pytest
```

### Tests d'intégration
- Tests API complets
- Tests de bout en bout
- Tests de charge

## Maintenance

### Mises à jour
- Dépendances sécurisées
- Migrations de base de données
- Déploiements sans interruption

### Sauvegarde
- Base de données quotidienne
- Code versionné (Git)
- Configuration externalisée

## Support

### Documentation utilisateur
- Guide d'utilisation intégré
- FAQ interactive
- Tutoriels vidéo

### Support technique
- Logs détaillés pour debug
- Monitoring proactif
- Alertes automatiques

## Roadmap

### Version 1.1
- [ ] Analyse de documents Histovec
- [ ] Estimation des coûts mensuels
- [ ] Conseils de vente personnalisés

### Version 1.2
- [ ] Intégration plateformes de vente
- [ ] Templates d'annonces personnalisables
- [ ] Analytics avancées

### Version 2.0
- [ ] Application mobile
- [ ] IA multimodale (photos)
- [ ] Marketplace intégrée

## Contribution

### Guidelines
1. Fork le repository
2. Créer une branche feature
3. Commits atomiques et descriptifs
4. Tests pour nouvelles fonctionnalités
5. Pull request avec description détaillée

### Standards de code
- ESLint + Prettier (Frontend)
- Black + Flake8 (Backend)
- Tests unitaires obligatoires
- Documentation des APIs

---

**AutoCoach** - Créé avec ❤️ pour simplifier la vente de véhicules d'occasion.

