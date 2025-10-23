# Projet : GlycAmed

**Durée totale : 2 jours + Maison**  
**Mode : Binôme**  
**Rendu : Repository Git + Démo live**

---

## 📋 Contexte du projet

**GlycAmed** est une application collaborative de suivi de consommation de sucre et de caféine pour un étudiant spécifique nommé **Amed**.

### Le problème

Amed est un étudiant en informatique qui consomme énormément de boissons énergisantes et sucrées tout au long de la journée. Il n'a pas conscience de la quantité réelle de sucre et de caféine qu'il ingère quotidiennement, ce qui peut avoir des impacts négatifs sur sa santé.

### La solution

Créer une application web où **n'importe quel étudiant de l'école** peut se connecter et enregistrer en temps réel ce qu'il a vu Amed consommer. Cela crée un **suivi collectif transparent** pour aider Amed à prendre conscience de sa consommation.

### Cas d'usage concret

- Thomas voit Amed boire un Red Bull à la bibliothèque → il se connecte à GlycAmed et l'enregistre
- Sarah voit Amed prendre un Monster au distributeur → elle l'ajoute dans l'app
- Lucas voit Amed avec un Coca à la cafétéria → il le note dans GlycAmed
- En fin de journée, Amed et tous les étudiants peuvent voir combien de sucre et de caféine Amed a consommé

---

## 🎯 Objectifs pédagogiques

Ce projet vous permettra de mettre en pratique :

- **TypeScript strict** : Configuration et utilisation avancée
- **Architecture backend** : Express, REST API, séparation des responsabilités
- **Mongoose et MongoDB** : Modélisation de données, relations, agrégations
- **Authentification JWT** : Sécurisation des routes
- **Docker** : Containerisation complète de l'application
- **Intégration d'API externe** : Connexion à l'API Open Food Facts (Yuka)
- **Frontend interactif** : Interface connectée au backend

---

## 📦 Stack technique imposée

### Backend

- **Node.js** + **Express**
- **TypeScript** avec mode strict
- **MongoDB** + **Mongoose**
- **JWT** pour l'authentification

### Frontend

- **HTML / CSS / JavaScript** vanilla
- TypeScript optionnel pour le frontend

### Containerisation

- **Docker**
- **docker-compose** pour orchestrer l'ensemble

### API externe

- **Open Food Facts API** : https://world.openfoodfacts.org/data
  - Endpoint principal : `https://world.openfoodfacts.org/api/v0/product/{barcode}.json`
  - Documentation : https://wiki.openfoodfacts.org/API

---

## 🚀 Fonctionnalités obligatoires

### 1. Authentification

**Les utilisateurs doivent pouvoir :**

- S'inscrire avec email, password, prénom et nom
- Se connecter et recevoir un token JWT
- Accéder à leur profil
- Modifier leurs informations personnelles

**Contraintes :**

- Tous les endpoints (sauf login/register) doivent être protégés par JWT
- Le password doit être hashé avant stockage
- Validation des données d'entrée

### 2. Dashboard principal (vue d'Amed)

**Affichage en temps réel de :**

- Les statistiques du jour pour Amed :
  - Total de sucre consommé (en grammes)
  - Total de caféine consommée (en milligrammes)
  - Total de calories
  - Nombre de contributions reçues aujourd'hui
- Des indicateurs visuels :
  - Jauge/barre de progression pour le sucre (max recommandé : 50g/jour)
  - Jauge/barre de progression pour la caféine (max recommandé : 400mg/jour)
  - Changement de couleur selon le niveau (vert → orange → rouge)
- Statut de santé en temps réel :
  - "✅ Sous les limites"
  - "⚠️ Limite de sucre dépassée"
  - "🚨 Toutes les limites dépassées"

### 3. Ajouter une consommation

**N'importe quel utilisateur connecté peut ajouter une consommation pour Amed :**

**Recherche de produit :**

