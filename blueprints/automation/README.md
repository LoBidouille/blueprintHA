# Blueprint : Gestion Thermique Clim

## 🎯 Objectif

Programmer automatiquement la température de vos climatiseurs selon l'horaire et le type de jour (semaine ou week-end).

## 📋 Architecture

Ce blueprint permet de créer **trois automatisations indépendantes** basées sur le même modèle logiciel :

- **Clim Salon** : Températures spécifiques pour votre espace de vie principal
- **Chambre Parentale** : Températures pour la chambre
- **Chambre d'Amis / Bureau** : Températures pour les pièces secondaires

Chaque climatisation aura donc ses propres paramètres selon vos besoins.

## 📦 Installation

### 1. Installer le blueprint

1. Ouvrir l'interface Home Assistant
2. Aller dans **Configuration > Blueprints**
3. Cliquer sur **"Blueprint"** > **"Importer un Blueprint"**
4. Sélectionner le fichier `clim_thermique_gestion.yaml`
5. Valider l'installation

### 2. Créer les trois automatisations

#### Automatisation 1 - Clim Salon

| Configuration       | Valeur par défaut                   | Description                                  |
| ------------------- | ---------------------------------- | ----------------------------------------- |
| Entité Clim         | `climate.salon`                    | Votre entité clim salon                          |
| Mode                | `heat`                             | Chauffage par défaut                           |
| Nom                 | `Salon`                            | Affiché dans l'historique HA                  |

**Semaine (Lundi-Vendredi) :**

| Heure            | Température              | Description                                  |
| ---------------- | ---------------------- | ----------------------------------------- |
| 05:00           | 21°C                   | Réveil, confort au réveil                    |
| 08:00           | 16°C                   | Départ éco pour la journée                  |
| 16:00           | 21°C                   | Retour maison, confort rétabli              |
| 23:30           | 20°C                   | Nuit bien dormir                            |

**Week-end (Samedi-Dimanche) :**

| Heure            | Température              | Description                                  |
| ---------------- | ---------------------- | ----------------------------------------- |
| 080:00          | 22°C                   | Réveil tardif, confort au réveil                     |
| 11:00           | 17°C                   | Eco pendant l'absence                         |
| 17:00           | 22°C                   | Retour à la maison, confort du soir                    |
| 23:00           | 19°C                   | Nuit du week-end                            |

#### Automatisation 2 - Chambre parentale

| Configuration       | Valeur par défaut                   | Description                                  |
| ------------------- | ---------------------------------- | ----------------------------------------- |
| Entité Clim         | `climate.chambre_dortoir`         | Votre entité clim chambre                          |
| Mode                | `heat`                             | Chauffage par défaut                           |
| Nom                 | `Chambre Parentale`                 | Affiché dans l'historique HA                  |

#### Automatisation 3 - Bureau

| Configuration       | Valeur par défaut                   | Description                                  |
| ------------------- | ---------------------------------- | ----------------------------------------- |
| Entité Clim         | `climate.chambre_bureau`          | Votre entité clim bureau                         |
| Mode                | `heat`                             | Chauffage par défaut                           |
| Nom                 | `Chambre/Bureau`                      | Affiché dans l'historique HA                  |

## 🔧 Logique de Fonctionnement

### 1. Surveillance en continu

Le blueprint surveille **8 déclencheurs** :
- 4 pour la Semaine (Lundi→Vendredi)
- 4 pour le Week-end (Samedi→Dimanche)

### 2. Détection du type de jour

À chaque déclenchement, l'automatisation vérifie automatiquement si c'est un jour ouvrable ou non :
```
isoweekday() >= 6 ? Weekend → Sinon Semaine
```

### 3. Application des températures

Une fois le type de jour identifié, l'automatisation applique la température correspondante à l'entité clim choisie.

## 🎛️ Exemples d'utilisation avancée

### Mode "cool" au lieu de "heat"
Modifier le champ **"Mode par défaut"** pour utiliser le mode refroidissement plutôt que chauffage.

### Températures personnalisées
Chaque climatisation peut avoir ses propres températures selon :
- L'isolation du logement
- Les besoins personnels des occupants
- L'exposition solaire (nord/sud)

### Désactiver la journalisation
Modifier le champ **"Journaliser les actions"** à `Off` si vous ne souhaitez pas que ces actions apparaissent dans l'historique Home Assistant.

## 📖 Références techniques

Le blueprint utilise :
- `climate.set_temperature` pour définir la température cible
- `climate.set_hvac_mode` pour activer le mode chauffage/cool
- `logbook.log` pour journaliser chaque action dans HA

Les déclencheurs peuvent être ajustés selon l'horaire de chacun et les horaires personnalisés sont disponibles dans la configuration.

## 📞 Besoin d'aide ?

Si vous rencontrez des problèmes :
1. Vérifier que les entités `climate.salon`, etc. existent bien
2. S'assurer que le mode sélectionné est supporté par votre climatiseur
3. Consulter les logs Home Assistant pour plus de détails

---

*Blueprint créé le 08/11/2026 - Version 1.0*
