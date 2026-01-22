![Titre Nginx](Images/Nginx-1024x425.jpg)

# Projet Nginx - Serveur Web

Objectif : Installer et configurer un serveur web Nginx, créer un site web statique et configurer un Virtual Host (Server Block).

Installation de Nginx :
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx -y
```

Vérifier le statut :
```bash
sudo systemctl status nginx
```
Configurer le pare-feu (facultatif mais recommandé)

```bash
sudo ufw allow 'Nginx HTTP'
sudo ufw status
```

Création du dossier du site :
```bash
sudo mkdir -p /var/www/monsite/html
sudo chown -R $USER:$USER /var/www/monsite/html
sudo chmod -R 755 /var/www/monsite
```
<p>
  <span style="color:red; font-weight:bold;">⚠️ Attention :</span>
  le dossier peut être modifié, mais une mauvaise configuration des permissions peut provoquer des erreurs d’accès,
  notamment si le dossier parent appartient à
  <code style="color:gray;">root</code>.
</p>

Création de la page HTML :
```bash
nano /var/www/monsite/html/index.html
```

Contenu du fichier index.html :
```html
<!DOCTYPE html>
<html>
<head>
    <title>Mon site Nginx</title>
</head>
<body>
    <h1>Bienvenue sur mon site web !</h1>
</body>
</html>
```
Enregistrez avec CTRL+O, puis quittez avec CTRL+X.

Créer un fichier de configuration Nginx :
Nginx utilise des fichiers de configuration pour chaque site dans /etc/nginx/sites-available/

```bash
sudo nano /etc/nginx/sites-available/monsite
```
Mettez ceci dedans (remplacez monsite par votre nom de domaine si vous en avez un)

Contenu du fichier monprojet :

```bash
server {
    listen 80;
    server_name monsite.com www.monsite.com;

    root /var/www/monsite/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Activer le site :
Créez un lien symbolique vers sites-enabled :
```bash
sudo ln -s /etc/nginx/sites-available/monsite /etc/nginx/sites-enabled/
```

Tester la configuration :
```bash
sudo nginx -t
```

Recharger Nginx :
```bash
sudo systemctl reload nginx
```

Ajouter l'entrée DNS locale (hosts) :
```bash
sudo nano /etc/hosts
```

Tester le site :
```
http://votre_ip
```
(Bonus)

Pour démarrer, arrêter ou redémarrer Nginx :
```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
```
Activer HTTPS avec Let's Encrypt
Installer Certbot :
```bash
sudo apt install certbot python3-certbot-nginx -y
```

```bash
sudo certbot --nginx -d monsite.com -d www.monsite.com

```
✅ Le site Nginx est maintenant opérationnel.
