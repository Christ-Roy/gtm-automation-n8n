# Guide d'utilisation du workflow Google Tag Manager Autonome

Ce guide détaillé vous explique comment utiliser le workflow n8n pour automatiser la configuration de Google Tag Manager, spécifiquement pour les balises de conversion Google Ads et leurs déclencheurs.

## Table des matières

1. [Prérequis](#prérequis)
2. [Installation](#installation)
3. [Configuration des identifiants](#configuration-des-identifiants)
4. [Personnalisation des paramètres](#personnalisation-des-paramètres)
5. [Exécution du workflow](#exécution-du-workflow)
6. [Comprendre le rapport](#comprendre-le-rapport)
7. [Dépannage](#dépannage)

## Prérequis

Avant de commencer, assurez-vous d'avoir:

- Une instance n8n opérationnelle (version 0.170.0 ou supérieure)
- Un compte Google avec accès à Google Tag Manager
- Un compte Google Ads avec au moins une conversion configurée
- Les droits d'édition sur le conteneur GTM cible

## Installation

1. **Téléchargez le fichier workflow.json** depuis ce dépôt
2. **Importez-le dans n8n**:
   - Connectez-vous à votre instance n8n
   - Cliquez sur "Workflows" dans le menu de gauche
   - Cliquez sur le bouton "Import from File"
   - Sélectionnez le fichier workflow.json téléchargé
   - Cliquez sur "Import"

## Configuration des identifiants

Le workflow utilise l'authentification OAuth2 de Google pour accéder à l'API Google Tag Manager.

1. **Créez un identifiant OAuth2 dans n8n**:
   - Allez dans "Credentials" dans le menu de gauche
   - Cliquez sur "Create New"
   - Sélectionnez "OAuth2 API" dans le menu déroulant
   - Choisissez "Google" comme OAuth2 API
   - Configurez l'identifiant avec vos informations client Google
   - Assurez-vous que la portée inclut `https://www.googleapis.com/auth/tagmanager.edit.containers`

2. **Associez l'identifiant aux nœuds HTTP Request**:
   - Ouvrez le workflow importé
   - Pour chaque nœud HTTP Request, cliquez dessus et assurez-vous que l'authentification OAuth2 est sélectionnée
   - Choisissez l'identifiant créé précédemment

## Personnalisation des paramètres

Le workflow accepte plusieurs paramètres que vous pouvez personnaliser:

1. **Paramètres GTM et Google Ads**:
   - `accountId`: ID du compte GTM (trouvé dans l'URL de l'interface GTM)
   - `containerId`: ID du conteneur GTM
   - `workspaceId`: ID de l'espace de travail (généralement "1")
   - `conversionId`: ID de conversion Google Ads
   - `conversionLabel`: Label de conversion Google Ads
   - `autoPublish`: Activer/désactiver la publication automatique (true/false)

2. **Comment trouver vos IDs Google Ads**:
   - Connectez-vous à votre compte Google Ads
   - Allez dans "Outils et paramètres" > "Conversions"
   - Sélectionnez la conversion souhaitée
   - L'ID de conversion et le label se trouvent dans la section "Balise"

## Exécution du workflow

### Exécution manuelle

1. Ouvrez le workflow importé
2. Modifiez les paramètres dans le nœud "Paramètres d'entrée" selon vos besoins
3. Cliquez sur "Execute Workflow"
4. Attendez que l'exécution se termine et consultez le rapport généré

### Exécution via API

Pour automatiser l'exécution, vous pouvez:

1. Activer le workflow en cliquant sur le commutateur "Active"
2. Configurer un déclencheur webhook:
   - Ajoutez un nœud "Webhook" avant le nœud "Start"
   - Configurez-le avec une méthode POST
   - Liez la sortie du webhook au nœud "Start"

3. Utilisez l'URL du webhook pour déclencher le workflow via une requête HTTP:
```
curl -X POST https://votre-instance-n8n.com/webhook/xxxx \
  -H "Content-Type: application/json" \
  -d '{"accountId": "1234567", "containerId": "7654321", "conversionId": "AW-123456789", "conversionLabel": "AbCdEf-123"}'
```

## Comprendre le rapport

Le workflow génère un rapport détaillé avec les sections suivantes:

- **meta**: Informations générales (statut, horodatage, URL pour vérification manuelle)
- **conversion**: Détails de la conversion configurée
- **result**: Résultat des opérations (création/mise à jour de balises, déclencheurs associés)
- **publication**: Détails sur la publication (si activée)
- **message**: Résumé en langage naturel des actions effectuées

Exemple de rapport:
```json
{
  "meta": {
    "status": "OK",
    "timestamp": "2025-04-15T12:00:00.000Z",
    "reportUrl": "https://tagmanager.google.com/#/container/accounts/1234567/containers/7654321/workspaces/1/tags"
  },
  "conversion": {
    "id": "AW-123456789",
    "label": "AbCdEf-123"
  },
  "result": {
    "initialTagFound": true,
    "created": false,
    "updated": true,
    "triggersAssociated": ["12345", "67890"]
  },
  "publication": {
    "versionCreated": true,
    "versionId": "15",
    "versionName": "Version créée par MCP Autonome - 2025-04-15T12:00:00.000Z",
    "published": true,
    "publishStatus": "PUBLISHED"
  },
  "message": "🔄 Balise de conversion mise à jour avec succès et associée aux déclencheurs. Les modifications ont été publiées automatiquement."
}
```

## Dépannage

Si vous rencontrez des problèmes:

1. **Erreurs d'authentification**:
   - Vérifiez que vos identifiants OAuth2 sont correctement configurés
   - Assurez-vous que les portées nécessaires sont incluses

2. **Erreurs d'exécution**:
   - Vérifiez que les IDs GTM et Google Ads sont corrects
   - Assurez-vous d'avoir les droits suffisants sur le conteneur GTM

3. **Aucun déclencheur créé**:
   - Vérifiez les logs du nœud "Analyser les déclencheurs"
   - Assurez-vous que les types d'événements sont correctement configurés

4. **La publication échoue**:
   - Vérifiez que vous avez les droits de publication sur le conteneur
   - Consultez les logs du nœud "Publier version"