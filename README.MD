

## PROJET : ARCHITECTURE RESEAU ET SECURITE ##


Année 2020 / 2021

Ce dossier technique a pour but de montrer comment mettre en place un
routeur pour administrer son réseau en y ajoutant diverses
fonctionnalités tel qu'un serveur DHCP, un portail captif etc\...

Nous avons choisi **pfSense** car c'est un routeur/pare-feu open source
avec une documentation très complète. Il a pour but d'assurer la
sécurité périmétrique. Il comporte l'équivalent libre des outils et
services utilisés habituellement sur des routeurs professionnels.

INSTALLATION ET CONFIGURATION DE PFSENSE

Avant de commencer à configurer pfSense, nous allons voir comment
l'installer. Pour se faire on se rend sur
<https://www.pfsense.org/download/> et on télécharge la version
souhaitée.

Une fois l'image téléchargé on se rend dans VMWare et on définit les
paramètres suivant lors de la création de la machine virtuelle :

-   «** Create a New Virtual Machine** »

    -   « **Custom (Advanced)** »

    -   Hardware Compatibility: « **Workstation 11.x ou Workstation
        12.x** »

    -   Install From: « **I will install the operating system later** »

    -   Select a Guest Operating System: « **Other à FreeBSD 64-bit** »

    -   Virtual Machine Name : « **pfSense** »

    -   Processors :

        -   Numbers of Processors : « **1** »

        -   Number of Cores per Processor: « **1** »

    -   Memory for this Virtual Machine: « **512 MB** »

    -   Network Connection: « **Use Bridged Networking** »

    -   SCSI Controller : « **LSI Logic (Recommended)** »

    -   Virtual Disk Type : « **IDE ou SCSI** »

        -   **Create a New Virtual Disk**

        -   Max Disk Size: « **5 GB** » « **Store Virtual Disk as a
            single file** »

Votre VM s'affiche sur l'interface VMware, cliquez sur « **Edit virtual
machine setting **», sélectionnez la ligne « **Sound Card **» et cliquez
sur le bouton « **Remove **» en bas. Sélectionnez la ligne « **USB
Controller** » et décochez toutes les cases. Sélectionnez la ligne
« **CD/DVD** » puis cochez la case « **Use ISO image File** », cliquez
sur « **Browse... **» et sélectionnez l'ISO de pfSense que vous avez
sans doute téléchargé. On peut maintenant installer pfSense. Au
redémarrage, on y applique les paramètres suivants :

« **Boot Multi User** » accepter les conditions

-   Choisir le clavier « **French ISO-8859-1** »

-   Montez d'un cran pour continuer avec le clavier **«iso.kbd
    keymap »**

-   Partitioning: «** Auto (UFS)  Guided Disk Setup** »

-   Manual Configuration « **No** »

Au démarrage, on à une suite de questions, voici les paramètres à
appliquer :

![](mediafolder/d03d76ecae50fbc6aba3a71d312c2ca6799de5a8.png)

Etant donné que nous possédons qu'une seule carte réseau, nous allons en
ajouter une autre. Pour se faire, on édite les paramètres de la machine
virtuelle pfSense. En bas, cliquez sur le bouton « **Add** »
sélectionnez « **Network Adapter** » et valider.

Relancer pfSense pour prendre en compte les nouvelles cartes réseaux
qu'on vient de rajouter. Nous allons maintenant les configurer sous
pfSense.

-   Tapez « **1** » pour *Assign Interfaces* et y appliquer les
    paramètres suivants

```{=html}
<!-- -->
```
-   Should VLANs be set up now → **n**

-   Enter the WAN interface name or 'a' for auto-detection: **em0**

-   (em1 or nothing if finished): **em1**

Sur votre PC *(machine hôte),* lancez une invite de commande (CMD) et
tapez «** ipconfig /all** ». Il faut donc identifier l'**adresse IP de
votre PC** et **la passerelle.**

Vous ne pouvez pas utiliser l'adresse de votre PC pour l'interface **WAN
de pfSense** car justement votre PC l'utilise déjà. Il faut donc choisir
une autre adresse.

J'ai donc choisi **192.168.1.26 **; le masque et la passerelle ne change
pas.

> Nous allons désormais définir les adresses du WAN et du LAN :

