# Espresso API - Starterkit

Date de création : 9/12/2025

Projet modèle d'API REST avec Express.js + TypeScript + Node.js + Docker.

- Runtime : __Node.js__,
- Langage : __TypeScript__,
- Micro Framewrok : __Express.js__,
- Conteneurisation : __Docker__.

## Variables d'environnement

Créez un fichier `.env` dans le dossier `api/` basé sur `.env.example` :

```env
NODE_ENV=development
PORT=3000
```

## Docker

### Mode Développement (avec hot reloading)

Le mode développement utilise `docker-compose.override.yml` automatiquement et active le __hot reloading__ grâce à la synchronisation des volumes.

Pour bénéficier de l'auto-complétion et ne pas afficher d'erreurs dans l'_IDE_, importez les dépendances _NPM_ sur la machine hôte (depuis la racine du dossier _api_) :

```bash
# se placer à la racine du dossier api
cd api
# import des dépendances NPM
npm install
```

```bash
# Démarrage en mode développement
docker-compose up

# Build de l'image et démarrage en mode développement
docker-compose up --build

# Démarrage en arrière-plan
docker-compose up -d
```

Chaque modification du code source dans `api/src/` produit un redémarrage automatique de l'application grâce à __nodemon__.

### Mode Production

Pour lancer en mode production, il faut préciser le fichier YAML à employer, soit docker-compose.yml (et non docker-compose.override.yml)

```bash
# Démarrer en mode production
docker-compose -f docker-compose.yml up

# Ou en arrière-plan
docker-compose -f docker-compose.yml up -d

# Build de l'image et démarrage
docker-compose -f docker-compose.yml up --build
```

### Commandes utiles

```bash
# Arrêter les conteneurs Docker
docker-compose down

# Arrêter et supprimer les volumes Docker
docker-compose down -v

# Rebuild des images Docker (sans démarrage des services)
docker-compose build

# Rebuild sans cache (et sans démarrage des services)
docker-compose build --no-cache
```

## Endpoints de l'API

Lorsque les services Docker sont démarrés, l'API est accessible sur `http://localhost:3000`

- `GET /` - Page d'accueil de l'API
- `GET /health` - Vérification de santé de l'application

Test avec curl :

```http
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=utf-8
Content-Length: 83
ETag: W/"53-ARR9a/zp2Vkv4HiRiiKDc3pOUQk"
Date: Tue, 09 Dec 2025 12:21:23 GMT
Connection: keep-alive
Keep-Alive: timeout=5

{"message":"Espresso API","version":"1.0.0","timestamp":"2025-12-09T12:21:23.060Z"}
```

### Dockerfile multi-stage

Le Dockerfile utilise une approche multi-stage pour optimiser les builds :

1. __Base__ : Installation des dépendances de base
2. __Development__ : Configuration pour le développement avec hot reloading
3. __Build__ : Compilation du code TypeScript
4. __Production__ : Image optimisée avec uniquement le code compilé et les dépendances de production

### docker-compose.yml (Production)

- __Important__ : le build de production nécessite la création d'un fichier package-lock.json sur la machine hôte.
- Build avec le stage `production` du Dockerfile
- Exécute le code compilé
- Image optimisée

### docker-compose.override.yml (Développement)

- Build avec le stage `development` du Dockerfile
- Monte les volumes pour la synchronisation du code
- Active nodemon pour le hot reloading
- Les `node_modules` sont dans un volume nommé pour éviter les conflits

--

!["Logotype Shrp"](https://sherpa.one/images/sherpa-logotype.png)

__Alexandre Leroux__  
_Enseignant / Formateur_  
_Développeur logiciel web & mobile_

Nancy (Grand Est, France)

<https://shrp.dev>
