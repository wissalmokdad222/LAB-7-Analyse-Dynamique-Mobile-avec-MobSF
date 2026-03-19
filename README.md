# LAB 7 : Analyse Dynamique Mobile avec MobSF

Ce laboratoire porte sur l'analyse dynamique (runtime) de l'application Android **DIVA (Damn Insecure and Vulnerable App)** en utilisant **MobSF (Mobile Security Framework)**.

##  Objectifs du lab
-Comprendre concrètement comment fonctionne l’analyse dynamique d’une application Android avec MobSF (ce qui se passe pendant l’exécution).
-Mettre en place un émulateur Android propre, sans Play Store, pour éviter les interférences.
-Installer et lancer MobSF facilement avec Docker.
-Tester une application vulnérable (DIVA) en conditions réelles : observer les logs, le trafic réseau, utiliser Frida, configurer le proxy HTTPS, etc.
-Apprendre à repérer les failles de sécurité en direct, comme le stockage non sécurisé, les intents exposés ou les informations sensibles codées en dur.

---

##  Étape 1 : Création de l’émulateur AVD sans Play Store
![](https://github.com/user-attachments/assets/05086294-23d6-4943-b996-3bdf1e39eef1)

##  Étape 2 : Cloner MobSF pour les scripts AVD
Ouvrez un terminal et tapez :
```bash
git clone https://github.com/MobSF/Mobile-Security-Framework-MobSF.git
cd Mobile-Security-Framework-MobSF
```
![](https://github.com/user-attachments/assets/8d18e771-812f-4453-829f-cfd51102c1bb)
##  Étape 3 : Lancement de l’émulateur avec le script MobSF
Dans le dossier MobSF :
- **Windows** : `scripts\start_avd.ps1`
Le script liste vos AVD → choisissez `MobSF_DIVA_API_30`.
```bash
./scripts/start_avd.sh MobSF_DIVA_API_30
```
![](https://github.com/user-attachments/assets/97cbc09c-1461-4ff0-9a64-1bdf3f1aef38)
![](https://github.com/user-attachments/assets/cf95a2cf-b8e2-4f2b-893a-d4b64c5b78de)
##  Étape 4 : Installation et lancement de MobSF via Docker
Récupérez l'image :
```bash
docker pull opensecurity/mobile-security-framework-mobsf:latest
```
Lancez MobSF avec l'identifiant de l'émulateur :
```bash
docker run -it --rm \
  -p 8000:8000 \
  -e MOBSF_ANALYZER_IDENTIFIER=emulator-5554 \
  opensecurity/mobile-security-framework-mobsf:latest
```
Accès via navigateur : `http://127.0.0.1:8000` (Login: `mobsf` / Pass: `mobsf`).

---
![](https://github.com/user-attachments/assets/134c84ac-4f9e-43bf-bfd0-5a2c7b81854a)
![](https://github.com/user-attachments/assets/53fd64df-f5d6-4d85-8405-f4a5e361b3d1)

##  Étape 5 : Préparation de l’APK DIVA
DIVA est une application contenant 13 challenges vulnérables.
![](https://github.com/user-attachments/assets/79613832-8054-4dd5-a281-6f07f5dc9379)
- Téléchargez l'APK sur le site officiel ou via GitHub.
- Gardez le fichier `diva.apk` prêt sur votre bureau.
  
![](https://github.com/user-attachments/assets/c24a3958-4f5c-4123-9217-138f4703477f)


##  Étape 6 : Analyse Statique + Dynamique de DIVA
1. Dans MobSF → **Upload & Analyze** → choisissez `diva.apk`.
   ![](https://github.com/user-attachments/assets/6ee3e40d-0ecc-49cb-ac79-f7f043e9d6e2)
   ![](https://github.com/user-attachments/assets/ba56183a-6da7-4f7e-b9be-3936d5e0053e)
3. Une fois l'analyse statique terminée, cliquez sur **Start Dynamic Analyzer**.

MobSF va configurer automatiquement l'environnement (Frida Server, Proxy HTTPS, installation de l'APK).
![](https://github.com/user-attachments/assets/85c04cd6-2743-4bfb-bae9-01c54af6826c)
![](https://github.com/user-attachments/assets/04b7458c-a2f1-4daa-83b1-3345183d5982)
![](https://github.com/user-attachments/assets/ef7690c1-0fe0-4ff5-b352-2f4cb01a9a4a)
### Exploration dans MobSF Dynamic Analyzer :
![](https://github.com/user-attachments/assets/4b1aee56-388d-488b-8d19-4ef4f3ea3720)
![](https://github.com/user-attachments/assets/57560c89-5ab5-4885-8f15-f040c317ae2f)
![](https://github.com/user-attachments/assets/3b92f0b6-d3cb-421a-a520-200d5b566e9b)
![](https://github.com/user-attachments/assets/b6b1c325-c45c-4750-80be-a71ffe74a681)

### Exemples de vulnérabilités trouvées :
| Challenge | Observation |
|-----------|-------------|
| **Insecure Data Storage** | Capture des fichiers écrits en clair dans le stockage local. |
| **Access Control** | Détection d'intents non sécurisés. |
| **Insecure Logging** | Fuite d'informations sensibles dans Logcat. |

---

##  Description du Menu Dynamic Analyzer

| Option | Utilité en analyse dynamique |
| :--- | :--- |
| **Stop Screen** | Arrêter le mirroring de l’écran de l’émulateur. |
| **Remove Root CA** | Nettoyer l’environnement après interception HTTPS. |
| **Unset Proxy** | Arrêter la redirection du trafic réseau vers MobSF. |
| **TLS Security** | Vérifier la validation des certificats et faiblesses TLS. |
| **Activity Tester** | Lancer et observer les écrans internes (activités exportées). |
| **Take Screenshot** | Documenter une étape de test ou une vulnérabilité. |
| **Logcat Stream** | Afficher les logs Android en temps réel (détection de fuites). |
| **Generate Report** | Regrouper tous les résultats dans un rapport final. |



##  Conclusion
Ce lab m'a permis de pratiquer l'analyse en temps réel, d'intercepter du trafic chiffré et d'observer comment une application manipule les données sensibles en mémoire et sur le stockage.

---
*Réalisé par Wissal MOKDAD*
