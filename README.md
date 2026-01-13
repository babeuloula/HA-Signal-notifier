# Signal Notifier pour Home Assistant

Cette intégration permet d'envoyer des notifications via Signal en utilisant une instance de `signal-cli-rest-api`.

## Fonctionnalités
- Configuration simplifiée de l'URL de l'API via l'interface utilisateur.
- Service dédié `signal_notifier.send_message` pour un envoi facile.
- Support du multi-destinataires via séparation par points-virgules.
- Service de notification standard `notify.signal_notifier`.

## Prérequis
Vous devez disposer d'une instance de [signal-cli-rest-api](https://github.com/bbernhard/signal-cli-rest-api) fonctionnelle.
- Une URL vers l'API (ex: `http://192.168.1.10:8080`)
- Un numéro de téléphone enregistré sur votre instance Signal.

## Installation

### Via HACS (Recommandé)

1. Assurez-vous que [HACS](https://hacs.xyz/) est installé et fonctionnel sur votre instance Home Assistant.
2. Ouvrez HACS dans Home Assistant.
3. Cliquez sur les trois points en haut à droite et sélectionnez **Dépôts personnalisés**.
4. Collez l'URL de ce dépôt : `https://github.com/babeuloula/HA-Signal-notifier`
5. Sélectionnez la catégorie **Intégration** et cliquez sur **Ajouter**.
6. Vous devriez maintenant voir "Signal Notifier" dans la liste. Cliquez sur **Télécharger**.
7. Redémarrez Home Assistant.

### Installation Manuelle

Si vous ne souhaitez pas utiliser HACS, vous pouvez installer l'intégration manuellement :

1. Accédez au répertoire de configuration de votre instance Home Assistant (où se trouve `configuration.yaml`).
2. Si le dossier `custom_components` n'existe pas, créez-le.
3. Créez un sous-dossier nommé `signal_notifier` dans `custom_components`.
4. Copiez tous les fichiers du dossier `custom_components/signal_notifier` de ce dépôt vers le dossier que vous venez de créer sur votre serveur Home Assistant.
5. Redémarrez Home Assistant.

## Configuration

1. Dans Home Assistant, allez dans **Paramètres** > **Appareils et services**.
2. Cliquez sur le bouton **Ajouter une intégration** en bas à droite.
3. Recherchez **Signal Notifier**.
4. Remplissez les informations demandées :
   - **URL de l'API Signal** : L'adresse de votre serveur `signal-cli-rest-api` (ex: `http://192.168.1.10:8080`).
   - **Numéro d'expéditeur** : Le numéro de téléphone enregistré sur votre instance Signal qui enverra les messages.
   - **Nom d'utilisateur / Mot de passe (Optionnel)** : Si votre API est protégée par une authentification Basic Auth.

## Utilisation

L'intégration expose un service principal pour envoyer des messages.

### Service `signal_notifier.send_message`

C'est la méthode recommandée car elle offre des champs clairs dans l'interface utilisateur de Home Assistant.

**Paramètres :**
- `message` : Le texte à envoyer.
- `recipients` : Un ou plusieurs numéros de téléphone séparés par des points-virgules (`;`).
- `notify_self` : (Optionnel, défaut: `true`) Si `true`, envoie aussi le message à l'expéditeur.
- `text_mode` : (Optionnel, défaut: `styled`) `normal` ou `styled`. `styled` permet le formatage Signal.

**Exemple YAML :**
```yaml
service: signal_notifier.send_message
data:
  message: |
    Bonjour depuis Home Assistant !
    Ceci est un message sur plusieurs lignes.
  recipients: "+33612345678;+33600000000"
  notify_self: true
  text_mode: "styled"
```

### Service `notify.signal_notifier`

Le service utilise l'expéditeur configuré par défaut.

```yaml
service: notify.signal_notifier
data:
  message: "Bonjour !"
  target: "+33612345678;+33600000000"
  data:
    notify_self: true
    text_mode: "styled"
```

### Test local (sans Home Assistant)
Si vous souhaitez tester votre configuration sans lancer Home Assistant, consultez le guide [Test Local](TEST_LOCAL.md).
