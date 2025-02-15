# Gestion des mots de passe utilisateurs sous Linux

## Suppression d'un mot de passe utilisateur
Pour supprimer un mot de passe utilisateur sous Linux, utilisez la commande suivante en tant que superutilisateur (root) :

```bash
sudo passwd -d nom_utilisateur
```

### Explication :
- **`sudo`** : Exécute la commande avec des privilèges d'administrateur.
- **`passwd`** : Commande pour gérer les mots de passe.
- **`-d`** : Supprime le mot de passe de l'utilisateur.
- **`nom_utilisateur`** : Remplacez cela par le nom de l'utilisateur concerné.

### Conséquences :
- L'utilisateur pourra se connecter sans mot de passe, ce qui peut poser un risque de sécurité.
- Pour les connexions SSH, cela peut poser des problèmes si l'authentification sans mot de passe n'est pas configurée correctement.

---

## Problème avec la connexion SSH après suppression du mot de passe
Si après avoir supprimé le mot de passe, vous ne pouvez toujours pas vous connecter en SSH ou l'ancien mot de passe ne fonctionne plus, suivez ces étapes :

### 1. Configurer l'accès par clé SSH
Pour éviter d'utiliser un mot de passe, configurez une clé SSH :

#### Sur le client (votre ordinateur) :
Si vous n'avez pas encore de clé SSH, générez-en une :
```bash
ssh-keygen
```
Ensuite, envoyez la clé publique au serveur :
```bash
ssh-copy-id nom_utilisateur@adresse_ip_serveur
```
Cela configurera automatiquement l'accès avec une clé SSH.

---

### 2. Autoriser l'accès sans mot de passe (si nécessaire)
Modifiez la configuration du serveur SSH pour autoriser les connexions sans mot de passe.

#### Modifier le fichier de configuration :
```bash
sudo nano /etc/ssh/sshd_config
```
Recherchez et assurez-vous que les lignes suivantes sont correctement configurées :
```
PasswordAuthentication yes
PermitEmptyPasswords yes
```
⚠️ **Attention** : Autoriser les connexions sans mot de passe (`PermitEmptyPasswords yes`) est très risqué et généralement déconseillé. Utilisez des clés SSH si possible.

#### Redémarrez le service SSH :
```bash
sudo systemctl restart sshd
```

---

### 3. Réinitialiser le mot de passe (si nécessaire)
Si vous voulez rétablir un mot de passe pour l'utilisateur, utilisez :
```bash
sudo passwd nom_utilisateur
```
Cela vous permettra de définir un nouveau mot de passe et de l'utiliser pour la connexion SSH.

---

### Vérification des permissions (si problème avec les clés SSH)
Si vous utilisez un fichier de clé SSH et que la connexion échoue toujours, vérifiez les permissions sur le serveur :
```bash
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```