- Saisir un code-barres OU un nom de produit
- Interroger l'API Open Food Facts
- Afficher les résultats avec nom, marque, image

**Enregistrement :**

- Sélectionner le produit
- Indiquer la quantité consommée
- (Optionnel) Préciser le lieu (BU, RU, salle de cours, etc.)
- (Optionnel) Préciser l'heure (par défaut : maintenant)
- (Optionnel) Ajouter des notes

**Validation :**

- Calcul automatique des nutriments (sucre, caféine, calories) en fonction de la quantité
- Affichage d'un aperçu avant validation
- Enregistrement avec attribution au contributeur

### 4. Historique des consommations

**Afficher toutes les consommations d'Amed :**

- Liste chronologique complète
- Pour chaque entrée :
  - Date et heure
  - Produit (nom, marque, image)
  - Quantité
  - Nutriments (sucre, caféine, calories)
  - Contributeur (qui a ajouté)
  - Lieu (si renseigné)
  - Notes (si présentes)

**Fonctionnalités :**

- Filtrer par date
- Filtrer par produit
- Filtrer par contributeur
- Filtrer par lieu
- Possibilité de modifier/supprimer ses propres contributions uniquement

### 5. Feed d'activité en temps réel

**Timeline publique des dernières contributions :**

- Format : "Thomas a ajouté Red Bull 250ml • il y a 5 min • À la BU"
- Affichage des nutriments ajoutés
- Photo du produit si disponible
- Ordre chronologique inversé (plus récent en premier)
- Pagination ou chargement dynamique

### 6. Statistiques et graphiques

**Sélecteur de période :**