-   Tapez « **2 **» pour « **Set interface(s) IP address **»

    -   Enter the new WAN IPv4 address: **192.168.1.26**

    -   Enter the new WAN IPv4 subnet: **24**

    -   Configure IPv6 address WAN interface via DHCP6 : **n**

    -   Do you want to revert to HTTP as the webConfigurator protocol:
        **y**

L'interface **WAN** est prête, vous pouvez même faire un test avec la
commande PING.

-   Tapez « **7** » pour «** Ping host** »

-   Tapez : **google.fr**

-   Où : **8.8.8.8**

Pour l'interface LAN, c'est la même procédure :

-   Tapez « **2 **» pour « **Set interface(s) IP address **»

    -   Enter the new LAN IPv4 address: **192.168.1.1**

    -   Enter the new LAN IPv4 subnet: **24**

    -   Do you want to enable DHCP server on LAN: **n**

    -   Do you want to revert to HTTP as the webConfigurator protocol:
        **y**

C'est bon pour le réseau **LAN (LAN-Serveurs)**. L'interface web de
pfSense est accessible à l'adresse **http://192.16.1.1/** avec les
identifiants par défaut suivant de pfSense :  **admin -- pfsense **

On obtient un résultat similaire à ça :

![](mediafolder/1a6905f6289c9f973322f1918569f591720aeacd.png)

C'est terminé pour l'installation et la configuration à partir de la VM,
désormais tout se fera à partir de l'interface Web de pfSense. Je vais
en profiter pour installer **Windows Education N** et le configurer sur
le réseau **LAN**, je pourrais ensuite me connecter à l'interface web de
pfSense.\
Pour se faire, on accède au paramètre de notre VM Client :

![](mediafolder/964b3faad6c7b8ae68d768696ec916cabd35e2c7.png)

Notre VM Client fait désormais partie du même réseau que notre routeur.

On se connecte à l'interface pfSense à l'adresse suivante *(dans mon
cas)* : <http://192.168.1.1>

Par défaut : user : **admin ;** password : **pfsense**

Une fois connecté, cliquez sur *\<Next>* pour procéder à une
configuration initiale. C'est facultatif, vous pouvez aussi cliquer sur
le logo pfSense pour atteindre le tableau de bord.

-   À l'étape 3/9, « **Time Server Information** », sélectionnez
    Europe/Paris dans « **Timezone** » laissez le reste par défaut.

-   À **l'étape 6/9**, mettez un mot de passe pour le **compte admin**.

-   Vous pouvez mettre pfSense en Français depuis le menu **System /
    General Setup** .

> ![](mediafolder/fc59ec9406e09c5134af371d0047e0d19ef6c207.png)

MISE EN PLACE DU SERVICE DHCP

Dans votre interface pfSense, cliquez sur le menu **Services**, puis sur
**DHCP Server**. Vous aurez deux possibilités :

-   Mise en place d'un DHCP sur la partie WAN

-   Mise en place d'un DHCP sur la partie LAN

```{=html}
<!-- -->
```
-   Le plus intéressant, c'est de mettre en place un **DHCP dans la
    partie LAN** ; cliquez donc sur « LAN », juste en dessous du bandeau
    gris.

    -   On se rend dans « **Services → DHCP Server → LAN** »

    -   Pour commencer, nous cochons évidemment la case \"Enable DHCP
        server on LAN interface\".

    -   On définit la tranche d'IP attribuable sur notre réseau. Dans
        notre cas, nous autorisons des connexions de 192.168.1.10 à 50

![](mediafolder/313845a3719ff7474c2c3b94a9c592f2dad26ee9.png)

On peut vérifier le fonctionnement du serveur DHCP en se rendant dans
« **Status → DHCP Leases** »

![](mediafolder/374a456d73ee3bdaa5d414b12c5595011501e16a.png)

On aperçoit bien que ma machine soit connectée en utilisant la première
adresse de la range que nous avons attribué, c'est-à-dire
**192.168.1.10**

MISE EN PLACE DU SERVEUR DNS

-   La configuration du serveur DNS se fait via le menu « **Services /
    DNS Resolver** »

    -   On commence par activer le service (on coche la case *Enable*).

> ![](mediafolder/96ee5e36a1658f63caf38daa084b57abd4a0a1dc.png)

-   On active la vérification des domaines, il suffit de cocher *DNSSE*.

> ![](mediafolder/6df0081a2d8a0e9d4fc1f506fb8c0fa4093d1e8d.png)

