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
Attention : le dossier peut être modifié, mais une mauvaise configuration des permissions peut provoquer des erreurs d’accès.
Création du dossier du site :
```bash
sudo mkdir -p workflow/nginx
```

Création de la page HTML :
```bash
sudo nano /var/www/monprojet/index.html
```

Contenu du fichier index.html :
```html
<!DOCTYPE html>
<html>
<head>
    <title>Mon site Nginx</title>
</head>
<body>
    <h1>Bienvenue sur mon site Nginx</h1>
    <p>Projet Administrateur système et réseau</p>
</body>
</html>
```

Configuration du Virtual Host (Server Block) :
```bash
sudo nano /etc/nginx/sites-available/monprojet
```

Contenu du fichier monprojet :
```nginx
server {
    listen 80;
    server_name monprojet.local;

    root /var/www/monprojet;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    access_log /var/log/nginx/monprojet_access.log;
    error_log /var/log/nginx/monprojet_error.log;
}
```

Activer le site :
```bash
sudo ln -s /etc/nginx/sites-available/monprojet /etc/nginx/sites-enabled/
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

Ajouter la ligne :
```
127.0.0.1 monprojet.local
```

Tester dans le navigateur : http://monprojet.local

Résultat attendu : tu dois voir ta page web avec le texte "Bienvenue sur mon site Nginx".

