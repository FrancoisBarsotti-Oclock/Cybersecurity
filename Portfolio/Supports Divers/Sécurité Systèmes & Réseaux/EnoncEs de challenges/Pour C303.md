# Pour Challenge C303 : VPN & Authentification Radius

**Contexte pro :**

Votre entreprise veut permettre l’accès distant sécurisé pour les équipes nomades. Les accès doivent être centralisés via l’Active Directory. Vous devez déployer un VPN client‑à‑site sur pfSense avec authentification RADIUS.

## Objectif principal

- Déployer un VPN client‑à‑site sur pfSense.
- Authentifier les utilisateurs via RADIUS (lié à l’AD).  
- Valider la connexion avec un compte AD.

## Étape 1 : Préparer l’environnement

- Vérifier les IPs et la connectivité entre pfSense, serveur RADIUS et AD.
- S’assurer que DNS et l’horloge sont corrects.

## Étape 2 : Configurer RADIUS (si pas déjà fait)

- Vérifier que RADIUS est bien lié à l’Active Directory.
- Tester une authentification AD depuis le serveur RADIUS.

## Étape 3 : Créer le VPN client‑à‑site
- Activer le service VPN sur pfSense (type au choix, ex: OpenVPN/IPsec).
- Définir le pool d’adresses VPN.

## Étape 4 : Activer l’authentification RADIUS

- Déclarer le serveur RADIUS dans pfSense.
- Associer l’authentification du VPN au RADIUS.

## Étape 5 : Définir les règles firewall

- Autoriser les flux nécessaires pour le VPN.
- Limiter les accès des clients VPN au strict besoin.

## Étape 6 : Tester un client

- Connexion avec un compte AD valide (succès).
- Test avec un compte invalide (échec).
- Vérifier l’accès aux ressources internes.


## Bonus : VPN site‑à‑site

- Interconnecter deux pfSense.
- Tester la communication entre les deux LANs.

#