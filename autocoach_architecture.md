# AutoCoach - Architecture Technique

## Vue d'ensemble

AutoCoach est une application SaaS moderne conçue pour les propriétaires de voitures d'occasion en France. L'application analyse les coûts, la santé des véhicules, et fournit des conseils de revente intelligents.

## Stack Technologique

### Backend
- **Framework**: Flask (Python 3.11)
- **Base de données**: SQLite (pour le MVP, évolutif vers PostgreSQL)
- **API**: RESTful avec Flask-RESTful
- **Authentification**: Magic link par email
- **IA**: OpenAI GPT-4 pour l'analyse et la génération de contenu
- **OCR**: Tesseract ou API cloud pour l'analyse de documents

### Frontend
- **Framework**: React 18 avec Create React App
- **Styling**: CSS Modules + Tailwind CSS
- **State Management**: React Context + useState/useReducer
- **HTTP Client**: Axios
- **Routing**: React Router v6
- **UI Components**: Custom components + Headless UI

### Services Externes
- **Email**: Service d'envoi d'emails pour magic links
- **Stockage**: Stockage local des fichiers uploadés
- **IA**: OpenAI API pour l'analyse et la génération

## Architecture des Données

### Modèle Utilisateur
```python
class User:
    id: int (Primary Key)
    email: str (Unique)
    created_at: datetime
    last_login: datetime
    is_active: bool
```

### Modèle Véhicule
```python
class Vehicle:
    id: int (Primary Key)
    user_id: int (Foreign Key)
    license_plate: str
    brand: str
    model: str
    year: int
    mileage: int
    fuel_type: str
    previous_owners: int
    purchase_date: date
    purchase_price: float
    created_at: datetime
    updated_at: datetime
```

### Modèle Document
```python
class Document:
    id: int (Primary Key)
    vehicle_id: int (Foreign Key)
    document_type: str (histovec, carte_grise, facture)
    file_path: str
    analysis_result: json
    uploaded_at: datetime
```

### Modèle Analyse
```python
class Analysis:
    id: int (Primary Key)
    vehicle_id: int (Foreign Key)
    reliability_score: float (1-10)
    monthly_cost_estimate: float
    resale_recommendation: json
    generated_ad: json
    created_at: datetime
```

## APIs Principales

### Authentification
- `POST /api/auth/request-login` - Demande de magic link
- `GET /api/auth/verify/{token}` - Vérification du token
- `POST /api/auth/logout` - Déconnexion

### Véhicules
- `GET /api/vehicles` - Liste des véhicules de l'utilisateur
- `POST /api/vehicles` - Ajout d'un nouveau véhicule
- `GET /api/vehicles/{id}` - Détails d'un véhicule
- `PUT /api/vehicles/{id}` - Mise à jour d'un véhicule
- `DELETE /api/vehicles/{id}` - Suppression d'un véhicule

### Documents
- `POST /api/vehicles/{id}/documents` - Upload de document
- `GET /api/vehicles/{id}/documents` - Liste des documents
- `GET /api/documents/{id}/analysis` - Résultat d'analyse

### Analyse IA
- `POST /api/vehicles/{id}/analyze` - Lancement de l'analyse complète
- `GET /api/vehicles/{id}/analysis` - Récupération de l'analyse
- `POST /api/vehicles/{id}/generate-ad` - Génération d'annonce de vente

## Structure des Dossiers

### Backend (Flask)
```
autocoach-backend/
├── app/
│   ├── __init__.py
│   ├── models/
│   │   ├── __init__.py
│   │   ├── user.py
│   │   ├── vehicle.py
│   │   └── document.py
│   ├── routes/
│   │   ├── __init__.py
│   │   ├── auth.py
│   │   ├── vehicles.py
│   │   └── analysis.py
│   ├── services/
│   │   ├── __init__.py
│   │   ├── ai_service.py
│   │   ├── ocr_service.py
│   │   └── email_service.py
│   └── utils/
│       ├── __init__.py
│       └── helpers.py
├── migrations/
├── uploads/
├── config.py
├── requirements.txt
└── run.py
```

### Frontend (React)
```
autocoach-frontend/
├── public/
├── src/
│   ├── components/
│   │   ├── common/
│   │   ├── auth/
│   │   ├── vehicle/
│   │   └── dashboard/
│   ├── pages/
│   │   ├── Landing.js
│   │   ├── Login.js
│   │   ├── Dashboard.js
│   │   └── VehicleDetails.js
│   ├── services/
│   │   └── api.js
│   ├── context/
│   │   └── AuthContext.js
│   ├── styles/
│   └── utils/
├── package.json
└── tailwind.config.js
```

## Fonctionnalités Clés

### 1. Landing Page
- Hero section avec proposition de valeur claire
- Démonstration des fonctionnalités
- Call-to-action pour l'inscription
- Témoignages et preuves sociales

### 2. Authentification Magic Link
- Connexion sans mot de passe
- Envoi d'email avec lien de connexion
- Gestion des sessions sécurisées

### 3. Onboarding Véhicule
- Saisie par plaque d'immatriculation
- Formulaire manuel en fallback
- Upload de documents (Histovec, Carte Grise)

### 4. Dashboard Véhicule
- Score de fiabilité (1-10)
- Estimation des coûts mensuels
- Recommandations de revente
- Alertes administratives

### 5. Assistant IA de Revente
- Génération d'annonces optimisées
- Suggestion de prix compétitifs
- Recommandations de photos
- Export vers LeBonCoin/LaCentrale

### 6. Génération de Rapports
- Rapport PDF détaillé
- Simulation de scénarios
- Comparaison avec d'autres modèles

## Sécurité et Performance

### Sécurité
- Authentification par token JWT
- Validation des données côté serveur
- Protection CSRF
- Chiffrement des données sensibles

### Performance
- Mise en cache des analyses IA
- Optimisation des images
- Lazy loading des composants
- Compression des assets

## Déploiement

### Environnement de Production
- Backend: Service de déploiement Flask
- Frontend: Service de déploiement statique
- Base de données: PostgreSQL hébergée
- CDN: Pour les assets statiques

Cette architecture garantit une application moderne, scalable et maintenable, répondant aux besoins spécifiques du marché français de l'automobile d'occasion.

