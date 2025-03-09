# Guide d'Installation et Configuration

## Prérequis
- Node.js v18+
- PostgreSQL
- Git
- Android Studio / Xcode (pour le développement mobile)

## 1. Configuration Initiale

```bash
# Cloner le dépôt
git clone https://github.com/andriantahiry2024/MCP-servers.git
cd MCP-servers

# Créer la structure de base
mkdir -p mobile-app/src/{components,screens,services,navigation,assets}
mkdir -p backend/src/{controllers,models,routes,middlewares,config}
mkdir -p admin-panel/src/{components,pages,services,assets}
mkdir -p delivery-module/src
```

## 2. Configuration du Backend

```bash
cd backend
npm init -y

# Installation des dépendances
npm install express cors dotenv
npm install pg sequelize
npm install jsonwebtoken bcryptjs
npm install firebase-admin nodemailer

# Créer le fichier .env
cat > .env << EOL
PORT=3000
NODE_ENV=development

# Database
DB_HOST=localhost
DB_USER=postgres
DB_PASS=your_password
DB_NAME=mcp_db

# JWT
JWT_SECRET=your_jwt_secret
JWT_EXPIRES_IN=24h

# Firebase
FIREBASE_PROJECT_ID=your_project_id
FIREBASE_PRIVATE_KEY=your_private_key
FIREBASE_CLIENT_EMAIL=your_client_email
EOL
```

## 3. Configuration de l'Application Mobile

```bash
cd ../mobile-app
npx react-native init MCPMobile

# Installation des dépendances
cd MCPMobile
npm install @react-navigation/native @react-navigation/stack
npm install axios redux react-redux @reduxjs/toolkit
npm install @react-native-firebase/app @react-native-firebase/messaging
npm install @react-native-async-storage/async-storage

# Configuration iOS (si nécessaire)
cd ios && pod install && cd ..
```

## 4. Configuration du Panel Admin

```bash
cd ../admin-panel
npx create-react-app mcp-admin

cd mcp-admin
npm install @mui/material @emotion/react @emotion/styled
npm install @reduxjs/toolkit react-redux
npm install axios react-router-dom
```

## 5. Configuration de la Base de Données

```sql
-- Se connecter à PostgreSQL et exécuter :
CREATE DATABASE mcp_db;
\c mcp_db

-- Tables principales
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10,2) NOT NULL,
    stock INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    total_amount DECIMAL(10,2) NOT NULL,
    status VARCHAR(20) NOT NULL,
    payment_ref VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 6. Lancement du Projet

### Backend
```bash
cd backend
npm run dev
```

### Application Mobile
```bash
cd mobile-app/MCPMobile
# Pour Android
npm run android
# Pour iOS
npm run ios
```

### Panel Admin
```bash
cd admin-panel/mcp-admin
npm start
```

## 7. Configuration Firebase

1. Créer un projet sur Firebase Console
2. Ajouter une application Android et iOS
3. Télécharger les fichiers de configuration
4. Placer `google-services.json` dans `mobile-app/android/app/`
5. Pour iOS, placer `GoogleService-Info.plist` dans le projet Xcode

## Notes de Sécurité

- Ne jamais commiter les fichiers .env
- Utiliser des variables d'environnement pour les secrets
- Configurer CORS correctement
- Utiliser HTTPS en production
- Mettre en place une validation des données
- Implémenter le rate limiting