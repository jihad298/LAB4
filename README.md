# LAB4

# 🔍 Android APK Reverse Engineering Report

## 🧾 Résumé exécutif

L’analyse de l’APK a permis d’identifier le flag du challenge  :

🟩 **Résultat : `I want to believe`**

L’application présente plusieurs faiblesses de sécurité, notamment une logique facilement reconstruisible et des données sensibles exposées.

---

## ℹ️ Informations générales

* **Hash APK :** []
* **Taille :** []
* **Package :** 
* **SDK cible :** 
* **Structure :** fichiers DEX, ressources, manifest

---

## 📦 Structure de l’APK

* Vérification des **magic bytes** (signature APK valide)
* Analyse des fichiers principaux :

| Fichier             | Description       |
| ------------------- | ----------------- |
| classes.dex         | Code compilé      |
| AndroidManifest.xml | Configuration app |
| resources.arsc      | Ressources        |
| META-INF            | Signature         |

---

## 📜 Analyse du Manifest

Principales configurations détectées :

| Élément     | Valeur  |
| ----------- | ------- |
| debuggable  | ⚠️ true |
| permissions | |

🔴 **Alerte :** L’application est en mode *debuggable*, ce qui facilite le reverse engineering.

---

## 🔎 String Pool DEX

Identification de **7 chaînes suspectes** :

| Index | String | Interprétation |
| ----- | ------ | -------------- |

---

## 🧠 Reconstruction de la logique

* Identification des classes principales
* Analyse du flux de validation

**Pseudo-code de `verify()` :**

```java
if (input.equals(decrypt(secret))) {
    return true;
}
return false;
```

---

## 🔐 Déchiffrement

Script Python utilisé :

```python
# Script simplifié
from Crypto.Cipher import AES

# logique de déchiffrement ici
```

**Résultat obtenu :**

```
I want to believe
```

✔ Utilisation du padding **PKCS7** pour compléter les blocs.

---

## ⚖️ Comparaison JADX vs JD-GUI

| Critère        | JADX | JD-GUI |
| -------------- | ---- | ------ |
| Lisibilité     | ✔️   | Moyen  |
| Reconstruction | ✔️   | Limité |
| Navigation     | ✔️   | ✔️     |

**Différences observées :**

* JADX reconstruit mieux les classes
* JD-GUI perd certaines structures
* Gestion différente des variables

---

## ⚠️ Vulnérabilités identifiées

### 1. Debug activé

* **Localisation :** Manifest
* **Description :** Mode debug actif
* **Preuve :** debuggable=true
* **Remédiation :** désactiver en production

### 2. Hardcoded strings

* **Localisation :** DEX
* **Description :** Données sensibles visibles
* **Remédiation :** utiliser chiffrement sécurisé

### 3. Logique exposée

* **Description :** Vérification facilement contournable

### 4. Absence d’obfuscation

* **Description :** Code lisible

### 5. Sécurité faible du chiffrement

* **Description :** Implémentation prévisible

---

## ✅ Conclusion

L’APK analysée présente plusieurs failles critiques facilitant son reverse engineering.

### 🎯 Enseignements

* Importance de l’obfuscation
* Sécurisation des données sensibles
* Désactivation du debug
* Protection du code métier
* Utilisation correcte du chiffrement
* Tests de sécurité avant déploiement

### 📊 Priorités (OWASP)

| Risque           | Priorité |
| ---------------- | -------- |
| Debug actif      | Haute    |
| Données exposées | Haute    |
| Logique visible  | Moyenne  |

---

## 📎 Annexe

* Commandes utilisées (jadx, apktool, etc.)
* Scripts complets
* Étapes détaillées d’analyse

---
