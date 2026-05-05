# ⏱ LAB 16 – Maîtriser les Services dans une Application Android

---

## 📖 Présentation

Ce laboratoire a pour objectif de comprendre et maîtriser les **Services Android**, en particulier :

- ✅ Foreground Service (obligatoire depuis Android 8.0)
- ✅ Notification persistante
- ✅ Bound Service (communication Activity ↔ Service)
- ✅ Gestion du cycle de vie du Service
- ✅ Démarrage et arrêt propre du Service
  <img width="460" height="793" alt="image" src="https://github.com/user-attachments/assets/884cfd1b-f726-464c-9f5f-ba161d3fbd91" />


L’application développée est un **Chronomètre exécuté en arrière-plan**, capable de continuer à fonctionner même lorsque l’Activity est fermée.

---

# 🎯 Objectifs pédagogiques

À la fin de ce lab, vous serez capable de :

- Comprendre le cycle de vie d’un Service
- Utiliser `onStartCommand()` et `onBind()`
- Implémenter un **Foreground Service**
- Créer une notification persistante
- Gérer START_STICKY et START_NOT_STICKY
- Lier un Service à une Activity (Bound Service)

---

# 🏗 Architecture du projet

```
ServiceChronometreJava
│
├── MainActivity.java
├── ChronometreService.java
│
├── res/layout/activity_main.xml
└── AndroidManifest.xml
```

---

# 🔄 Fonctionnement général

```
MainActivity
     ↓
startForegroundService()
     ↓
ChronometreService
     ↓
Notification persistante
     ↓
ScheduledExecutorService
     ↓
Incrémentation chaque seconde
```

---

# ⚙ Fonctionnement technique

## ✅ 1️⃣ ChronometreService

### 🔹 Rôle :

- Exécuter un chronomètre en arrière-plan
- Maintenir une notification persistante
- Permettre la communication avec l’Activity

### 🔹 Mécanisme utilisé :

- `ScheduledExecutorService`
- Incrémentation toutes les 1 seconde
- `startForeground()` obligatoire Android 8+

---

## ✅ 2️⃣ Notification persistante

Depuis Android 8 (API 26) :

- Un Foreground Service doit afficher une notification.
- La notification ne peut pas être supprimée par l’utilisateur.
- Utilisation de `NotificationChannel`.

---

## ✅ 3️⃣ Bound Service

Grâce au `Binder` :

```java
public class LocalBinder extends Binder {
    public ChronometreService getService() {
        return ChronometreService.this;
    }
}
```

L’Activity peut récupérer l’instance du Service.

---

# 🔐 Permissions utilisées

```xml
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
```

✅ Obligatoire Android 13+

---

# 🧠 Cycle de vie du Service

| Méthode | Rôle |
|----------|------|
| onCreate() | Initialisation |
| onStartCommand() | Démarrage logique |
| onBind() | Liaison avec Activity |
| onDestroy() | Nettoyage mémoire |

---

# 🧪 Étapes de test

### ✅ Test 1 – Démarrage

- Lancer l’application
- Cliquer sur **DÉMARRER SERVICE**

Résultat attendu :

✔ Notification visible  
✔ Chronomètre commence  

---

### ✅ Test 2 – Quitter l’application

- Appuyer sur Home
- Fermer l’application

Résultat attendu :

✔ Notification toujours visible  
✔ Chronomètre continue  

---

### ✅ Test 3 – Arrêt

- Ouvrir l’application
- Cliquer sur **ARRÊTER SERVICE**

Résultat attendu :

✔ Notification supprimée  
✔ Chronomètre arrêté  

---

# 🧩 Code clé – Démarrage du Service

```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
    startForegroundService(intent);
} else {
    startService(intent);
}
```

---

# 🧩 Code clé – Notification

```java
startForeground(NOTIFICATION_ID, creerNotification());
```

---

# ⚠ Bonnes pratiques

- Toujours utiliser `startForeground()` pour un service long
- Créer un `NotificationChannel` (Android 8+)
- Nettoyer les threads dans `onDestroy()`
- Ne jamais bloquer le thread principal

---

# 🔒 Sécurité et bonnes pratiques

- Foreground Service requis pour les tâches longues
- Importance LOW pour notification silencieuse
- `stopForeground(true)` pour supprimer proprement la notification
- `START_STICKY` pour redémarrer automatiquement si tué

---

# 🎯 Résultat final

✔ Service fonctionnel  
✔ Notification persistante  
✔ Chronomètre temps réel  
✔ Bound + Foreground Service  
✔ Compatible Android 8+  

---

# 👨‍💻 Auteur

Projet réalisé dans le cadre du module :

**Programmation Mobile – Android avec Java**

Année universitaire 2025–2026

---

# ✅ Conclusion

Ce laboratoire permet de maîtriser un concept fondamental d’Android :

Les Services.

Il illustre parfaitement :

- La différence entre Activity et Service
- Le fonctionnement en arrière-plan
- Les contraintes Android modernes
- La gestion avancée du cycle de vie

Il constitue une base essentielle pour les applications de :

- GPS en arrière-plan
- Musique
- Téléchargement
- Synchronisation
- Notifications persistantes
