
> [!info] Info
> **Nom :** Interpreter
> **IP :** 10.129.244.184
> **OS :** Linux / Windows
> **Difficulté :** 🟩 Easy / 🟨 Medium / 🟥 Hard / 🟪 Insane

---

## 🔍 1. Enumeration

### Nmap
Initial port scan to find open services:

![[Pasted image 20260315173338.png]]

**Interesting findings :**
- Port 22 : SSH (OpenSSH X.X)
- Port 80 : HTTP (Jetty)
- Port 443 : HTTPS (Jetty)


### Énumération Web (Port 80/443)
> Si un site web est présent, penser à ajouter l'IP dans `/etc/hosts`.

Recherche de répertoires cachés avec Gobuster / Feroxbuster :
```bash
gobuster dir -u http://10.10.11.XXX -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.txt
```

Recherche de sous-domaines avec ffuf :
```bash
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://10.10.11.XXX -H "Host: FUZZ.{{title}}.htb" 
```

---

## 🚪 2. Accès Initial (Foothold)

### Découverte de la vulnérabilité
*Explique ici ce que tu as trouvé (ex: un paramètre vulnérable à une LFI, un formulaire avec SQLi, un exploit public pour un service).*

- [ ] CVE trouvée : `CVE-XXXX-XXXX`
- [ ] Explication : ...

### Exploitation
Commandes ou script Python utilisé pour obtenir un shell :

```bash
# Commande du reverse shell ou de l'exploit
nc -lvnp 4444
```

> [!success] User Flag
> **Utilisateur :** `nom_utilisateur`
> **Flag :** `cat user.txt` -> `[Copier le flag ici ou mettre une capture]`

---

## ⬆️ 3. Élévation de privilèges (Privesc)

### Énumération Interne
*Ce que tu cherches une fois sur la machine (LinPEAS, WinPEAS, sudo -l, SUID, cronjobs).*

```bash
# Exemple : vérification des droits sudo
sudo -l
```
*Résultat : l'utilisateur peut lancer `/usr/bin/python` en tant que root sans mot de passe.*

### Exploitation
*Comment tu es passé de User à Root/System.*

```bash
sudo python -c 'import pty; pty.spawn("/bin/sh")'
```

> [!danger] Root Flag
> **Utilisateur :** `root` / `NT AUTHORITY\SYSTEM`
> **Flag :** `cat /root/root.txt` -> `[Copier le flag ici ou mettre une capture]`

---

## 🔑 4. Loot & Credentials trouvés

*Garde une trace des identifiants trouvés, très utile pour les mouvements latéraux.*

| Service/Emplacement | Utilisateur | Mot de passe / Hash |
| ------------------- | ----------- | ------------------- |
| Database config     | admin       | password123         |
| /etc/shadow         | root        | $6$hash...          |

---

## 💡 5. Conclusion & Apprentissage

> [!abstract] Ce que j'ai appris
> - Concept 1 (ex: Comment exploiter une LFI vers RCE via les logs SSH).
> - Concept 2 (ex: Utilisation de Chisel pour du port forwarding).

**Ressources utiles :**
-[Lien vers un article de blog qui t'a aidé]
- [Lien vers PayloadAllTheThings ou GTFOBins]
```

### 💡 Quelques astuces pour Obsidian :
1. **Les variables `{{title}}` et `{{date}}`** : Si tu utilises le plugin de base "Modèles" (Templates) d'Obsidian, elles se remplaceront automatiquement par le nom de ton fichier et la date du jour.
2. **Insérer des images** : Quand tu prends des captures d'écran, tu peux juste faire `Ctrl+V` directement sous les sections pour garder une trace visuelle de tes exploits.
3. **Mise en page** : Les sections `> [!success]` ou `>[!danger]` vont générer des blocs colorés esthétiques en mode lecture, parfaits pour mettre en évidence les flags !