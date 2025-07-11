# 🚀 AutoCoach - Guide d'Accès Complet au Projet

## 📋 **STRUCTURE DU PROJET**

### **Frontend React** (`/autocoach-frontend/`)
```
autocoach-frontend/
├── public/
│   ├── _redirects              # Configuration SPA
│   └── index.html
├── src/
│   ├── components/             # Composants React
│   │   ├── Header.jsx
│   │   ├── Footer.jsx
│   │   ├── Dashboard.jsx
│   │   ├── VehicleAnalysisWizard.jsx
│   │   ├── AnalysisResults.jsx
│   │   ├── MarketTrends.jsx
│   │   └── ProtectedRoute.jsx
│   ├── pages/                  # Pages principales
│   │   ├── Login.jsx
│   │   ├── Signup.jsx
│   │   └── Upgrade.jsx
│   ├── contexts/               # Contextes React
│   │   ├── AuthContext.jsx
│   │   └── AnalysisContext.jsx
│   ├── lib/                    # Utilitaires
│   │   ├── supabase.js
│   │   ├── mockAuth.js
│   │   └── api.js
│   ├── App.jsx                 # Composant principal
│   ├── App.css                 # Styles globaux
│   └── main.jsx               # Point d'entrée
├── .env                        # Variables d'environnement
├── package.json               # Dépendances
├── vite.config.js             # Configuration Vite
└── tailwind.config.js         # Configuration Tailwind
```

### **Backend Python** (`/autocoach-advanced/`)
```
autocoach-advanced/
├── src/
│   ├── models/
│   │   └── vehicle.py
│   ├── services/
│   │   ├── vehicle_history_service.py
│   │   ├── market_analysis_service.py
│   │   ├── vehicle_assessment_service.py
│   │   ├── ai_listing_service.py
│   │   └── vehicle_analysis_orchestrator.py
│   ├── routes/
│   │   └── vehicle_analysis.py
│   └── main.py                # Point d'entrée FastAPI
├── requirements.txt           # Dépendances Python
└── README.md
```

## 🔑 **VARIABLES D'ENVIRONNEMENT**

### **Frontend (.env)**
```env
# Supabase Configuration
VITE_SUPABASE_URL=https://vaziudakazzkaayphude.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InZheml1ZGFrYXp6a2FheXBodWRlIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTIwNTMzMzEsImV4cCI6MjA2NzYyOTMzMX0.hkPZXog3_5lQazvIFvvksaiFrPbmr-w1BIDv_uZpxFI

# API Configuration
VITE_API_URL=http://localhost:8000

# Environment
VITE_ENV=development
```

### **Backend (à créer)**
```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/autocoach

# Supabase
SUPABASE_URL=https://vaziudakazzkaayphude.supabase.co
SUPABASE_SERVICE_KEY=your_service_key_here

# External APIs
HISTOVEC_API_KEY=your_histovec_key
ARGUS_API_KEY=your_argus_key

# AI Services
OPENAI_API_KEY=your_openai_key

# Environment
ENVIRONMENT=development
DEBUG=true
```

## 🛠 **TECHNOLOGIES UTILISÉES**

### **Frontend**
- **React 18** avec Vite
- **Tailwind CSS** pour le styling
- **Framer Motion** pour les animations
- **React Router** pour la navigation
- **Lucide React** pour les icônes
- **Supabase** pour l'authentification

### **Backend**
- **FastAPI** (Python)
- **Pydantic** pour la validation
- **SQLAlchemy** pour l'ORM
- **PostgreSQL** comme base de données
- **Supabase** pour l'auth et la DB

## 📦 **INSTALLATION ET CONFIGURATION**

### **1. Cloner le Projet**
```bash
# Créer le répertoire du projet
mkdir autocoach-project
cd autocoach-project

# Les fichiers sont disponibles dans le sandbox Manus
# Vous devrez les télécharger depuis /home/ubuntu/autocoach-frontend/ et /home/ubuntu/autocoach-advanced/
```

### **2. Configuration Frontend**
```bash
cd autocoach-frontend

# Installer les dépendances
npm install

# Configurer les variables d'environnement
cp .env.example .env
# Éditer .env avec vos clés

# Démarrer en développement
npm run dev

# Build pour production
npm run build
```

### **3. Configuration Backend**
```bash
cd autocoach-advanced

# Créer un environnement virtuel
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou venv\Scripts\activate  # Windows

# Installer les dépendances
pip install -r requirements.txt

# Configurer les variables d'environnement
cp .env.example .env
# Éditer .env avec vos clés

# Démarrer le serveur
uvicorn src.main:app --reload
```

## 🗄 **CONFIGURATION SUPABASE**

