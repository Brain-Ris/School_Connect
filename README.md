# SchoolConnect 📱🎓

Application mobile Android pour le suivi scolaire Parent-Enfant, développée dans le cadre du cours **Développement Mobile** — UFR/SEA, Université Joseph Ki-Zerbo.
Elle consomme l'API REST du projet web **SchoolApp** (Laravel 12), développé dans le cadre du cours de Programmation Web et Framework.

**Enseignant :** Lionel Marcus G. KABORET

---

## 👥 Membres du groupe

| Nom complet | Rôle |
|---|---|
| OUEDRAOGO Tegawinde Cédric | Développement mobile (Android/Kotlin) + API Laravel |
| OUEDRAOGO Pouswende Ariane Léontia | Développement mobile (Android/Kotlin) + API Laravel |

---

## 📦 Structure du dépôt

```
/
├── App_mobile/         # Application Android (Kotlin)
└── SchoolApp/          # API backend (Laravel 12)
```

---

## 📋 Présentation générale

**SchoolApp** est l'API backend (Laravel). Elle permet à un gestionnaire d'administrer l'école (classes, élèves, notes, paiements) et à des enseignants de saisir les notes. Elle expose une API REST consommée par l'application mobile.

**SchoolConnect** est l'application mobile (Android). Elle permet aux parents de suivre en temps réel la scolarité de leurs enfants : notes, moyennes, rang, absences, paiements et annonces de l'école.

---

## ✨ Fonctionnalités de l'application mobile

### 🔐 Authentification
- Connexion sécurisée par email et mot de passe
- Déconnexion depuis le menu latéral

### 👤 Profil
- Modification des informations personnelles (nom, prénom, téléphone)
- Changement de mot de passe volontaire (accessible à tout moment depuis le profil)

### 🏠 Tableau de bord (Accueil)
- Carte de bienvenue avec le prénom du parent connecté
- **Carte "Mon enfant"** présentant l'enfant actuellement sélectionné :
  - Photo (ou avatar par défaut), prénom, nom et classe
  - Bouton **"Changer d'enfant"** pour basculer vers un autre enfant du foyer
  - Sélecteur de trimestre (**T1 / T2 / T3**) pour afficher la moyenne et le rang du trimestre choisi
  - Nombre d'absences non justifiées
  - Tableau des notes du trimestre sélectionné
- Aperçu des **notifications non lues** uniquement

### 👨‍👩‍👧 Mes enfants
- Liste de tous les élèves rattachés au compte du parent (photo, prénom, nom, classe)
- Au clic sur un élève, celui-ci devient l'**enfant sélectionné** et toutes les sections (Notes, Paiements, Absences) affichent ses informations

### 📝 Notes et bulletin
- Liste des matières et notes par trimestre
- Calcul et affichage des moyennes trimestrielles
- Consultation et telechargement du bulletin avec rang dans la classe

### 💰 Suivi des paiements
- Historique des versements effectués
- Montant total payé et montant restant à payer
- Consultation et téléchargement du reçu de paiement (PDF)

### 📅 Suivi des absences
- Liste des absences de l'élève sélectionné avec dates et motifs
- Distinction absences justifiées / non justifiées

### 🔔 Notifications et annonces
- Réception des annonces de l'école (réunions, examens, échéances)
- **Accueil** : affiche uniquement les notifications non lues
- **Page Notifications** : affiche toutes les notifications (lues et non lues)
- Marquage d'une notification comme lue au clic
- Bouton "Tout marquer comme lu"

---

## 🛠️ Stack technique

### Application mobile (Android)

| Élément | Détail |
|---|---|
| Langage | Kotlin |
| Architecture | MVVM (ViewModel + LiveData) |
| Asynchrone | Kotlin Coroutines |
| Stockage local | SharedPreferences (session, token, enfant sélectionné) |
| UI | Material Components 3 |

### API Backend (Laravel)

| Élément | Détail |
|---|---|
| Langage | PHP 8.2+ |
| Framework | Laravel 12 |
| Base de données | MySQL |
| PDF | barryvdh/laravel-dompdf 3.1 |

---

## 🏗️ Architecture du projet Android

```
App_mobile/app/src/main/java/com/example/schoolconnect/
├── MainActivity.kt                 # Dashboard principal + navigation (Drawer)
├── api/
│   ├── ApiService.kt               # Définition des endpoints Retrofit
│   ├── RetrofitClient.kt           # Configuration HTTP (BASE_URL, SERVER_URL)
│   └── AuthInterceptor.kt          # Injection automatique du token Bearer
├── models/
│   └── Models.kt                   # Modèles de données (Eleve, Note, Paiement, ...)
├── ui/
│   ├── auth/                       # Splash, Login, Changement de mot de passe
│   ├── dashboard/
│   │   └── DashboardViewModel.kt   # Logique métier du Dashboard
│   ├── profil/                     # Modification du profil + changement de mdp
│   ├── enfants/                    # Mes enfants (sélection de l'élève actif)
│   ├── notes/                      # Notes et bulletin
│   ├── paiements/                  # Suivi des paiements
│   ├── absences/                   # Suivi des absences
│   └── notifications/              # Annonces et notifications
└── utils/
    ├── SessionManager.kt           # Session + enfant sélectionné
    └── ImageLoader.kt              # Chargement asynchrone des photos (sans Glide)
```

---

## ⚙️ Installation

