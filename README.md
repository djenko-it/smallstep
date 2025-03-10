# Installation d'une PKI sous smallstep-ca

Installation de smallstep cli sous Debian

https://smallstep.com/docs/step-cli/installation/#debian-ubuntu

```
wget https://dl.smallstep.com/cli/docs-cli-install/latest/step-cli_amd64.deb
sudo dpkg -i step-cli_amd64.deb
```

Création du dossier secret

```
mkdir -p ./secrets
echo "votre_mot_de_passe_securise" > ./secrets/ca_password.txt
```

Lancer le docker

```
docker-compose up -d
```

# Récupération du fingerprint de la CA

```
docker exec smallstep-ca step certificate fingerprint /home/step/certs/root_ca.crt
```

```
step ca bootstrap --ca-url https://djenkoit.fr:9000 --fingerprint <FINGERPRINT>
```

Certificat Root (a deployer pour valider l'autorité de certification)
```
/root/.step/certs/root_ca.crt
```

Exemple sous Linux

```
cp root_ca.crt /usr/local/share/ca-certificates/
update-ca-certificates

update-ca-certificates
Updating certificates in /etc/ssl/certs...
rehash: warning: skipping ca-certificates.crt,it does not contain exactly one certificate or CRL
1 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
```

Authentification auprès de smallstep-ca
Pour générer des certificats, vous devez vous authentifier auprès de smallstep-ca. Utilisez la commande suivante :

```
step ca token <COMMON_NAME> --password-file=/path/to/password.txt
step ca token app1.djenkoit.fr --password-file=secrets/ca_password.txt
```

Générer un certificat
Utilisez le jeton d'authentification pour générer un certificat :

```
step ca certificate <COMMON_NAME> cert.crt cert.key --token=<TOKEN>
step ca certificate app1.djenkoit.fr cert.crt cert.key --token=eyJhbGciOiJFUzI1NiIsImtpZCI6Il
```

Vérifier le certificat
Vous pouvez vérifier le certificat généré avec la commande suivante :
```
step certificate inspect cert.crt
```

Renouveler un certificat
Les certificats générés par smallstep-ca ont une durée de validité limitée. Pour renouveler un certificat, utilisez la commande suivante :

```
step ca renew cert.crt cert.key --force
```