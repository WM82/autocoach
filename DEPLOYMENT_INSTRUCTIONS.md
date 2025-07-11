# 🚀 AutoCoach - Instructions de Déploiement Complètes

## 🎯 **DÉPLOIEMENT RECOMMANDÉ**

### **Frontend : Vercel (Recommandé #1)**

#### **Pourquoi Vercel ?**
- ✅ Support natif des SPA React
- ✅ Déploiement automatique depuis Git
- ✅ Configuration zero-config
- ✅ CDN global intégré
- ✅ Analytics intégrées

#### **Étapes de Déploiement**

1. **Préparation du Repository**
```bash
# Créer un nouveau repository Git
git init
git add .
git commit -m "Initial commit - AutoCoach project"

# Pousser vers GitHub
git remote add origin https://github.com/votre-username/autocoach.git
git push -u origin main
```

2. **Déploiement Vercel**
```bash
# Option 1: Via CLI
npm i -g vercel
vercel login
vercel

# Option 2: Via Dashboard
# 1. Aller sur vercel.com
# 2. Connecter GitHub
# 3. Importer le repository
# 4. Configurer les variables d'environnement
```

3. **Variables d'Environnement Vercel**
```
VITE_SUPABASE_URL=https://vaziudakazzkaayphude.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InZheml1ZGFrYXp6a2FheXBodWRlIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTIwNTMzMzEsImV4cCI6MjA2NzYyOTMzMX0.hkPZXog3_5lQazvIFvvksaiFrPbmr-w1BIDv_uZpxFI
VITE_API_URL=https://votre-api.railway.app
VITE_ENV=production
```

### **Frontend : Netlify (Alternative)**

#### **Déploiement Netlify**
```bash
# Via CLI
npm i -g netlify-cli
netlify login

# Build et déploiement
npm run build
netlify deploy --prod --dir=dist

# Le fichier _redirects est déjà configuré !
```

#### **Configuration Netlify**
```toml
# netlify.toml (optionnel)
[build]
  publish = "dist"
  command = "npm run build"

[build.environment]
  NODE_VERSION = "18"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

## 🔧 **BACKEND : Railway (Recommandé)**

### **Pourquoi Railway ?**
- ✅ Déploiement Python/FastAPI simple
- ✅ Base de données PostgreSQL intégrée
- ✅ Variables d'environnement sécurisées
- ✅ Scaling automatique

### **Déploiement Railway**

1. **Préparation**
```bash
# Créer Procfile
echo "web: uvicorn src.main:app --host 0.0.0.0 --port \$PORT" > Procfile

# Créer runtime.txt
echo "python-3.11" > runtime.txt
```

2. **Déploiement**
```bash
# Via CLI
npm i -g @railway/cli
railway login
railway init
railway up

# Via Dashboard
# 1. Aller sur railway.app
# 2. Connecter GitHub
# 3. Déployer le repository
```

3. **Variables d'Environnement Railway**
```
DATABASE_URL=postgresql://postgres:password@localhost:5432/autocoach
SUPABASE_URL=https://vaziudakazzkaayphude.supabase.co
SUPABASE_SERVICE_KEY=votre_service_key
HISTOVEC_API_KEY=votre_histovec_key
ARGUS_API_KEY=votre_argus_key
OPENAI_API_KEY=votre_openai_key
ENVIRONMENT=production
```

## 🗄 **BASE DE DONNÉES : Supabase**

### **Configuration Supabase**

1. **Créer le Projet**
   - Aller sur supabase.com
   - Créer un nouveau projet
   - Noter l'URL et les clés

2. **Exécuter le Schéma**
```sql
-- Copier le contenu de supabase-schema.sql
-- L'exécuter dans l'éditeur SQL Supabase