### Prérequis communs
- Git
- PHP 8.2+, Composer, MySQL
- Android Studio (Ladybug ou plus récent), JDK 11+
- Un appareil/émulateur Android API 24+

---

### 1️⃣ Backend — SchoolApp (Laravel)

```bash
# Cloner le dépôt et aller dans le dossier backend
cd SchoolApp

# Installer les dépendances PHP
composer install

# Copier le fichier d'environnement et configurer la base
cp .env.example .env
```

Éditer `.env` :
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=schoolapp
DB_USERNAME=root
DB_PASSWORD=votre_mot_de_passe

APP_URL=http://192.168.1.X:8000   # ← IP locale de votre machine
```

```bash
# Lancer les migrations
php artisan migrate

# Créer le lien symbolique pour les photos d'élèves
php artisan storage:link

# Insérer les données de test
php artisan db:seed

# Démarrer le serveur en écoutant sur toutes les interfaces réseau
php artisan serve --host=0.0.0.0 --port=8000
```

> ⚠️ `--host=0.0.0.0` est obligatoire pour que l'application Android (appareil physique ou émulateur) puisse joindre le serveur.

---

### 2️⃣ Application mobile — SchoolConnect (Android)

```bash
# Aller dans le dossier mobile
cd App_mobile
```

1. Ouvrir le dossier `App_mobile` dans **Android Studio** (`File > Open`)
2. Attendre la synchronisation Gradle (`Sync Now` si demandé)
3. Configurer l'adresse du serveur dans `RetrofitClient.kt` :

```kotlin
// App_mobile/app/src/main/java/com/example/schoolconnect/api/RetrofitClient.kt

const val SERVER_URL = "http://192.168.1.X:8000"       // ← même IP que APP_URL dans .env
private const val BASE_URL = "$SERVER_URL/api/"
```

> 💡 Sur émulateur Android Studio : utiliser `http://10.0.2.2:8000` à la place de l'IP.

4. Sélectionner un appareil/émulateur et cliquer sur ▶️ **Run**

---

## 🔑 Comptes de test

### Parents (application mobile)

| Prénom | Email | Mot de passe | Enfants |
|---|---|---|---|
| Fatoumata | `f.traore@parent.bf` | `Parent1234` | 2 enfants en CP1 |
| Moussa | `m.traore@parent.bf` | `Parent1234` | 2 enfants en CP1 |
| Rasmata | `r.sankara@parent.bf` | `Parent1234` | 1 enfant en CE1 |
| Boureima | `b.ouedraogo@parent.bf` | `Parent1234` | 1 enfant en CM1 |

### Administration (interface web Laravel)

| Rôle | Email | Mot de passe |
|---|---|---|
| Gestionnaire | `admin@gmail.com` | `Admin@1234` |
| Enseignant CP1 | `pierre@schoolapp.com` | `Password@123` |
| Enseignant CP2 | `awa@schoolapp.com` | `Password@123` |
| Enseignant CE1 | `moussa@schoolapp.com` | `Password@123` |
| Enseignant CE2 | `fatou@schoolapp.com` | `Password@123` |
| Enseignant CM1 | `issa@schoolapp.com` | `Password@123` |
| Enseignant CM2 | `mariam@schoolapp.com` | `Password@123` |

> ⚠️ À la première connexion sur l'interface web, un changement de mot de passe obligatoire est demandé.

---

## 📡 API REST consommée

| Méthode | Endpoint | Description |
|---|---|---|
| `POST` | `/api/login` | Connexion du parent |
| `POST` | `/api/logout` | Déconnexion |
| `GET` | `/api/profil` | Profil du parent connecté |
| `PUT` | `/api/profil` | Modifier le profil |
| `PUT` | `/api/profil/password` | Changer le mot de passe |
| `GET` | `/api/eleves` | Liste des enfants du parent |
| `GET` | `/api/eleves/{id}` | Détails d'un élève (moyenne, absences...) |
| `GET` | `/api/eleves/{id}/notes` | Notes par matière et trimestre |
| `GET` | `/api/eleves/{id}/moyennes` | Moyennes trimestrielles |
| `GET` | `/api/eleves/{id}/bulletin` | Bulletin et rang dans la classe |
| `GET` | `/api/eleves/{id}/paiements` | Historique des paiements |
| `GET` | `/api/eleves/{id}/absences` | Liste des absences |
| `GET` | `/api/notifications` | Toutes les notifications |
| `PATCH` | `/api/notifications/{id}/read` | Marquer une notification comme lue |
| `PATCH` | `/api/notifications/read-all` | Tout marquer comme lu |

---

## 📌 Notes de conception

- L'**enfant sélectionné** est mémorisé localement (`SessionManager`). Toutes les sections (Notes, Paiements, Absences) affichent les données de ce même enfant jusqu'au prochain changement via "Mes enfants".
- Le sélecteur **T1 / T2 / T3** sur l'Accueil recharge dynamiquement la moyenne, le rang et les notes du trimestre choisi via l'API.
- Les **notifications** sont globales au parent (annonces de l'école non filtrées par enfant). L'Accueil n'affiche que les non lues ; la page Notifications affiche tout.
- Le chargement des photos d'élèves est assuré par `ImageLoader.kt` (coroutines + `BitmapFactory`), sans dépendance externe (Glide/Picasso non disponibles).

---

## 📄 Licence

Projet académique — UFR/SEA, Université Joseph Ki-Zerbo. Usage pédagogique uniquement.
