# 📄 Rapport TP : Mise en place d'un NAS avec NFS

## 🎯 Objectif du TP

L'objectif de cette première partie est de mettre en place un **NAS (Network Attached Storage)** en utilisant le protocole **NFS (Network File System)** permettant le partage d'un répertoire entre deux machines Linux :

* Serveur NFS : partage le dossier
* Client NFS : accède au dossier partagé et peut lire/écrire

---

## 🖥️ Configuration utilisée

| Rôle        | Machine    | Adresse IP    | OS            |
| ----------- | ---------- | ------------- | ------------- |
| Serveur NFS | nfs-server | 192.168.56.10 | Ubuntu Server |
| Client NFS  | nfs-client | 192.168.56.11 | Ubuntu Client |

---

## ✅ 1. Installation du service NFS

### 🔹 Sur le serveur

```bash
sudo apt update
sudo apt install nfs-kernel-server -y
```

**📸 Screenshot : installation du serveur NFS**

<img width="1201" height="410" alt="image" src="https://github.com/user-attachments/assets/12b538cb-fab4-4fac-a0d4-ce695c95b78e" />


### 🔹 Sur le client

```bash
sudo apt install nfs-common -y
```

**📸 Screenshot : installation du client NFS**
<img width="1226" height="164" alt="image" src="https://github.com/user-attachments/assets/ac1c9381-d68a-4377-8500-b2cd4579c14a" />


---

## ✅ 2. Création du dossier partagé (Serveur)

```bash
sudo mkdir -p /srv/nfs_share
sudo chown nobody:nogroup /srv/nfs_share
sudo chmod 777 /srv/nfs_share
```

Ajout d'un fichier test :

```bash
echo "Bienvenue sur le NAS NFS" | sudo tee /srv/nfs_share/readme.txt
```

**📸 Screenshot : dossier partagé et fichier créé**

<img width="1244" height="114" alt="image" src="https://github.com/user-attachments/assets/c54e4b95-21e9-4a7d-ad5f-f7fe7c4fd485" />


---

## ✅ 3. Configuration du partage NFS

Édition du fichier `/etc/exports` :

```bash
sudo nano /etc/exports
```

Ajout de la ligne :

```bash
/srv/nfs_share 192.168.56.11(rw,sync,no_subtree_check)
```

Application de la configuration :

```bash
sudo exportfs -rav
sudo systemctl restart nfs-kernel-server
```

**📸 Screenshot : export NFS actif**

<img width="1252" height="70" alt="image" src="https://github.com/user-attachments/assets/f69ea1c1-59fb-4ed8-80c1-a3e70cbf7762" />


---

## ✅ 4. Montage du partage sur le client

```bash
sudo mkdir -p /mnt/nfs
sudo mount 192.168.56.10:/srv/nfs_share /mnt/nfs
```

Vérification :

```bash
ls /mnt/nfs
```

**📸 Screenshot : accès au partage NFS depuis le client**

<img width="1226" height="78" alt="image" src="https://github.com/user-attachments/assets/ac96a1b1-8f5b-4ddd-ab77-e67998c559e9" />


---


## ✅ Conclusion

Dans cette première partie du TP, nous avons réussi à :

✅ Installer et configurer un serveur NFS
✅ Partager un dossier sur le réseau
✅ Monter ce partage sur un client
✅ Vérifier l'accès en lecture et écriture

Ce mécanisme permet un partage simple de fichiers au sein d'un réseau local.


