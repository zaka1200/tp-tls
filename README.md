# tp-tls

## Analyse textuelle

Analyse du certificat **godard.pem** :

Le certificat a été signé par GEANT Personal CA 4

![image](https://github.com/user-attachments/assets/50b3e322-6ee0-46d9-8540-eb0d84ec0cde)

chaque certificat est entre les balises -----BEGIN CERTIFICATE----- et -----END CERTIFICATE-----

## Certificats et le Certificat de Votre Enseignant 

Cette commande affiche le contenu du certificat en format PEM (ici, godard1.pem)

```cmd
openssl x509 -text -in godard1.pem
```

![image](https://github.com/user-attachments/assets/82bc276a-7ee5-4e66-bbf6-073e4ebada16)

Même chose que la commande précédente, mais pour le certificat de l'autorité de certification (CA), ici le fichier **geant.pem** :

```cmd
openssl x509 -text -in geant.pem
```

![image](https://github.com/user-attachments/assets/c656ae6d-b147-4648-9d4c-e280f7b5aea0)

Cette commande vérifie la validité du certificat **godard.pem** en utilisant la chaîne de confiance présente dans **geant.pem** :

```cmd
openssl verify -CAfile geant.pem -purpose any godard1.pem
```

cela signifie que la clé publique de notre enseignant est authentique, car elle a été signée par une autorité de certification de confiance (GÉANT).

![image](https://github.com/user-attachments/assets/fe4893d9-a45c-4547-854d-0d697af0625a)

## Certificats des étudiants :

Pour générer une clé et créer une requête de signature pour notre certificat, on commence par Créer une paire de clés privée et publique

```cmd
openssl genpkey -algorithm RSA -out z.key.pem -pkeyopt rsa_keygen_bits:2048
```
![image](https://github.com/user-attachments/assets/8c6c82cb-6eb3-4065-b7a9-7c6b227a8df0)

maintenant on peux lire notre cle privee :

```cmd
cat z.key.pem
```

![image](https://github.com/user-attachments/assets/4afc3690-867b-4c46-9d36-75e1aef444bd)

Ensuite, créez une requête de signature de certificat (CSR) avec la commande suivante.

```cmd
openssl req -new -key z.key.pem -out z.csr
```

![image](https://github.com/user-attachments/assets/a4a9e68a-fe9f-4bc7-89e2-a4274a8d2e86)

apres on depose la requête sur le répertoire partagé

Pour vérifier le certificat, on utilise la commande suivante

```cmd
openssl x509 -text -in z.tp.pem
```

![image](https://github.com/user-attachments/assets/787145c2-90bd-4acd-bd63-c09a4fe3e552)
