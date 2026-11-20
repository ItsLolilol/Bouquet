# 💐 Bouquet ASCII 3D Interactif

![License: MIT](https://img.shields.io/badge/License-MIT-pink.svg)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?logo=docker&logoColor=white)
![Portainer](https://img.shields.io/badge/Portainer-Compatible-13BEF9?logo=portainer&logoColor=white)
![Zero Dependencies](https://img.shields.io/badge/Dependencies-0-brightgreen)
![AI Assisted](https://img.shields.io/badge/Made%20with-AI%20Assistance-8A2BE2)

Une application web minimaliste et auto-hébergeable qui génère un **bouquet de fleurs 3D en temps réel rendu intégralement en ASCII art coloré** sur fond noir pur. Conçue pour être légère, fluide à 60 FPS sur tous les écrans (smartphone, iPad, ordinateur) et déployable en quelques secondes avec **Docker** et **Portainer**.

---

## 📑 Sommaire

- [Aperçu et fonctionnalités](#-aperçu-et-fonctionnalités)
- [Architecture technique](#-architecture-technique)
- [Structure du dépôt](#-structure-du-dépôt)
- [Déploiement avec Portainer (Recommandé)](#-déploiement-avec-portainer-recommandé)
- [Déploiement via Docker CLI / Docker Compose](#-déploiement-via-docker-cli--docker-compose)
- [Contrôles interactifs](#-contrôles-interactifs)
- [Personnalisation](#-personnalisation)
- [Dépannage & Mises à jour](#-dépannage--mises-à-jour)
- [Transparence — Projet réalisé avec l'IA](#-transparence--projet-réalisé-avec-lia)
- [Licence MIT & Mentions légales](#-licence-mit--mentions-légales)

---

## ✨ Aperçu et fonctionnalités

* **Moteur de rendu 3D ASCII sur mesure :** véritable rastériseur logiciel 3D avec tampon de profondeur (*Z-buffer*) et éclairage directionnel + lumière de contour, projeté directement dans une grille de caractères ASCII (` .:-=+*#%@`).
* **Composition florale sculptée en 3D :**
  * **4 grands lys** à 6 pétales recourbés, dégradé blanc nacré/rosé et étamines dorées.
  * **8 roses en spirale de Fibonacci** (20 pétales galbés chacune) dans des tons rouge velours, framboise, rose poudré et champagne.
  * **2 tulipes latérales** lilas et champagne.
  * **Feuillage botanique arrondi** à pointe douce et touches de gypsophile blanc.
  * **Papier fleuriste 100 % opaque** avec plis naturels, col arrière élégant et grand nœud satiné en 3D.
* **Ambiance lumineuse :** 78 particules scintillantes en orbite autour du bouquet et message ASCII pulsant en bas de l'écran (`I love you sweetheart <3`).
* **100 % Responsive & Multi-plateforme :** adaptation dynamique de la grille ASCII, de la taille de police et du cadrage sur iPhone, Android, iPad, Mac et PC.
* **Ultra-léger côté client et serveur :** zéro framework, zéro dépendance externe, servi par un conteneur `nginx:alpine` consommant quelques mégaoctets de RAM.

---

## ⚙️ Architecture technique

| Composant | Technologie | Rôle |
| :--- | :--- | :--- |
| **Serveur Web** | `nginx:alpine` | Sert le fichier statique `index.html` sur le port interne `80` |
| **Rendu 3D** | HTML5 Canvas 2D + Vanilla JavaScript | Géométrie procédurale, transformations 3D, rastérisation par triangles et batching couleur |
| **Conteneurisation** | Docker / Docker Compose | Isolation, redémarrage automatique et exposition sur le port `2222` |
| **Orchestration** | Portainer | Déploiement simplifié par Stack (Web Editor ou dépôt Git) |

---

## 📁 Structure du dépôt

```text
.
├── index.html          # Application autonome (HTML, CSS, moteur 3D ASCII JS)
├── Dockerfile          # Image légère basée sur nginx:alpine
├── docker-compose.yml  # Configuration Docker exposant le port 2222
├── LICENSE             # Texte officiel de la licence MIT
└── README.md           # Documentation du projet
```

---

## 🐳 Déploiement avec Portainer (Recommandé)

Ce projet a été pensé pour être déployé directement depuis l'interface web de **Portainer**, sur le port **`2222`** par défaut.

### Méthode A : Via Portainer Web Editor (sans compilation locale — la plus rapide)

Cette méthode télécharge automatiquement la dernière version de `index.html` depuis GitHub au démarrage du conteneur. Pour appliquer une modification future, un simple **Restart** du conteneur suffit.

1. Dans Portainer, allez dans **Stacks** > **+ Add stack**.
2. Donnez un nom à la stack (ex. `bouquet-ascii`).
3. Choisissez **Web editor** et collez le YAML suivant en remplaçant `ItsLolilol` et `Bouquet` par les informations de ce dépôt :

```yaml
services:
  bouquet-ascii:
    image: nginx:alpine
    container_name: bouquet-ascii
    restart: unless-stopped
    ports:
      - "2222:80"
    command: >
      sh -c "wget -qO /usr/share/nginx/html/index.html https://raw.githubusercontent.com/ItsLolilol/Bouquet/main/index.html && nginx -g 'daemon off;'"
```

4. Cliquez sur **Deploy the stack**.
5. Accédez au site via `http://<IP-DE-VOTRE-SERVEUR>:2222`.

---

### Méthode B : Via Portainer Repository (avec `Dockerfile` et `docker-compose.yml`)

1. Dans Portainer, allez dans **Stacks** > **+ Add stack**.
2. Nommez la stack `bouquet-ascii`.
3. Sélectionnez **Repository**.
4. Renseignez :
   * **Repository URL :** `https://github.com/ItsLolilol/Bouquet`
   * **Repository reference :** `refs/heads/main`
   * **Compose path :** `docker-compose.yml`
5. Cliquez sur **Deploy the stack**.

> **Astuce Reverse Proxy :** Pour exposer le site en HTTPS sur un nom de domaine personnalisé (via Nginx Proxy Manager, Traefik ou Caddy), redirigez simplement votre sous-domaine vers `http://<IP-DU-SERVEUR>:2222`.

---

## 💻 Déploiement via Docker CLI / Docker Compose

Si vous préférez déployer le projet depuis un terminal sur votre serveur :

```bash
# 1. Cloner le dépôt
git clone https://github.com/ItsLolilol/Bouquet.git
cd Bouquet

# 2. Construire et démarrer le conteneur en arrière-plan
docker compose up -d --build
```

Le service sera immédiatement accessible sur `http://localhost:2222`.

Pour arrêter et supprimer le conteneur :

```bash
docker compose down
```

---

## 🕹️ Contrôles interactifs

Le bouquet tourne automatiquement sur lui-même et peut être manipulé librement en 3D :

| Action | PC / Mac (Souris / Trackpad) | Mobile / iPad (Tactile) |
| :--- | :--- | :--- |
| **Faire pivoter le bouquet** | Cliquer-glisser | Glisser avec 1 doigt |
| **Zoomer / Dézoomer** | Molette / défilement | Pincer avec 2 doigts |
| **Réinitialiser l'angle et le zoom** | Double-clic | Double-tap |

---

## 🎨 Personnalisation

Toute la configuration visuelle se trouve directement dans `index.html` :

* **Changer le message affiché en bas :** modifiez la constante `MESSAGE` en haut du `<script>` :
  ```js
  const MESSAGE = "I love you sweetheart <3";
  ```
* **Changer les couleurs :** ajustez les valeurs RVB `[R, G, B]` dans l'objet `COLORS`.
* **Modifier le nombre de particules :** changez la valeur `78` dans l'initialisation du tableau `sparkles`.
* **Changer la vitesse de rotation automatique :** modifiez `AUTO_SPIN_SPEED`.
* **Changer le port exposé :** remplacez `"2222:80"` par `"<VOTRE_PORT>:80"` dans votre stack Portainer ou dans `docker-compose.yml`.

---

## 🛠️ Dépannage & Mises à jour

* **Mettre à jour après une modification sur GitHub :**
  * *Avec la méthode `wget` :* dans Portainer, allez dans **Containers** > `bouquet-ascii` > **Restart**.
  * *Avec la méthode `build` / Repository :* supprimez l'ancienne image dans **Images** ou utilisez **Pull and redeploy** avec reconstruction de l'image.
* **Les modifications n'apparaissent pas dans le navigateur :** videz le cache de la page (`Ctrl + Shift + R` sur Windows/Linux, `Cmd + Shift + R` sur macOS, ou fermez et rouvrez l'onglet sur mobile).
* **Le navigateur affiche du code HTML brut :** assurez-vous que `index.html` a bien été enregistré en texte brut UTF-8 (et non en RTF/HTML encapsulé par un éditeur de texte enrichi) et que la première ligne commence exactement par `<!DOCTYPE html>`.

---

## 🤖 Transparence — Projet réalisé avec l'IA

Ce projet (le code du moteur 3D ASCII, la modélisation géométrique procédurale des fleurs, la configuration Docker/Portainer ainsi que cette documentation) a été **conçu et développé de manière itérative à l'aide de l'intelligence artificielle (Google Gemini)**, guidée par les choix esthétiques, les retours visuels et les exigences techniques de l'auteur du dépôt.

Les utilisateurs sont libres d'étudier, modifier, forker et réutiliser ce code dans le respect des termes de la licence MIT ci-dessous.

---

## 📄 Licence MIT & Mentions légales

Ce projet est distribué en open source sous **licence MIT**. Vous êtes libre de l'utiliser, le copier, le modifier, le fusionner, le publier, le distribuer, le sous-licencier et/ou en vendre des copies, à condition d'inclure l'avis de droit d'auteur et la présente autorisation dans toutes les copies ou parties substantielles du logiciel.

> **Recommandation :** Pensez à ajouter également un fichier nommé `LICENSE` à la racine du dépôt GitHub (via *Add file > Create new file > `LICENSE` > Choose a license template > MIT License*) pour que GitHub affiche automatiquement la licence dans l'encadré latéral du projet.

```text
MIT License

Copyright (c) 2026 [Votre Nom ou Pseudo GitHub]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

### Clause de non-responsabilité
Ce logiciel est fourni **« en l'état » (*AS IS*)**, sans garantie d'aucune sorte, expresse ou implicite. L'auteur ne pourra en aucun cas être tenu responsable des dommages directs ou indirects liés à l'utilisation, au déploiement ou à la modification de ce projet. Les marques **Docker**, **Portainer** et **NGINX** mentionnées dans cette documentation appartiennent à leurs propriétaires respectifs et ne sont citées qu'à des fins de compatibilité technique.