### **Tables Principales**
```sql
-- Utilisateurs (géré par Supabase Auth)
-- Profils utilisateurs
CREATE TABLE profiles (
  id UUID REFERENCES auth.users(id) PRIMARY KEY,
  email TEXT,
  full_name TEXT,
  plan TEXT DEFAULT 'free',
  analyses_used INTEGER DEFAULT 0,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Analyses de véhicules
CREATE TABLE vehicle_analyses (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES profiles(id),
  license_plate TEXT,
  vin TEXT,
  analysis_data JSONB,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### **Politiques RLS (Row Level Security)**
```sql
-- Activer RLS
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE vehicle_analyses ENABLE ROW LEVEL SECURITY;

-- Politiques pour profiles
CREATE POLICY "Users can view own profile" ON profiles
  FOR SELECT USING (auth.uid() = id);

CREATE POLICY "Users can update own profile" ON profiles
  FOR UPDATE USING (auth.uid() = id);

-- Politiques pour vehicle_analyses
CREATE POLICY "Users can view own analyses" ON vehicle_analyses
  FOR SELECT USING (auth.uid() = user_id);

CREATE POLICY "Users can insert own analyses" ON vehicle_analyses
  FOR INSERT WITH CHECK (auth.uid() = user_id);
```

## 🚀 **DÉPLOIEMENT**

### **Frontend - Options Recommandées**

#### **Option 1: Vercel (Recommandé)**
```bash
# Installer Vercel CLI
npm i -g vercel

# Déployer
vercel

# Configuration automatique pour React SPA
# Vercel gère automatiquement les redirections
```

#### **Option 2: Netlify**
```bash
# Installer Netlify CLI
npm i -g netlify-cli

# Build
npm run build

# Déployer
netlify deploy --prod --dir=dist

# Le fichier _redirects est déjà configuré
```

#### **Option 3: Configuration manuelle**
```nginx
# Configuration Nginx pour SPA
location / {
  try_files $uri $uri/ /index.html;
}
```

### **Backend - Options Recommandées**

#### **Option 1: Railway**
```bash
# Installer Railway CLI
npm i -g @railway/cli

# Login et déployer
railway login
railway init
railway up
```

#### **Option 2: Heroku**
```bash
# Créer Procfile
echo "web: uvicorn src.main:app --host 0.0.0.0 --port \$PORT" > Procfile

# Déployer
heroku create autocoach-api
git push heroku main
```

## 🔧 **DÉVELOPPEMENT ET MAINTENANCE**

### **Scripts Utiles**

#### **Frontend**
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint src --ext js,jsx",
    "format": "prettier --write src/**/*.{js,jsx}"
  }
}
```

#### **Backend**
```bash
# Tests
pytest

# Linting
flake8 src/

# Formatage
black src/

# Migration base de données
alembic upgrade head
```

### **Workflow de Développement Recommandé**

1. **Branches Git**
   - `main` : Production
   - `develop` : Développement
   - `feature/*` : Nouvelles fonctionnalités

2. **Tests**
   - Tests unitaires frontend (Jest/Vitest)
   - Tests API backend (pytest)
   - Tests E2E (Playwright/Cypress)

3. **CI/CD**
   - GitHub Actions pour l'intégration continue
   - Déploiement automatique sur merge

## 🔐 **SÉCURITÉ**

### **Authentification**
- Supabase Auth avec JWT
- Row Level Security (RLS)
- Validation côté client et serveur

### **API**
- CORS configuré
- Rate limiting
- Validation des entrées avec Pydantic

### **Variables Sensibles**
- Utiliser des services de secrets (Vercel Secrets, Railway Variables)
- Ne jamais commiter les clés dans Git
- Rotation régulière des clés API

## 📊 **MONITORING ET ANALYTICS**

### **Recommandations**
- **Sentry** pour le monitoring d'erreurs
- **PostHog** pour l'analytics utilisateur
- **Supabase Dashboard** pour la base de données
- **Vercel Analytics** pour les performances

## 🎯 **PROCHAINES ÉTAPES**

### **Fonctionnalités à Développer**
1. **Intégration APIs Externes**
   - Histovec pour l'historique
   - Argus pour l'évaluation
   - APIs constructeurs

2. **IA et Machine Learning**
   - Évaluation automatique de l'état
   - Génération d'annonces optimisées
   - Prédiction de prix

3. **Monétisation**
   - Intégration Stripe
   - Gestion des abonnements
   - Facturation automatique

4. **Fonctionnalités Avancées**
   - Export PDF des rapports
   - Notifications en temps réel
   - Dashboard analytics

## 📞 **SUPPORT TECHNIQUE**

Pour toute question technique ou assistance :
- Documentation Supabase : https://supabase.com/docs
- Documentation Vite : https://vitejs.dev/
- Documentation FastAPI : https://fastapi.tiangolo.com/

---

**🎉 Vous avez maintenant tous les éléments pour prendre le contrôle technique complet du projet AutoCoach !**

