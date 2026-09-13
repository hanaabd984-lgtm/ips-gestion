# Serveur de synchronisation IPS (PC ↔ Téléphone)

Ce petit serveur permet à l'application IPS installée sur ton PC et sur ton
téléphone de partager exactement les mêmes données (articles, clients,
factures, stock, etc.), en temps réel, avec sauvegarde automatique du travail
fait hors connexion.

## 1. Choisir un compte

Décide d'un **email** et d'un **mot de passe** — ce sont les identifiants que
tu utiliseras pour te connecter depuis l'app, sur le PC comme sur le
téléphone. Ce n'est pas un vrai système de comptes multiples : tout appareil
connecté avec ces identifiants voit et modifie les mêmes données.

## 2. Déployer le serveur (gratuit) — via Render.com

1. Crée un compte sur [render.com](https://render.com) (gratuit).
2. Mets ce dossier `server/` dans un dépôt GitHub (ou utilise "Public Git
   Repository" avec le lien direct si tu en as un).
3. Sur Render : **New +** → **Web Service** → connecte le dépôt.
4. Renseigne :
   - **Build Command** : `npm install`
   - **Start Command** : `npm start`
5. Dans l'onglet **Environment**, ajoute ces variables :
   | Clé | Valeur |
   |---|---|
   | `SYNC_EMAIL` | ton email choisi à l'étape 1 |
   | `SYNC_PASSWORD` | ton mot de passe choisi à l'étape 1 |
   | `JWT_SECRET` | une longue phrase aléatoire (ex : générée sur https://1password.com/password-generator) |
6. Clique **Create Web Service**. Render te donne une URL du type :
   `https://ips-sync-xxxx.onrender.com`

*(Railway.app fonctionne de la même façon si tu préfères.)*

## 3. Se connecter depuis l'app

Sur le PC **et** sur le téléphone, à l'écran de connexion (ou dans
**Paramètres → Synchronisation**), renseigne :

- **Adresse du serveur** : l'URL donnée par Render (ex :
  `https://ips-sync-xxxx.onrender.com`)
- **Email** / **Mot de passe** : ceux choisis à l'étape 1

Connecte d'abord l'appareil qui a déjà toutes tes données (probablement le
PC) : il enverra son contenu au serveur. Connecte ensuite le téléphone : il
recevra automatiquement toutes ces données.

## 4. Fonctionnement au quotidien

- **Hors ligne** (pas de réseau) : l'app continue de fonctionner normalement,
  toutes les modifications sont enregistrées sur l'appareil.
- **Retour en ligne** : tout ce qui a été fait hors ligne est envoyé
  automatiquement au serveur, puis renvoyé à l'autre appareil (PC ou
  téléphone) — aucune action manuelle n'est nécessaire.
- Le badge en haut de l'app ("À jour", "Hors ligne", "Synchronisation…")
  indique l'état à tout moment.

## Note sur l'hébergement gratuit

Les instances gratuites de Render/Railway peuvent se mettre en veille après
une période d'inactivité — la première requête après une veille peut prendre
quelques secondes le temps qu'elle redémarre, c'est normal. Les données ne
sont pas perdues (elles sont sur le disque du service), mais si tu veux
zéro coupure, un petit plan payant (quelques dollars/mois) évite les mises en
veille.

## Développement local

```bash
cd server
npm install
SYNC_EMAIL="toi@exemple.com" SYNC_PASSWORD="motdepasse" JWT_SECRET="une-phrase-secrete" npm start
```

Le serveur écoute sur `http://localhost:3000` par défaut.
