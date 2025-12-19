# Guide Rapide - TP Ansible

## 📦 Prérequis - Ce qu'il faut installer

### 1. WSL (Windows Subsystem for Linux)

**Vérifier si WSL est installé :**
```powershell
wsl --status
```

**Si pas installé :**
```powershell
# Dans PowerShell en administrateur
wsl --install
```
Redémarrer le PC après l'installation.

### 2. Ubuntu dans WSL

Après l'installation de WSL, Ubuntu devrait s'installer automatiquement. Si ce n'est pas le cas :

```powershell
wsl --install -d Ubuntu
```

### 3. Ansible

**Dans Ubuntu WSL :**
```bash
sudo apt update
sudo apt install ansible -y
```

**Vérifier l'installation :**
```bash
ansible --version
```

### 4. Python3 et PyMySQL (pour MySQL)

**Dans Ubuntu WSL :**
```bash
sudo apt install python3 python3-pymysql -y
```

### 5. UFW (Firewall Ubuntu - optionnel)

**Dans Ubuntu WSL :**
```bash
sudo apt install ufw -y
```

## ✅ Vérification de l'installation

```bash
# Vérifier WSL
wsl --status

# Vérifier Ansible
ansible --version

# Vérifier Python
python3 --version
```

## 🚀 Commandes essentielles pour utiliser le TP

### 1. Ouvrir WSL et aller dans le projet

```bash
# Ouvrir WSL
wsl

# Aller dans le projet
cd /mnt/c/Users/Martin/Documents/travail/master/ansible
```

### 2. Tester la connexion aux serveurs

```bash
ansible all -i inventory/hosts -m ping
```

### 3. Exécuter le playbook serveur web

```bash
ansible-playbook -i inventory/hosts playbooks/webserver.yml --ask-become-pass
```

**Ce que ça fait :**
- Installe Apache
- Crée le répertoire `/var/www/html`
- Crée une page d'accueil
- Démarre Apache

### 4. Exécuter le playbook base de données

```bash
ansible-playbook -i inventory/hosts playbooks/database.yml --ask-become-pass --ask-vault-pass
```

**Ce que ça fait :**
- Installe MySQL
- Configure le mot de passe root
- Crée la base de données `appdb`
- Crée l'utilisateur `appuser`

**Mots de passe demandés :**
- `BECOME password` : Votre mot de passe sudo Linux
- `Vault password` : Le mot de passe du vault Ansible (celui que vous avez créé avec `ansible-vault`)

### 5. Exécuter le playbook principal (orchestration complète)

```bash
ansible-playbook -i inventory/hosts playbooks/site.yml --ask-become-pass --ask-vault-pass
```

**Ce que ça fait :**
- Configure la base (paquets essentiels, fuseau horaire)
- Configure les serveurs web
- Configure les bases de données
- Configure les serveurs applicatifs

## 🔐 Gestion des secrets (Ansible Vault)

### Créer un fichier vault

```bash
ansible-vault create group_vars/dbservers/vault.yml
```

Entrez un mot de passe fort (à retenir !).

### Éditer un fichier vault

```bash
ansible-vault edit group_vars/dbservers/vault.yml
```

### Voir le contenu d'un vault

```bash
ansible-vault view group_vars/dbservers/vault.yml
```

## 📝 Commandes utiles

### Vérifier la syntaxe d'un playbook

```bash
ansible-playbook -i inventory/hosts playbooks/site.yml --syntax-check
```

### Mode simulation (dry-run)

```bash
ansible-playbook -i inventory/hosts playbooks/webserver.yml --check
```

### Exécuter avec plus de détails

```bash
ansible-playbook -i inventory/hosts playbooks/site.yml -vvv
```

### Lister les hôtes de l'inventaire

```bash
ansible all -i inventory/hosts --list-hosts
```

## 🎯 Résumé des commandes principales

| Action | Commande |
|--------|----------|
| **Tester connexion** | `ansible all -i inventory/hosts -m ping` |
| **Playbook web** | `ansible-playbook -i inventory/hosts playbooks/webserver.yml --ask-become-pass` |
| **Playbook DB** | `ansible-playbook -i inventory/hosts playbooks/database.yml --ask-become-pass --ask-vault-pass` |
| **Playbook complet** | `ansible-playbook -i inventory/hosts playbooks/site.yml --ask-become-pass --ask-vault-pass` |
| **Créer vault** | `ansible-vault create group_vars/dbservers/vault.yml` |
| **Éditer vault** | `ansible-vault edit group_vars/dbservers/vault.yml` |

## ⚠️ Notes importantes

1. **Toujours utiliser `-i inventory/hosts`** car Ansible ignore `ansible.cfg` dans WSL (répertoire world-writable)

2. **Mots de passe :**
   - `--ask-become-pass` : Demande votre mot de passe sudo Linux
   - `--ask-vault-pass` : Demande le mot de passe du vault Ansible

3. **Chemin du projet :**
   - Windows : `C:\Users\Martin\Documents\travail\master\ansible`
   - WSL : `/mnt/c/Users/Martin/Documents/travail/master/ansible`

## 🆘 Dépannage rapide

### Erreur : "ansible: command not found"
→ Ansible n'est pas installé. Installez-le avec `sudo apt install ansible`

### Erreur : "No inventory was parsed"
→ Ajoutez `-i inventory/hosts` à vos commandes

### Erreur : "Access denied"
→ Vérifiez que vous utilisez `--ask-become-pass` et entrez le bon mot de passe sudo

### Erreur : "Vault password"
→ Vous devez utiliser `--ask-vault-pass` pour les playbooks qui utilisent des secrets

---

**Prêt à démarrer !** 🚀

