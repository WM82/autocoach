<<<<<<< HEAD
# 🚗 AutoCoach - Plateforme SaaS de Génération d'Annonces Automobiles

[![React](https://img.shields.io/badge/React-19-blue.svg)](https://reactjs.org/)
[![Flask](https://img.shields.io/badge/Flask-3.1-green.svg)](https://flask.palletsprojects.com/)
[![Supabase](https://img.shields.io/badge/Supabase-Database-orange.svg)](https://supabase.com/)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-purple.svg)](https://openai.com/)

AutoCoach est une plateforme SaaS moderne qui utilise l'intelligence artificielle pour générer automatiquement des annonces automobiles optimisées, maximisant les chances de vente au meilleur prix.

## ✨ Fonctionnalités

### 🔐 Authentification Sécurisée
- Inscription et connexion avec Supabase Auth
- Gestion des sessions JWT
- Protection des routes et données utilisateur

### 🚗 Génération d'Annonces IA
- Formulaire multi-étapes intuitif avec validation en temps réel
- Génération automatique via OpenAI GPT-4 Turbo
- Titres optimisés, descriptions persuasives, suggestions de photos
- Conseils de vente personnalisés

### 📊 Gestion Complète
- Historique des annonces générées
- Recherche et filtres avancés
- Statistiques d'utilisation
- Système de quotas par plan

### 🎨 Interface Moderne
- Design responsive (mobile-first)
- Composants shadcn/ui + TailwindCSS
- Animations fluides et micro-interactions
- Accessibilité optimisée (WCAG 2.1)

### 🔧 Services IA Avancés
- Analyse de documents Histovec
- Estimation des coûts mensuels
- Conseils de timing de vente
- Optimisation des prix

## 🏗️ Architecture

```
AutoCoach/
├── 🎨 Frontend (React + Vite)
│   ├── shadcn/ui + TailwindCSS
│   ├── React Router + Context API
│   └── Supabase Client
├── ⚙️ Backend (Flask + Python)
│   ├── API REST sécurisée
│   ├── Services IA (OpenAI)
│   └── Intégration Supabase
└── 🗄️ Base de données (Supabase)
    ├── Authentification
    ├── Stockage des données
    └── Row Level Security
```

## 🚀 Installation Rapide

### Prérequis
- Node.js 20+ et pnpm
- Python 3.11+ et pip
- Compte Supabase (gratuit)
- Clé API OpenAI

### 1. Configuration de la base de données

1. Créez un projet sur [Supabase](https://supabase.com)
2. Exécutez le script SQL :
```sql
-- Voir le fichier supabase-schema.sql pour le schéma complet
```

### 2. Configuration du Backend

```bash
cd autocoach-backend-v2
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou venv\Scripts\activate  # Windows

pip install -r requirements.txt

# Configuration des variables d'environnement
cp .env.example .env
# Éditez .env avec vos clés API
```

### 3. Configuration du Frontend

```bash
cd annonces-auto
pnpm install

# Configuration des variables d'environnement
cp .env.example .env
# Éditez .env avec vos URLs Supabase
```

### 4. Lancement en développement

```bash
# Terminal 1 - Backend
cd autocoach-backend-v2
source venv/bin/activate
python src/main.py

# Terminal 2 - Frontend
cd annonces-auto
pnpm run dev
```

L'application sera accessible sur `http://localhost:5173`

## 🔧 Configuration

### Variables d'environnement Backend (.env)
```env
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_KEY=your-service-role-key
OPENAI_API_KEY=your-openai-api-key
SECRET_KEY=your-secret-key
CORS_ORIGINS=http://localhost:5173
```

### Variables d'environnement Frontend (.env)
```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
VITE_API_URL=http://localhost:5000
```

## 📦 Déploiement

### Option 1: Déploiement Full-Stack (Recommandé)

```bash
# 1. Construire le frontend
cd annonces-auto
pnpm run build

# 2. Copier dans le backend
cp -r dist/* ../autocoach-backend-v2/src/static/

# 3. Déployer le backend (contient le frontend)
cd ../autocoach-backend-v2
# Déployer sur Railway, Heroku, ou votre plateforme préférée
```

### Option 2: Déploiement Séparé

**Frontend** (Vercel/Netlify):
```bash
cd annonces-auto
pnpm run build
# Déployer le dossier dist/
```

**Backend** (Railway/Heroku):
```bash
cd autocoach-backend-v2
# Configurer les variables d'environnement
# Déployer avec Python buildpack
```

## 🧪 Tests

### Frontend
```bash
cd annonces-auto
pnpm run test
pnpm run test:coverage
```

### Backend
```bash
cd autocoach-backend-v2
source venv/bin/activate
python -m pytest
python -m pytest --cov=src
```

## 📚 Documentation

- [📖 Documentation complète](./autocoach-documentation.md)
- [🗄️ Schéma de base de données](./annonces-auto/supabase-schema.sql)
- [🔌 Documentation API](./autocoach-backend-v2/docs/api.md)
- [🎨 Guide des composants](./annonces-auto/docs/components.md)

## 🛠️ Technologies Utilisées

### Frontend
- **React 19** - Framework UI moderne
- **Vite** - Build tool ultra-rapide
- **TailwindCSS** - Framework CSS utility-first
- **shadcn/ui** - Composants UI de haute qualité
- **React Router** - Routing côté client
- **Lucide Icons** - Icônes modernes

### Backend
- **Flask 3.1** - Framework web Python
- **Supabase** - Backend-as-a-Service
- **OpenAI API** - Intelligence artificielle
- **Flask-CORS** - Gestion CORS
- **python-dotenv** - Variables d'environnement

### Base de données
- **PostgreSQL** (via Supabase)
- **Row Level Security** - Sécurité au niveau des lignes
- **Real-time subscriptions** - Mises à jour en temps réel

## 🔒 Sécurité

- ✅ Authentification JWT sécurisée
- ✅ Row Level Security (RLS) activée
- ✅ Validation des données côté client et serveur
- ✅ Protection CORS configurée
- ✅ Sanitisation des entrées utilisateur
- ✅ Gestion sécurisée des erreurs

## 📊 Performance

- ⚡ Code splitting automatique
- ⚡ Lazy loading des composants
- ⚡ Cache des requêtes optimisé
- ⚡ Compression des assets
- ⚡ Optimisation des images

## 🤝 Contribution

1. Fork le projet
2. Créez votre branche feature (`git checkout -b feature/AmazingFeature`)
3. Committez vos changements (`git commit -m 'Add some AmazingFeature'`)
4. Push vers la branche (`git push origin feature/AmazingFeature`)
5. Ouvrez une Pull Request

### Standards de code
- **Frontend**: ESLint + Prettier
- **Backend**: Black + Flake8
- **Tests**: Jest (Frontend) + Pytest (Backend)
- **Commits**: Conventional Commits

## 📈 Roadmap

### Version 1.1 (Q2 2024)
- [ ] Analyse automatique de documents Histovec
- [ ] Estimation intelligente des coûts mensuels
- [ ] Conseils de timing de vente optimaux
- [ ] Templates d'annonces personnalisables

### Version 1.2 (Q3 2024)
- [ ] Intégration directe avec LeBonCoin/AutoScout24
- [ ] Analyse de photos de véhicules par IA
- [ ] Système de notifications push
- [ ] Analytics avancées et insights

### Version 2.0 (Q4 2024)
- [ ] Application mobile native (React Native)
- [ ] IA multimodale (texte + images)
- [ ] Marketplace intégrée
- [ ] API publique pour développeurs

## 📞 Support

- 📧 Email: support@autocoach.fr
- 💬 Discord: [Rejoindre la communauté](https://discord.gg/autocoach)
- 📖 Documentation: [docs.autocoach.fr](https://docs.autocoach.fr)
- 🐛 Issues: [GitHub Issues](https://github.com/autocoach/issues)

## 📄 Licence

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

## 🙏 Remerciements

- [OpenAI](https://openai.com/) pour l'API GPT-4
- [Supabase](https://supabase.com/) pour l'infrastructure backend
- [shadcn/ui](https://ui.shadcn.com/) pour les composants UI
- [Lucide](https://lucide.dev/) pour les icônes
- La communauté open source pour les outils fantastiques

---

<div align="center">
  <p>Fait avec ❤️ par l'équipe AutoCoach</p>
  <p>
    <a href="https://autocoach.fr">🌐 Site Web</a> •
    <a href="https://docs.autocoach.fr">📚 Documentation</a> •
    <a href="https://twitter.com/autocoach_fr">🐦 Twitter</a>
  </p>
</div>

=======
# autocoach
>>>>>>> 189065ed692da470a8f07593822c2445300fd6d1
