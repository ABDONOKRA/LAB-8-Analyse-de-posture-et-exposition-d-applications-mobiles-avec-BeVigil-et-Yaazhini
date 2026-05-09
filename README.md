# 📱 LAB 8 — Audit de Sécurité Mobile (BeVigil & Yaazhini)

---

## 🧭 1. Objectif

Réaliser un audit de sécurité complet d'une application Android vulnérable (**InsecureBankv2**) en combinant deux approches complémentaires :

- **Analyse externe** via la plateforme BeVigil
- **Analyse statique interne** via l'outil Yaazhini

---

## ⚙️ 2. Mise en place de l'environnement

Organisation du répertoire de travail avec les dossiers nécessaires à l'audit.

<img width="334" height="677" alt="image" src="https://github.com/user-attachments/assets/6cecfb36-6494-4871-ad10-4163f656cce1" />


---

## 📦 3. Application analysée

| Champ | Valeur |
|-------|--------|
| Nom | InsecureBankv2 |
| Package | `com.android.insecurebankv2` |
| Version | 1.0 |
| Min SDK | 15 |
| Target SDK | 22 |

---

## 🔐 4. Vérification de l'intégrité — Hash SHA-256

Commande exécutée pour calculer l'empreinte du fichier APK :

```powershell
Get-FileHash "InsecureBankv2.apk" -Algorithm SHA256
```

> **Hash obtenu :** *(voir capture ci-dessous)*

<img width="1704" height="770" alt="image" src="https://github.com/user-attachments/assets/99b20bf8-f114-4098-8176-083a664220ed" />


---

## 🌐 5. Analyse externe — BeVigil

Inspection de l'exposition de l'application depuis la plateforme BeVigil.

**Problèmes détectés :**

| # | Vulnérabilité |
|---|---------------|
| 1 | Stockage non sécurisé de données sensibles |
| 2 | Données critiques exposées dans les logs |
| 3 | Activity exportée sans restriction |
| 4 | Secret potentiel identifié dans le code |
| 5 | Utilisation du protocole HTTP en clair |

---

## 🔍 6. Analyse statique interne — Yaazhini

Décompilation et inspection du contenu de l'APK via Yaazhini.

**Métadonnées extraites :**

```
Package  : com.android.insecurebankv2
Version  : 1.0
Min SDK  : 15
Target   : 22
```

---

## 📊 7. Triage et classification des vulnérabilités

Consolidation des résultats des deux outils, suppression des doublons et association aux référentiels OWASP Mobile Top 10.

<img width="1734" height="910" alt="image" src="https://github.com/user-attachments/assets/9b2aad23-397a-4eb2-b041-3dc4a9dc8460" />


---

## ⚠️ 8. Vulnérabilités identifiées

| Sévérité | Vulnérabilité | Catégorie OWASP |
|----------|---------------|-----------------|
| 🔴 Critique | Stockage de données sensibles non chiffré | M2 |
| 🔴 Critique | Logs exposant des informations critiques | M2 |
| 🟠 Haute | Communication HTTP non chiffrée | M3 |
| 🟠 Haute | Composants Android exportés sans contrôle | M1 |
| 🟡 Moyenne | SDK Android obsolète (API 15/22) | M8 |

---

## 🛡️ 9. Recommandations

* 🔒 Chiffrer toutes les données sensibles stockées localement
* 🚫 Désactiver les logs en environnement de production
* 🌐 Migrer vers HTTPS avec validation des certificats
* 📦 Mettre à jour les SDK vers des versions récentes et maintenues
* 🔐 Restreindre les composants exportés via `AndroidManifest.xml`

---

## 📌 10. Conclusion

L'audit de **InsecureBankv2** révèle plusieurs failles critiques liées à la gestion des données, aux communications réseau et à la configuration des composants Android. L'application sert de cas d'étude idéal pour comprendre les risques couverts par l'**OWASP Mobile Top 10** et l'importance d'intégrer la sécurité dès la phase de développement.