-   Ne reste qu'à s'assurer que le serveur effectue ses requêtes plutôt
    que les transmettre à d'autres serveurs. Pour ça, on décoche DNS
    Query Forwarding.

> ![](mediafolder/ae3d3a46f018b4a0e55b6aea317b6b8bcd7d01eb.png)

Pour connaître l'état du serveur, vous pouvez passer par le menu
« **Status / DNS Resolver** ». Cet écran vous liste les serveurs
*racines* qu'il connaît. La liste est longue mais montre que,
globalement, tout marche correctement.

![](mediafolder/fa886e47646d292092926134ac144c51a1388bc0.png)

MISE EN PLACE D'UN PORTAIL CAPTIF

-   On commence par la configuration du portail captif :

    -   On se rend dans « **Services → Captive Portal** »

    -   On ajoute un portail captif en apppuyant sur le « **+** » 

![](mediafolder/e44fa8712070e1f0d7ff6e0121661f235763283c.png)

-   Activer « **Enable Captive Portal** » et sélectionner l'interface
    « **LAN** »

-   Maximum concurrent connections : **1** (Limite le nombre de
    connexions simultanées d'un même utilisateur)

> ![](mediafolder/fdb1225f774452cdd57e1f756f933e7b557447a8.png)

-   Activer « **Enable logout popup window** » (une fenêtre popup permet
    aux clients de se déconnecter)

-   Définir « **Pre-authentication Redirect URL** » (URL de redirection
    par défaut. Les visiteurs ne seront redirigés vers cette URL après
    authentification que si le portail captif ne sait pas où les
    rediriger)

-   [**Note**]{.ul} : Avec « **http://.....** » devant le domaine :
    Exemple : <http://www.google.fr>

-   Définir « **After authentication Redirection URL** » (URL de
    redirection forcée. Les clients seront redirigés vers cette URL au
    lieu de celle à laquelle ils ont initialement tenté d'accéder après
    s'être authentifiés)

-   [**Note**]{.ul} : Avec « **http://.....** »devant le domaine :
    Exemple : <http://www.google.fr>

-   Activer « **Disable Concurrent user logins** » (seule la connexion
    la plus récente par nom d'utilisateur sera active)

-   Activer « **Disable MAC filtering** » (nécessaire lorsque l'adresse
    MAC du client ne peut pas être déterminée)

> ![](mediafolder/b18bd2d11a6992181f7b21fcc66f7a805b3df310.png)

-   Sélectionner «**Use an Authentication backen** »

-   Sélectionner «**Local Database**» pour «**Authentication Server**»

-   **Attention** : Ne pas sélectionner «**Local Database** » pour
    «**Secondary Authentication Server**»

-   Activer « **Local Authentication Privileges** » (Autoriser
    uniquement les utilisateurs avec les droits de « Connexion au
    portail captif ») puis **Save**

On obtient ce résultat :

![](mediafolder/4b428c6aff77298c6ea34529ac867b07afbc7447.png)

**→** Création d'un groupe et utilisateur qui aura pour fonction de
créer des Utilisateurs autorisés a se connecter au Portail Captif. Ce
groupe et utilisateurs associés auront seulement le droit de créer des
Utilisateurs du Portail Captif.

-   Dans « **Groups** », cliquez sur « **Add** »

-   Renseignez le **Nom du Groupe** "Agent" et sa **description**
    "Delegation Creation Utilisateurs Portail". Cliquez "**Save**"

-   ![](mediafolder/31ede21bf43b493439c0256763f72302dc34d9b3.png)

-   Dans le menu « **Actions** », **modifier le groupe** créé en
    cliquant sur le stylo

-   ![](mediafolder/42792c8bdc6b975fea9d97b6eaec0403c1e083fb.png)

    -   Cliquez sur « **Add »** puis « **Assign Privileges** »

    -   Sélectionnez dans la liste « **WebCfg -- System: User
        Manager** » (Accès à la page de gestion des utilisateurs "User
        Manager") 

    -   Sélectionnez dans la liste « **WebCfg -- Status: Captive
        Portal** » (Voir le Status des utilisateurs connectés")

    -   **Vérifier les droits**, puis cliquez sur « **Save** »

-   Dans « **Users** », cliquez sur « **Add** »

    -   Entrer un **Nom d'Utilisateu**r "agent", son **mot de passe** et
        sa **description** (Agent autorisé à créer des utilisateurs du
        Portail Captif).

    -   Sélectionner dans « **Group membership** » le groupe « Agent »
        précédemment créé. Cliquez sur « **Move to Member of list** »
        puis « **Save** »

La délégation pour l'utilisateur "Agent" est autorisé a créer des
utilisateurs pour connexion au Portail Captif

![](mediafolder/a5b7e4a30a8d6f61016a97dc26261300e8f6a947.png)

**Configuration du Groupe et Utilisateurs autorisés à se connecter au
Portail Captif**

Ce groupe et utilisateurs associés auront seulement le droit d'utiliser
le Portail Captif.

Onglet « **Groups** », cliquez sur « **+ Add** »

![](mediafolder/93621de4861b8621eda6bfa587c314ba0e8fd5e9.png)

Renseigner le **Nom du Groupe** « Portail » et sa **description**
« Utilisateurs du Portail ». Cliquez « **Save** »

Dans le menu « Actions », **modifier** le groupe créé en cliquant sur le
stylo

Cliquez sur « **+ Add** » rubrique « **Assigned Privileges** ».

Sélectionnez dans la liste « **User -- Services: Captive Portal
login** » (Autorisé seulement à se connecter au Portail Captif) puis
sauvegardez

Onglet « **Users** », cliquez sur « **+ Add** »

Entrer un **Nom d'Utilisateur** « testportail », son **mot de passe** et
sa **description** : « Un Utilisateur du Portail ».

Sélectionner dans « **Group membership** » le groupe « Portail »
précédemment créé. Cliquez sur « **Move to Member of list** » puis
« **Save** »

![](mediafolder/bb83f6ceca2d05c3cc0632def37669cbf7eef5aa.png)

L'utilisateur testportail est désormais autorisé à se connecter au
portail captif

→**Connexion avec le Compte "agent"**

Cet utilisateur a seulement le droit de créer des Utilisateurs du
Portail Captif par délégation et de voir le Statut des utilisateurs
connectés.

![](mediafolder/c5147759622499a5b1e3f2f81e3a5cb4027ac925.png)

Pour personnaliser le portail captif, on se rend dans « **Services** »,
« **Captive Portal** » puis on modifie la configuration du portail.

Rubrique « **Captive Portal Login Page** » : Activer « **Enable Custom
Logo Image** » puis parcourir pour sélectionner votre image

![](mediafolder/4730d0666b292c7f608b01d9352894e169a146ab.png)

Lors de la prochaine connexion, les utilisateurs auront le droit à cette
interface de connexion :

![](mediafolder/c23bf43155cbaccf9bf5cd3180ff2622039b6443.png)

-   Pour que le portail s'affiche en français :

    -   Sélectionner « **Diagnostics** » puis « **Edit File** »

    -   Tapez : /usr/local/captiveportal puis « Browse » puis cliquez
        sur « index.php »

![](mediafolder/dd6861f7c75be8fc1184f31e734611aae5958a13.png)

Rechercher (Ctrl + F) « You are connected » → Remplacer par : « Vous
êtes connecté »

![](mediafolder/a9178954ee12c8c08ca1049023b4005f83d77979.png)

Rechercher (Ctrl + F) « Disconnecting... » et « You have been
disconnected » -- Remplacer par « Déconnexion... » et « Vous êtes
déconnecté »

![](mediafolder/960873ada1f49c4bf9a06daf11d83d5ebf5d2ce1.png)

Rechercher (Ctrl + F) « Invalid credentials specified » -- Remplacer par
« Les informations saisies sont invalides »

![](mediafolder/2d3282f945e8849d6ad055afcc43d98262aa59e7.png)

Tapez : /etc/inc puis « Browse » puis cliquez sur « captiveportal.inc »

Rechercher (Ctrl + F) « Captive Portal login Page » -- Remplacer par :
« Portail Ynov Project »

![](mediafolder/5c4c989b6625149791c8d6c992e8301db0712c49.png)

Rechercher (Ctrl + F) "User" et "Password" -- Remplacer par
"Utilisateur" et "Mot de Passe"

![](mediafolder/2d8afec3e781ddf5bd85bf4105b5798ba1bf735e.png)

On a terminé pour le portail captif. En résumé, nous avons mis un
système d'administration et nous avons modifié l'interface de connexion
des utilisateurs.

CONFIGURATION DE SQUIDGUARD ET MISE EN PLACE D'UNE BLACKLIST

Sélectionner "Services" et "SquidGuard Proxy Filter"

![](mediafolder/fe6e7a93e475690be2840c53bced92e91cb694c2.png)

Activer SquidGuard "**Enable**"

Activer "**Enable Log**" et "**Enable log rotation**"

![](mediafolder/0d4c704df1ab8d71a0cfb8d6ec5a4f34e731aba0.png)

Activer "**Enable Blacklist**" et inserer dans **Blacklist URL** :
http://dsi.ut-capitole.fr/blacklists/download/blacklists_for_pfsense.tar.gz

Puis cliquez sur "**Save**"

Onglet "**Blacklist**" : Cliquer sur "**Download**" pour télécharger les
listes de filtrage

Onglet "**Common ACL**", Cliquez, dans "**Target Rules List**" sur le "
**+** "

Sélectionner les catégories à bloquer (ou à autoriser)

Sélectionnez" Allow" pour "**Default access \[all\]**"

Cochez "**Do not allow IP addresses in URL**" et "**Use Safe Search
engine**"

Puis cliquer "**Save**"

MISE EN PLACE DE SNORT

Snort est un système de détection d\'intrusion libre publié sous licence
GNU GPL

Accédez au menu Pfsense System et sélectionnez l'option De gestionnaire
de paquets.

![](mediafolder/e646dca7992816141f845c85ac5624de1b5ca80b.png)

Sur l'écran du gestionnaire de paquets, accédez à l'onglet Paquets
disponibles.

Sur l'onglet Paquets disponibles, recherchez SNORT et installez le
paquet Snort.

![](mediafolder/c3233614ba780f3b2364eaefa9cb7a21270a9ec1.png)

Dans notre exemple, nous avons installé la version 3.2.9.10 du paquet
Snort.

Attendez la fin de l'installation Snort.

Accédez au menu Pfsense Services et sélectionnez l'option Snort.

Sur l'onglet Paramètres Global, localisez les règles d'abonné Snort et
effectuez la configuration suivante :

Activer Snort VRT - Oui\
Code Snort Oinkmaster - Entrez-vous OikCode

Si vous n'avez pas d'Oinkcode, accédez au [site Web
Snort,](https://www.snort.org/users/sign_up)créez un compte et obtenez
un Oinkcode gratuit.

![](mediafolder/b6d5c5be6ec239979f0656f89fa107e9f2389a3d.png)

Localiser la zone Paramètres de mise à jour des règles et effectuer la
configuration suivante :

\- Intervalle de mise à jour - Sélectionnez l'intervalle de mise à jour
souhaité\
- Heure de démarrage de mise à jour - Définir l'heure désirée pour
mettre à jour les règles Snort

![](mediafolder/cc5c070d836929fb4813ba7066c16975b46675a9.png)

Localiser la zone Paramètres généraux et effectuer la configuration
suivante :

Supprimer l'intervalle des hôtes bloqués - 1 heure\
Supprimer les hôtes bloqués après la désinstallation - Non\
- Conserver les paramètres snort après la désinstallation - Oui\
- Intervalle de mise à jour de démarrage/shutdown -- non

![](mediafolder/864766f774a945a9fadafd214bf440d564278564.png)

Sur l'onglet Interfaces Snort, cliquez sur le bouton Ajouter et
effectuez la configuration suivante.

Activer - Oui\
Interface - Sélectionnez l'interface désirée pour surveiller

![](mediafolder/02315ae9cf3b07dbf245c1d96a4a1fa4279c7a10.png)

Après avoir terminé la configuration, cliquez sur le bouton Enregistrer.

Sur l'écran des interfaces Snort, modifiez la configuration de
l'interface.

![](mediafolder/de44d3f067af61d2ebb9dab8da20e66d046a51d1.png)

Accédez à l'onglet Catégories Wan et effectuez la configuration suivante
:

Résoudre les débits - Oui\
- Utiliser la politique IPS - Oui\
Sélection des politiques IPS -- Connectivité

![](mediafolder/a48a50b5e2e1b195fe76d3bf939ea0d1ecbd7ae2.png)

On peut observer les dangers repérés par Snort dans Service → Snort →
Alert

![](mediafolder/10ac91f376e6e86a37f92dcd075cff8570b85575.png)
