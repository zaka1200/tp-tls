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

## Usage des Certificats

1.  le QR code n’est qu’un support technique permettant de représenter des informations de manière lisible par des dispositifs électroniques .Un certificat Covid-19 est un document qui atteste d’une information officielle relative au statut sanitaire d'une personne, par exemple son statut vaccinal, un test négatif récent ou une guérison. Il est signé numériquement par une autorité certifiante, ce qui permet de garantir son authenticité et son intégrité.
2.  Les autorités de certification (CA) dans le cadre des certificats Covid-19 sont désignées au niveau national dans chaque pays membre de l'Union Européenne. Chaque pays dispose d’une infrastructure de gestion des certificats (PKI) pour signer les certificats Covid.
3.  on peut dire que la sécurité opérationnelle des certificats Covid-19 repose sur plusieurs principes :
    - Signature numérique : Les certificats sont signés par une autorité de certification officielle, garantissant leur authenticité.
    - Protection des données : Le certificat contient uniquement les informations strictement nécessaires et est conçu pour minimiser la collecte de données personnelles.
4. Les certificats Covid-19 et les certificats SSL ont des points communs, comme l’utilisation de la cryptographie, mais ils servent à des fins très différentes. Un certificat SSL assure une connexion sécurisée sur internet, protégeant les échanges entre un site web et ses utilisateurs, et il est valable pendant un à deux ans. Le certificat Covid-19, quant à lui, prouve le statut sanitaire d'une personne, comme une vaccination ou un test, mais sa durée est plus courte, limitée à la validité du test ou de la vaccination. Enfin, chaque type de certificat s’appuie sur des systèmes de vérification bien distincts : l’un dans le domaine du web, l’autre dans celui de la santé publique.

## Utilisation des Sockets SSL

### Démarrage du serveur SSL sans certificat

```cmd
Utilisation des Sockets SSL
```

Cette commande crée un serveur SSL en écoute sur le port par défaut (4433). Cependant, comme l'option -nocert est utilisée, aucun certificat n'est fourni au serveur.

![image](https://github.com/user-attachments/assets/a6dfc551-5cda-4727-8578-aad4067c410d)

### Connexion du client au serveur SSL

```cmd
openssl s_client -connect 127.0.0.1:4433 -showcerts
```

![image](https://github.com/user-attachments/assets/e786f633-1050-4e4f-af47-ba39e9f154a6)

La connexion SSL s'est bien établie, mais elle est anonyme, sans certificat serveur. Ce comportement est attendu, car la commande -nocert a probablement été utilisée pour lancer le serveur. Cela signifie que même si la session est chiffrée, aucune authentification n'a été réalisée pour vérifier l'identité du serveur.

### Connexion sans authentification

essayons une connexion sans authentification en utilisant une suite cryptographique anonyme, comme SSL_DH_anon_WITH_3DES_EDE_CBC_SHA. Cette suite permet une communication cryptée sans vérifier l'identité des parties.

on trouve qu il ya pas de cipher  SSL_DH_anon_WITH_3DES_EDE_CBC_SHA dans la liste des ciphers openssl don on va utiliser une autre suite de chiffrement anonyme (ADH-AES128-SHA).

```cmd
openssl s_server -nocert -cipher ADH-AES128-SH
```

![image](https://github.com/user-attachments/assets/281c973c-5e54-4f9c-bead-0afd84589b91)

connection du client

![image](https://github.com/user-attachments/assets/58b54a2f-bce7-45a1-9825-de358240ee73)

### Utilisation d'autres suites SS

![image](https://github.com/user-attachments/assets/56b2ef37-8c65-4d0f-ba69-7f9d71c3437b)

Du côté client

![image](https://github.com/user-attachments/assets/a5734112-a333-4b10-9aef-8baa2950ba1d)

la vérification de certificat est toujours désactivée
