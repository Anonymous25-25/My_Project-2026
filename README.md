# 🛡️ Configuration et Sécurisation de pfSense

![pfSense](https://img.shields.io/badge/pfSense-2.7.x-blue?style=for-the-badge&logo=pfsense)
![FreeBSD](https://img.shields.io/badge/FreeBSD-Based-red?style=for-the-badge&logo=freebsd)
![Security](https://img.shields.io/badge/Security-Hardening-green?style=for-the-badge&logo=security)
![Academic](https://img.shields.io/badge/Project-Academic-orange?style=for-the-badge&logo=academia)

## 📋 Description du Projet

Ce projet présente la **configuration complète et la sécurisation d'un pare-feu pfSense** dans le cadre du module **Durcissement des Systèmes et Réseaux**. Il documente l'implémentation d'une architecture réseau sécurisée avec segmentation des zones (LAN/WAN/DMZ), authentification SSH par clés RSA et contrôle d'accès via portail captif.

> **Contexte Académique** : OFPPT - Institut Spécialisé de Technologie Appliquée
> **Filière** : Infrastructure Digitale - Option Cyber Security
> **Module** : Durcissement des Systèmes

## 📚 Table des Matières

- [🎯 Objectifs du TP](#-objectifs-du-tp)
- [📊 Aperçu du Contenu](#-aperçu-du-contenu)
- [🏗️ Structure du Document](#️-structure-du-document)
- [💻 Technologies Utilisées](#-technologies-utilisées)
- [🌐 Architecture Réseau](#-architecture-réseau)
- [⚙️ Fonctionnalités Implémentées](#️-fonctionnalités-implémentées)
- [📖 Comment Utiliser ce Document](#-comment-utiliser-ce-document)
- [🧪 Tests Réalisés](#-tests-réalisés)
- [📋 Prérequis](#-prérequis)
- [🚀 Améliorations Futures](#-améliorations-futures)
- [👨‍🎓 Auteur](#-auteur)
- [📄 Licence](#-licence)

## 🎯 Objectifs du TP

Ce travail pratique vise à développer les compétences suivantes :

1. **🔧 Installation et Configuration** - Déploiement de pfSense en environnement virtualisé
2. **🔒 Segmentation Réseau** - Création et isolation des zones de sécurité (LAN/WAN/DMZ)
3. **🛡️ Durcissement Système** - Application du principe de moindre privilège
4. **🔐 Authentification Forte** - Configuration SSH avec clés RSA (2048+ bits)
5. **👤 Contrôle d'Accès** - Implémentation d'un portail captif
6. **🔍 Audit de Sécurité** - Validation par tests de pénétration basiques

## 📊 Aperçu du Contenu

Le rapport complet comprend **15 sections détaillées** couvrant tous les aspects techniques et sécuritaires :

| Section                            | Contenu                                                            | Pages |
| ---------------------------------- | ------------------------------------------------------------------ | ----- |
| **Introduction**             | Contexte des cybermenaces et importance du durcissement            | 2     |
| **Configuration Interfaces** | Plan d'adressage IP et segmentation réseau                        | 3     |
| **Règles Firewall**         | Politiques de sécurité avec justifications techniques            | 4     |
| **Sécurisation SSH**        | Authentification par clés RSA et désactivation des mots de passe | 2     |
| **Portail Captif**           | Contrôle d'accès utilisateur et gestion des identités           | 3     |
| **Tests & Validation**       | Tableaux de résultats et analyse de conformité                   | 2     |
| **Analyse Sécuritaire**     | Évaluation approfondie des mesures implémentées                 | 3     |

## 🏗️ Structure du Document

```
📄 rapport-pfsense.html
├── 🎨 Page de garde (Logo, informations institutionnelles)
├── 📋 Table des matières (Navigation interactive)
├── 📖 Introduction (Contextualisation technique)
├── 🎯 Objectifs (6 objectifs spécifiques)
├── 💻 Environnement (Tableau matériel/logiciels)
├── 🌐 Architecture (Topologie 3 zones)
├── ⚙️ Installation (Procédure étape par étape)
├── 🔧 Configuration Interfaces (Plan d'adressage)
├── 🛡️ Règles Firewall (Justifications techniques)
├── 🔐 SSH Sécurisé (Clés RSA + durcissement)
├── 👤 Portail Captif (AAA - Authentication, Authorization, Accounting)
├── ✅ Tests (Tableau de validation)
├── 🔍 Analyse Sécurité (4 axes d'analyse)
├── 📝 Conclusion (Synthèse et compétences)
└── 📚 Annexes (Commandes et références)
```

## 💻 Technologies Utilisées

| Catégorie               | Technologie               | Version   | Badge                                                                            |
| ------------------------ | ------------------------- | --------- | -------------------------------------------------------------------------------- |
| **Pare-feu**       | pfSense Community Edition | 2.7.x     | ![pfSense](https://img.shields.io/badge/pfSense-2.7.x-0066CC?logo=pfsense)         |
| **Système**       | FreeBSD                   | 13.x      | ![FreeBSD](https://img.shields.io/badge/FreeBSD-13.x-CC0000?logo=freebsd)          |
| **Virtualisation** | VMware Workstation        | Pro 17    | ![VMware](https://img.shields.io/badge/VMware-Workstation-607078?logo=vmware)      |
| **SSH Client**     | PuTTY + PuTTYgen          | 0.78+     | ![PuTTY](https://img.shields.io/badge/PuTTY-SSH%20Client-blue)                     |
| **Cryptographie**  | RSA Keys                  | 4096 bits | ![Encryption](https://img.shields.io/badge/RSA-4096%20bits-green?logo=letsencrypt) |

## 🌐 Architecture Réseau

```
                    [ INTERNET ]
                         |
                    (WAN Interface)
                         |
               ┌─────────────────────┐
               │                     │
               │    pfSense FW       │
               │  192.168.10.1/24    │
               │                     │
               └─────────┬───────────┘
                         │
           ┌─────────────┼─────────────┐
           │             │             │
    (LAN Interface)  (DMZ Interface)   │
      192.168.10.0/24  192.168.20.0/24 │
           │             │             │
    ┌─────────────┐ ┌─────────────┐    │
    │  PC Clients │ │ Serveur Web │    │
    │   + Admin   │ │   Public    │    │
    └─────────────┘ └─────────────┘    │
                                      │
                              ┌─────────────┐
                              │   Gateway   │
                              │   Internet  │
                              └─────────────┘
```

**Zones de Sécurité** :

- 🔴 **WAN** : Non fiable (Untrusted) - Bloc tout par défaut
- 🟢 **LAN** : Fiable (Trusted) - Réseau interne avec contrôle d'accès
- 🟡 **DMZ** : Semi-fiable - Services publics isolés du LAN

## ⚙️ Fonctionnalités Implémentées

### 🔒 Sécurité Réseau

- ✅ **Segmentation stricte** entre LAN/DMZ/WAN
- ✅ **Règles de filtrage** basées sur le principe "Default Deny"
- ✅ **Isolation DMZ** → Empêche les mouvements latéraux
- ✅ **NAT et masquerading** pour l'accès Internet

### 🔐 Administration Sécurisée

- ✅ **SSH durci** avec authentification par clés RSA uniquement
- ✅ **Interface Web HTTPS** avec certificat auto-signé
- ✅ **Désactivation des mots de passe** SSH pour éliminer le brute force
- ✅ **Logs d'audit** pour traçabilité des connexions

### 👤 Contrôle d'Accès

- ✅ **Portail captif** avec base utilisateurs locale
- ✅ **Authentification obligatoire** avant accès Internet
- ✅ **Gestion des sessions** avec timeout configurable
- ✅ **Page de connexion personnalisée**

### 📊 Monitoring

- ✅ **Dashboard temps réel** (États, CPU, RAM, Bande passante)
- ✅ **Logs système** et firewall centralisés
- ✅ **Alertes de sécurité** configurables

## 📖 Comment Utiliser ce Document

### 💻 Visualisation du Rapport

1. **Ouvrir le fichier HTML** :

   ```bash
   # Double-clic sur le fichier ou
   firefox rapport-pfsense.html
   # ou
   chrome rapport-pfsense.html
   ```
2. **Navigation** :

   - Utilisez la **table des matières interactive** pour naviguer
   - Cliquez sur les liens pour accéder directement aux sections
   - Le document est optimisé pour l'**impression A4**
3. **Export PDF** :

   ```
   Navigateur → Imprimer → Enregistrer au format PDF
   ```

### 📱 Compatibilité

- ✅ Tous navigateurs modernes (Chrome, Firefox, Safari, Edge)
- ✅ Responsive design pour tablettes
- ✅ Optimisé impression A4
- ✅ Navigation clavier accessible

## 🧪 Tests Réalisés

| Test                           | Description                 | Résultat Attendu        | Statut |
| ------------------------------ | --------------------------- | ------------------------ | ------ |
| **🌐 Connectivité LAN** | Ping client → Gateway      | Réponse OK              | ✅     |
| **🚫 Isolation DMZ**     | DMZ → LAN                  | Bloqué                  | ✅     |
| **🔐 SSH Clé RSA**      | Connexion avec clé privée | Accès immédiat         | ✅     |
| **❌ SSH Mot de passe**  | Tentative sans clé         | Refusé                  | ✅     |
| **🌐 Portail Captif**    | Navigation HTTP             | Redirection login        | ✅     |
| **🔒 Règles Firewall**  | Trafic inter-zones          | Conforme à la politique | ✅     |

## 📋 Prérequis

### 🖥️ Matériel Minimum

- **CPU** : 2 cœurs, support virtualisation (VT-x/AMD-V)
- **RAM** : 4 GB (8 GB recommandés)
- **Stockage** : 20 GB disponibles
- **Réseau** : 2+ interfaces réseau (virtuelles)

### 💿 Logiciels Requis

- **Hyperviseur** : VMware Workstation/VirtualBox/Hyper-V
- **pfSense ISO** : Version 2.7.x depuis [pfsense.org](https://www.pfsense.org/download/)
- **Client SSH** : PuTTY (Windows) / OpenSSH (Linux/macOS)
- **Générateur clés** : PuTTYgen ou ssh-keygen

### 📚 Connaissances Préalables

- **Réseaux TCP/IP** : Adressage, routage, VLAN
- **Administration Linux/BSD** : Ligne de commande, services
- **Sécurité réseau** : Firewalls, VPN, authentification

## 🚀 Améliorations Futures

### 🔒 Sécurité Avancée

- [ ] **IDS/IPS** - Implémentation Suricata pour détection d'intrusion
- [ ] **VPN Site-to-Site** - Connexion sécurisée entre sites distants
- [ ] **2FA** - Authentification à deux facteurs pour l'interface web
- [ ] **Fail2Ban** - Protection automatique contre brute force

### 📊 Monitoring & Alerting

- [ ] **ELK Stack** - Centralisation logs avec Elasticsearch
- [ ] **Grafana** - Dashboards avancés temps réel
- [ ] **SNMP** - Supervision réseau avec Nagios/Zabbix
- [ ] **Alertes email** - Notifications automatiques incidents

### 🌐 Fonctionnalités Réseau

- [ ] **Load Balancing** - Répartition de charge multi-WAN
- [ ] **QoS** - Priorisation trafic par application
- [ ] **Proxy Transparent** - Cache web avec Squid
- [ ] **DNS Filtering** - Blocage domaines malveillants

## 👨‍🎓 Auteur

**Youness Boussedari**
🎓 **Filière** : Infrastructure Digitale - Option Cyber Security
🏫 **Institution** : OFPPT - Direction Régionale Rabat-Salé-Kénitra
📅 **Année Académique** : 2025-2026
📧 **Contact** : [youness.boussedari@edu.ma](mailto:youness.boussedari@edu.ma)

### 🎯 Compétences Développées

- Configuration et administration pfSense
- Durcissement système et réduction surface d'attaque
- Segmentation réseau et politiques de sécurité
- Cryptographie appliquée (SSH, RSA)
- Audit de sécurité et tests de pénétration

## 📚 Ressources Utiles

### 📖 Documentation Officielle

- [pfSense Documentation](https://docs.netgate.com/pfsense/en/latest/) - Guide complet officiel
- [FreeBSD Handbook](https://docs.freebsd.org/en/books/handbook/) - Base système
- [RFC 4251](https://tools.ietf.org/html/rfc4251) - SSH Protocol Architecture

### 🎥 Formation Complémentaire

- [pfSense Fundamentals](https://www.netgate.com/resources/videos/pfsense-fundamentals) - Vidéos officielles
- [Network Security Courses](https://www.cybrary.it/catalog/network-security/) - Cybrary
- [SANS SEC511](https://www.sans.org/cyber-security-courses/continuous-monitoring-security-tuning/) - Formation avancée

### 🛠️ Outils Complémentaires

- [Nmap](https://nmap.org/) - Découverte réseau et audit
- [Wireshark](https://www.wireshark.org/) - Analyse de paquets
- [OpenVAS](https://www.openvas.org/) - Scanner de vulnérabilités

## 📄 Licence

📚 **Usage Académique Uniquement**

Ce document est créé dans un cadre pédagogique pour le module **Durcissement des Systèmes**. Il est destiné à :

- ✅ Formation et apprentissage
- ✅ Partage avec la communauté éducative
- ✅ Référence pour futurs étudiants
- ❌ Usage commercial interdit
- ❌ Reproduction sans attribution interdite

---

<div align="center">

**🏆 Projet réalisé avec passion pour l'excellence en cybersécurité**

![Made with](https://img.shields.io/badge/Made%20with-❤️-red?style=for-the-badge)
![Security](https://img.shields.io/badge/Security-First-green?style=for-the-badge)

</div>