-- Tables principales
CREATE TABLE profiles (
  id UUID REFERENCES auth.users(id) PRIMARY KEY,
  email TEXT,
  full_name TEXT,
  plan TEXT DEFAULT 'free',
  analyses_used INTEGER DEFAULT 0,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE vehicle_analyses (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES profiles(id),
  license_plate TEXT,
  vin TEXT,
  analysis_data JSONB,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Activer RLS
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE vehicle_analyses ENABLE ROW LEVEL SECURITY;

-- Politiques
CREATE POLICY "Users can view own profile" ON profiles
  FOR SELECT USING (auth.uid() = id);

CREATE POLICY "Users can update own profile" ON profiles
  FOR UPDATE USING (auth.uid() = id);

CREATE POLICY "Users can view own analyses" ON vehicle_analyses
  FOR SELECT USING (auth.uid() = user_id);

CREATE POLICY "Users can insert own analyses" ON vehicle_analyses
  FOR INSERT WITH CHECK (auth.uid() = user_id);
```

3. **Configuration Auth**
   - Activer l'authentification email/password
   - Configurer les redirections
   - Personnaliser les emails

## 🔐 **CONFIGURATION DOMAINE PERSONNALISÉ**

### **Vercel**
```bash
# Ajouter un domaine
vercel domains add autocoach.com

# Configurer DNS
# A record: @ -> 76.76.19.61
# CNAME: www -> cname.vercel-dns.com
```

### **Netlify**
```bash
# Ajouter un domaine
netlify domains:add autocoach.com

# Configurer DNS selon les instructions Netlify
```

## 📊 **MONITORING ET ANALYTICS**

### **Sentry (Monitoring d'Erreurs)**
```bash
# Installation
npm install @sentry/react @sentry/tracing

# Configuration dans main.jsx
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: "YOUR_SENTRY_DSN",
  environment: import.meta.env.VITE_ENV,
});
```

### **PostHog (Analytics)**
```bash
# Installation
npm install posthog-js

# Configuration
import posthog from 'posthog-js'

posthog.init('YOUR_POSTHOG_KEY', {
  api_host: 'https://app.posthog.com'
})
```

## 🔄 **CI/CD AUTOMATIQUE**

### **GitHub Actions**
```yaml
# .github/workflows/deploy.yml
name: Deploy to Vercel

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '18'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Build
        run: npm run build
        
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
```

## 🧪 **TESTS ET QUALITÉ**

### **Tests Frontend**
```bash
# Installation
npm install -D vitest @testing-library/react @testing-library/jest-dom

# Configuration vitest.config.js
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    setupFiles: ['./src/test/setup.js'],
  },
})

# Lancer les tests
npm run test
```

### **Tests Backend**
```bash
# Installation
pip install pytest pytest-asyncio httpx

# Lancer les tests
pytest

# Avec coverage
pytest --cov=src
```

## 🚀 **DÉPLOIEMENT COMPLET - CHECKLIST**

### **Pré-déploiement**
- [ ] Code testé localement
- [ ] Variables d'environnement configurées
- [ ] Base de données Supabase configurée
- [ ] Repository Git créé et poussé

### **Frontend**
- [ ] Déployé sur Vercel/Netlify
- [ ] Domaine personnalisé configuré
- [ ] HTTPS activé
- [ ] Variables d'environnement définies
- [ ] Navigation testée

### **Backend**
- [ ] Déployé sur Railway/Heroku
- [ ] Base de données connectée
- [ ] APIs externes configurées
- [ ] CORS configuré
- [ ] Documentation API accessible

### **Post-déploiement**
- [ ] Tests E2E passés
- [ ] Monitoring configuré
- [ ] Analytics activées
- [ ] Sauvegardes configurées
- [ ] SSL/TLS vérifié

## 🆘 **DÉPANNAGE COURANT**

### **Problème : Routes 404**
```bash
# Solution : Vérifier _redirects ou configuration serveur
# Vercel : Automatique
# Netlify : _redirects configuré
# Autres : Configurer try_files
```

### **Problème : Variables d'environnement**
```bash
# Vérifier le préfixe VITE_
# Redéployer après modification
# Vérifier la casse et l'orthographe
```

### **Problème : CORS**
```python
# Backend FastAPI
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://votre-frontend.vercel.app"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

**🎉 Avec ces instructions, vous pouvez déployer AutoCoach en production !**