- 1 jour (aujourd'hui)
- 7 jours
- 30 jours
- 6 mois
- 1 an
- Personnalisé (date de début et de fin)

**Visualisations à implémenter :**

- Graphique en barres : sucre consommé par jour
- Graphique en ligne : évolution de la caféine dans le temps
- Graphique camembert : répartition par type de produit
- Heatmap (bonus) : heures de consommation

**Données agrégées à afficher :**

- Moyenne de sucre par jour
- Moyenne de caféine par jour
- Nombre de jours où les seuils ont été dépassés
- Produit le plus consommé
- Lieu le plus fréquent
- Contributeur le plus actif

### 7. Rapport de santé

**Génération d'un rapport sur une période choisie :**

- Score de santé global (0-100) basé sur le respect des recommandations OMS
- Résumé :
  - Moyenne quotidienne de sucre
  - Moyenne quotidienne de caféine
  - Nombre de jours en dépassement / Total
  - Tendance : en hausse 📈 / en baisse 📉 / stable ➡️
- Top 10 des produits les plus consommés
- Top 10 des contributeurs les plus actifs
- Recommandations automatiques (si temps)

### 8. Système d'alertes

**Alertes automatiques quand Amed dépasse les seuils :**

- Badge visible sur le dashboard si sucre > 50g aujourd'hui
- Badge visible sur le dashboard si caféine > 400mg aujourd'hui
- Historique des jours d'alerte
- Compteur de jours consécutifs en dépassement

### 9. Classement des contributeurs

**Page dédiée aux contributeurs :**

- Classement par nombre de contributions totales
- Affichage : prénom, nombre de contributions, dernière contribution
- Badges spéciaux (bonus) :
  - 🥇 Top contributeur du mois
  - 🔥 Contribution quotidienne (streak)
  - 👀 Première contribution du jour

---

## 📊 Recommandations OMS (à respecter)

- **Sucre** : Maximum 50g par jour (idéalement 25g)
- **Caféine** : Maximum 400mg par jour pour un adulte
- Ces seuils doivent être utilisés pour les calculs et les alertes

---

## 🔧 Contraintes techniques

### TypeScript

**Configuration obligatoire dans tsconfig.json :**

- `"strict": true`
- `"noImplicitAny": true`
- `"strictNullChecks": true`
- Aucun `any` dans le code (sauf justification en commentaire)

**Architecture attendue :**

- Séparation claire : Models / Controllers / Services / Routes / Middlewares
- Types et DTOs définis pour toutes les opérations
- Tous les modèles Mongoose doivent être complètement typés

### MongoDB et Mongoose

**Vous devez gérer :**

- La persistance des données
- Les relations entre collections
- Les agrégations pour les statistiques
- Les index pour optimiser les requêtes fréquentes

**Points d'attention :**

- Typer complètement vos schémas (interfaces, methods, statics)
- Gérer les validations au niveau du schéma
- Implémenter des hooks si nécessaire (pre/post save)

### API REST

**Conception d'une API REST complète :**

- Verbes HTTP appropriés (GET, POST, PUT, DELETE)
- Codes de statut HTTP corrects (200, 201, 400, 401, 404, 500)
- Format de réponse cohérent (JSON)
- Gestion des erreurs uniforme
- Pagination quand nécessaire

### Sécurité

- Hash des mots de passe (bcrypt)
- Protection JWT sur les routes sensibles
- Validation des données entrantes
- Gestion des erreurs sans exposer d'informations sensibles

### Docker

**Fichiers à fournir :**

- `Dockerfile` pour le backend
- `docker-compose.yml` orchestrant :
  - MongoDB
  - Backend
  - Frontend (via nginx ou serveur statique)

**L'application complète doit démarrer avec :**

```bash
docker-compose up
```

---

## 📐 Architecture attendue

```
glycamed/
├── backend/
│   ├── src/
│   │   ├── models/         # Schémas Mongoose
│   │   ├── controllers/    # Logique de traitement des requêtes
│   │   ├── services/       # Logique métier
│   │   ├── routes/         # Définition des routes
│   │   ├── middlewares/    # Auth, validation, error handling
│   │   ├── types/          # Interfaces et types TypeScript
│   │   │   └── dtos/       # Data Transfer Objects
│   │   ├── utils/          # Fonctions utilitaires
│   │   ├── config/         # Configuration (DB, etc.)
│   │   └── index.ts        # Point d'entrée
│   ├── Dockerfile
│   ├── tsconfig.json
│   ├── package.json
│   └── .env.example
├── frontend/
│   ├── index.html
│   ├── styles.css
│   ├── app.js
│   └── assets/
├── docker-compose.yml
└── README.md
```

---

## 🎨 Interface Frontend (suggestions)

**Pages minimales attendues :**

1. **Page de connexion/inscription**
2. **Dashboard principal** (vue d'Amed avec jauges et feed)
3. **Formulaire d'ajout de consommation** (recherche produit + validation)
4. **Page historique** (liste avec filtres)
5. **Page statistiques** (graphiques et période)
6. **Page rapport** (génération de rapport)
7. **Page classement** (top contributeurs)

**Vous êtes libres sur le design**, mais l'interface doit être :

- Fonctionnelle et intuitive
- Responsive (mobile-friendly)
- Connectée à votre API backend

---

## 📝 Livrables attendus

### 1. Code source

**Repository Git avec :**

- Commits réguliers et messages clairs
- Structure de dossiers propre et organisée
- Code TypeScript compilant sans erreur
- Pas de fichiers sensibles (.env, node_modules) dans Git

### 2. Documentation (README.md)

**Le README doit contenir :**

````markdown
# GlycAmed

## Description

[Brève description du projet]

## Prérequis

- Docker et docker-compose
- Node.js 18+ (pour développement local)

## Installation et lancement

### Avec Docker (recommandé)

```bash
docker-compose up
```
````

## Accès à l'application

- Frontend : http://localhost:8080
- Backend API : http://localhost:3000
- MongoDB : localhost:27017

## Endpoints API

### Auth

- POST /api/auth/register
- POST /api/auth/login
  [... liste complète ...]

## Fonctionnalités implémentées

- [x] Authentification JWT
- [x] Ajout de consommation
- [x] Dashboard avec jauges

### Application fonctionnelle

**L'application doit :**

- Démarrer avec `docker-compose up`
- Être entièrement fonctionnelle
- Permettre de :
  - Créer un compte
  - Se connecter
  - Ajouter des consommations pour Amed
  - Visualiser le dashboard
  - Consulter l'historique
  - Voir les statistiques

---

## 🎯 Critères d'évaluation (sur 20)

| Critère                              | Points | Détails                                                |
| ------------------------------------ | ------ | ------------------------------------------------------ |
| **Configuration TypeScript stricte** | /3     | `strict: true`, pas de `any`, types cohérents          |
| **Architecture backend**             | /3     | Séparation des responsabilités, organisation claire    |
| **Modèles Mongoose typés**           | /3     | Interfaces complètes, validations, méthodes            |
| **API REST complète**                | /3     | Tous les endpoints, gestion d'erreurs, JWT             |
| **Intégration API Yuka**             | /2     | Récupération produits, parsing, gestion d'erreurs      |
| **Docker fonctionnel**               | /2     | docker-compose lance toute l'app correctement          |
| **Frontend connecté**                | /2     | Interface fonctionnelle, appels API, affichage données |
| **Qualité du code**                  | /2     | Lisibilité, bonnes pratiques, README complet           |

---

## 📅 Planning recommandé

### Jour 1 - Fondations (7-8h)

1. **Setup initial**
   - Initialiser le projet (npm, Git)
   - Configurer `tsconfig.json` en mode strict
   - Créer `docker-compose.yml` et tester MongoDB
   - Structure de dossiers

2. **Authentification**
   - Modèle User avec Mongoose
   - Routes register/login
   - Génération JWT
   - Middleware d'authentification
   - Tests Postman

3. **Modèles de données**
   - Réfléchir à l'architecture de données
   - Créer les schémas Mongoose (Produit, Consommation, Stats...)
   - Typer complètement avec interfaces
   - Tester les connexions et insertions

4. **API Yuka et Produits **
   - Service pour interroger l'API Open Food Facts
   - Parser les réponses
   - Sauvegarder les produits en base
   - Route de recherche de produits

5. **CRUD Consommations **
   - Service et controller pour les consommations
   - Routes POST, GET, PUT, DELETE
   - Calculs de nutriments
   - Tests avec Postman

6. **Tests et corrections**
   - Débugger les erreurs
   - Améliorer la gestion d'erreurs
   - Commits réguliers

7. **Dashboard et statistiques (2h)**
   - Agrégations MongoDB pour stats quotidiennes
   - Calcul des moyennes par période
   - Routes pour dashboard et stats
   - Système d'alertes

8. **Frontend**
   - Structure HTML des pages principales
   - CSS de base
   - Page connexion/inscription
   - Navigation

9. **Frontend - Intégration**
   - Appels fetch vers l'API
   - Affichage du dashboard
   - Formulaire d'ajout de consommation
   - Feed d'activité
   - Historique avec filtres

10. **Docker et finalisation**
    - Créer Dockerfile backend
    - Tester docker-compose complet
    - Corrections finales
    - README.md complet
    - Dernier commit et push

---

## 💡 Conseils et astuces

### Commencez simple

- Ne cherchez pas la perfection immédiatement
- Fonctionnel > Parfait
- Itérez progressivement

### TypeScript

- Définissez vos types AVANT d'écrire le code
- Utilisez l'autocomplétion au maximum
- Lisez attentivement les erreurs du compilateur

### API Yuka

- Testez d'abord l'API avec Postman/Thunder Client
- Gérez les cas où le produit n'existe pas
- Cachez les produits déjà récupérés en base

### Calculs de nutriments

```typescript
// Exemple : Red Bull 250ml
// API retourne : sugars_100g = 11
// Quantité consommée : 250ml
// Calcul : (11 * 250) / 100 = 27.5g de sucre
```

### MongoDB Aggregations

- Utilisez `$match` pour filtrer
- Utilisez `$group` pour agréger
- Utilisez `$project` pour formater la sortie
- Testez dans MongoDB Compass avant de coder

### Frontend

- Commencez par le HTML/structure
- Ajoutez le CSS ensuite
- Connectez l'API en dernier
- Testez chaque fonctionnalité isolément

### Git

- Commits réguliers (toutes les 30min-1h)
- Messages clairs : "feat: ajout authentification JWT"
- Pushez souvent (sauvegarde)

---

## ⚠️ Pièges à éviter

❌ Utiliser `any` pour aller plus vite  
❌ Ne pas tester Docker avant le rendu final  
❌ Oublier la gestion d'erreurs  
❌ Ne pas valider les données entrantes  
❌ Coder sans avoir réfléchi à l'architecture  
❌ Oublier `.bind(this)` dans les routes Express  
❌ Ne pas hasher les passwords  
❌ Exposer des informations sensibles dans les erreurs  
❌ Faire un README incomplet ou absent

---

## 🚀 Fonctionnalités bonus (si temps restant)

Si vous terminez en avance, vous pouvez ajouter :

- **Scan de code-barres** via la caméra du téléphone
- **Export de données** en CSV ou PDF
- **Notifications** (simulation) quand alertes déclenchées
- **Comparaison temporelle** : "Cette semaine vs semaine dernière"
- **Objectifs collectifs** : "Aidons Amed à rester sous 40g cette semaine"
- **Commentaires** sur les contributions
- **Réactions** (émojis) sur les contributions
- **Mode sombre**
- **Graphiques plus avancés** (heatmap, courbes complexes)
- **Prédictions** : "Si Amed continue ainsi..."
- **Alternatives suggérées** : "Au lieu de Red Bull, essayer eau pétillante"

---

## 📚 Ressources utiles

### Documentation officielle

- **TypeScript** : https://www.typescriptlang.org/docs/
- **Express** : https://expressjs.com/
- **Mongoose** : https://mongoosejs.com/docs/typescript.html
- **Open Food Facts API** : https://wiki.openfoodfacts.org/API

### Exemples de codes-barres pour tests

- Red Bull Energy Drink 250ml : `9002490100070`
- Monster Energy 500ml : `0070847811329`
- Coca-Cola 330ml : `5449000000996`
- Pepsi 330ml : `5000112637458`

### Outils recommandés

- **Postman / Thunder Client** : Tester l'API
- **MongoDB Compass** : Explorer la base de données
- **Docker Desktop** : Gérer les containers

---

## 📞 En cas de blocage

1. **Lisez l'erreur TypeScript** attentivement
2. **Consultez la documentation** officielle
3. **Testez isolément** (commentez du code pour isoler le problème)
4. **Vérifiez les types** (hover sur les variables dans l'IDE)
5. **Demandez de l'aide** si bloqué plus de 30 minutes

---

## ✅ Checklist avant le rendu

- [ ] `tsconfig.json` avec `"strict": true`
- [ ] Aucune erreur de compilation TypeScript
- [ ] Pas de `any` dans le code (ou justifié)
- [ ] Tous les modèles Mongoose typés
- [ ] Authentification JWT fonctionnelle
- [ ] API REST complète testée
- [ ] Intégration API Yuka fonctionnelle
- [ ] Docker lance toute l'app (`docker-compose up`)
- [ ] Frontend connecté au backend
- [ ] README.md complet avec instructions
- [ ] Commits réguliers avec messages clairs
- [ ] Repository propre (pas de node_modules, .env)

---

# 🎉 Bonne chance !

**Rappel des 3 règles d'or :**

1. `"strict": true` toujours
2. Pas de `any`
3. Architecture réfléchie AVANT le code

**N'oubliez pas :** Un code qui compile en TypeScript strict est un code de qualité professionnelle !

**Amusez-vous et apprenez ! 💪**
