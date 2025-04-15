# Workflow n8n Autonome pour Google Tag Manager

Ce dépôt contient un workflow n8n complet et autonome pour la gestion de Google Tag Manager (GTM), spécialement conçu pour la configuration des balises de conversion Google Ads et leurs déclencheurs associés.

## Fonctionnalités

- **Création et configuration de balises de conversion Google Ads**
- **Création de déclencheurs personnalisés** basés sur les événements du dataLayer
- **Association automatique des déclencheurs aux balises**
- **Publication automatique des modifications** avec création de version
- **Génération de rapports détaillés** sur les opérations effectuées

## Types d'événements supportés

Le workflow crée par défaut des déclencheurs pour les événements suivants :
- `formulaireSoumis` : Déclenché lors de la soumission d'un formulaire
- `phoneClick` : Déclenché lors d'un clic sur un numéro de téléphone
- `pageView` : Déclenché lors de la visualisation d'une page

## Prérequis

- Une instance n8n fonctionnelle
- Un compte Google avec accès à Google Tag Manager
- Des identifiants OAuth2 configurés pour l'API Google Tag Manager

## Installation

1. Téléchargez le fichier `workflow.json` depuis ce dépôt
2. Dans votre instance n8n, cliquez sur "Workflows" dans le menu de gauche
3. Cliquez sur le bouton "Import from File"
4. Sélectionnez le fichier `workflow.json` téléchargé
5. Configurez les identifiants OAuth2 de Google pour les nœuds HTTP Request

## Paramètres disponibles

| Paramètre | Description | Valeur par défaut |
|-----------|-------------|-------------------|
| `accountId` | ID du compte GTM | "6276037669" |
| `containerId` | ID du conteneur GTM | "210165143" |
| `workspaceId` | ID de l'espace de travail | "1" |
| `conversionId` | ID de conversion Google Ads | "16965181803" |
| `conversionLabel` | Label de conversion Google Ads | "h2PJCLqBiLQaEOvC0Jk_" |
| `autoPublish` | Publication automatique | true |

## Utilisation

1. **Exécution manuelle**:
   - Ouvrez le workflow importé
   - Modifiez les paramètres d'entrée selon vos besoins
   - Cliquez sur "Execute Workflow"

2. **Exécution via API**:
   - Activez le workflow en cliquant sur le bouton "Active"
   - Utilisez l'URL du webhook pour déclencher le workflow via une requête HTTP
   - Passez les paramètres dans le corps de la requête JSON

## Rapport généré

Le workflow génère un rapport détaillé comprenant:
- Les balises créées ou mises à jour
- Les déclencheurs créés ou existants utilisés
- Le statut de la publication (si activée)
- Un lien vers l'interface GTM pour vérification manuelle

## Licence

Ce projet est sous licence MIT.
