# NumChat : Snapchat sur NumWorks

NumChat met tes conversations Snapchat sur une calculatrice NumWorks reliée en
USB à un ordinateur (Mac ou Windows). Tu lis tes messages, tu réponds depuis le
clavier de la calculatrice (avec les accents), tu vois les emojis, et tu peux
ouvrir les snaps photo et vidéo reçus. L'ordinateur fait passerelle avec
Snapchat Web, ouvert dans une fenêtre Chrome qui lui est dédiée.

- Liste des conversations, non-lus en premier
- Messages récents avec défilement, les tiens à droite, ceux des autres à gauche
- Saisie de texte avec majuscules, chiffres et accents (é è ê ë à ç ù…)
- Les nouveaux messages arrivent pendant que tu lis ou que tu écris
- Emojis affichés en couleur (14×14 pixels), y compris 👍🏽, 🇫🇷 ou 👨‍👩‍👧
- Snaps photo et vidéo affichés en plein écran, sur demande
- Les vocaux et autocollants s'affichent en une ligne : `[vocal]`, `[autocollant]`

NumChat fait partie de **[NumSocial](https://github.com/derim00/numsocial)** :
le même firmware contient aussi **[NumTube](https://github.com/derim00/numtube)**
(YouTube) et **[NumScreen](https://github.com/derim00/numscreen)**
(l'écran de l'ordinateur sur la calculatrice), et une seule appli, **NumSocial**,
installe celles que tu choisis.

> [!WARNING]
> **Ton compte Snapchat.** Piloter Snapchat Web automatiquement est contraire aux
> [conditions d'utilisation de Snapchat](https://snap.com/fr-FR/terms), qui
> interdisent l'accès par des « moyens automatisés ». Snapchat détecte les
> applis tierces et peut **verrouiller ton compte**, temporairement ou
> définitivement. NumChat limite ce risque (vrai Chrome, rythme humain, lecture
> passive de la page), mais ne peut pas le supprimer. **Commence avec un compte
> secondaire.**

## Sommaire

1. [Ce que NumChat fait et ne fait jamais](#ce-que-numchat-fait-et-ne-fait-jamais)
2. [Pourquoi un patch et pas un firmware](#pourquoi-un-patch-et-pas-un-firmware)
3. [Ce qu'il faut](#ce-quil-faut)
4. [Tutoriel : installation sans rien compiler (Windows ou Mac)](#tutoriel--installation-sans-rien-compiler-windows-ou-mac)
5. [Tutoriel : installation en compilant sur ton Mac](#tutoriel--installation-en-compilant-sur-ton-mac)
6. [Tutoriel : installer sur la calculatrice](#tutoriel--installer-sur-la-calculatrice)
7. [Tutoriel : première connexion à Snapchat](#tutoriel--première-connexion-à-snapchat)
8. [Utilisation](#utilisation)
9. [Comment ça marche](#comment-ça-marche)
10. [Tests sans calculatrice](#tests-sans-calculatrice)
11. [Limites connues](#limites-connues)
12. [Avertissements et licence](#avertissements-et-licence)

## Ce que NumChat fait et ne fait jamais

| NumChat fait | NumChat ne fait jamais |
| --- | --- |
| Ouvrir la conversation que tu choisis sur la calculatrice | Voir, taper ou stocker ton mot de passe ou tes codes : c'est toi qui te connectes |
| Taper et envoyer le texte que **tu** as écrit | Envoyer quoi que ce soit d'autre en ton nom |
| Ouvrir un snap quand **tu** appuies sur OK dessus | Ouvrir un snap tout seul |
| Fermer la conversation dans Snapchat Web quand tu la quittes (sinon les messages suivants seraient marqués « lus ») | Enregistrer une image, un snap ou une vidéo sur le disque |
| Garder ta session dans un profil Chrome à part | Toucher à ton Chrome habituel ou à ses mots de passe |

Tout passe en local, entre la calculatrice, le câble USB et ton ordinateur.

Quand tu ouvres un snap sur la calculatrice, il est **consommé** comme sur le
téléphone (une seule vue) et l'expéditeur voit « Ouvert ».

## Pourquoi un patch et pas un firmware

Depuis juillet 2021, le code d'Epsilon (le logiciel de la NumWorks) n'est plus
sous licence libre : NumWorks n'autorise plus sa redistribution. Ce dépôt ne
contient donc **aucun code ni aucun firmware de NumWorks**. Il contient :

| Dossier | Contenu |
| --- | --- |
| `patches/numsocial-epsilon.patch` | les modifications à appliquer à Epsilon : lien USB de Termia, applis NumChat et NumTube, choix des applis à l'installation |
| `EPSILON_COMMIT` | la version exacte d'Epsilon sur laquelle le patch s'applique |
| `scripts/build-firmware.sh` | télécharge Epsilon chez NumWorks, applique le patch, compile |
| `app/` | l'appli NumSocial : installeur et passerelle (page dans le navigateur) |
| `gateway/numchat_snap.py` | le pilote de Snapchat Web (Playwright) |
| `gateway/numchat_service.py` | le service NumChat de la passerelle (protocole, emojis, snaps) |
| `gateway/numchat_media.py` | emojis et images au format de l'écran de la calculatrice |
| `tests/fake_numchat.py` | une fausse calculatrice pour tester la passerelle |

Chacun récupère Epsilon depuis [le dépôt officiel de NumWorks](https://github.com/numworks/epsilon)
et compile le firmware sur sa propre machine, ou dans sa copie **privée** sur
GitHub. Le firmware compilé et l'appli qui l'embarque sont pour ton usage : ne
les partage pas, partage ce dépôt.

## Ce qu'il faut

- Une NumWorks **N0120** ou **N0115** avec la version officielle **26.4.0**
  (testé ; d'autres versions peuvent afficher « OFFICIAL UPGRADE REQUIRED »)
- Un câble USB qui transfère les données (pas seulement la charge)
- Un **Mac** (testé avec une calculatrice) ou un **PC Windows 10/11** (compilé
  et vérifié par GitHub, pas encore essayé avec une calculatrice)
- **Google Chrome** installé (sous Windows, **Microsoft Edge** sert de secours)
- Un compte Snapchat (de préférence secondaire, voir l'avertissement)

## Tutoriel : installation sans rien compiler (Windows ou Mac)

GitHub compile tout pour toi, dans **ta propre copie privée** de ce dépôt.

1. Connecte-toi à GitHub (un compte gratuit suffit).
2. En haut de cette page : **Use this template** > **Create a new repository**.
   Donne-lui un nom et coche **Private**. C'est obligatoire : la compilation
   refuse de tourner dans un dépôt public, pour que le firmware ne soit pas
   téléchargeable par tous.
3. Dans ta copie : onglet **Actions**. Si GitHub le demande, clique
   « I understand my workflows, go ahead and enable them ».
4. À gauche, **Build NumSocial**, puis à droite **Run workflow** > **Run workflow**.
5. Attends environ 15 minutes (pastille verte). Ouvre la compilation terminée,
   et en bas de la page, dans **Artifacts**, télécharge :
   - **NumSocial-windows** pour Windows,
   - **NumSocial-mac** pour Mac.
6. Décompresse le fichier téléchargé (deux fois si besoin : il contient un zip).
7. Lance l'appli :
   - **Windows** : `NumSocial\NumSocial.exe`. Si SmartScreen s'affiche :
     « Informations complémentaires » > « Exécuter quand même ».
   - **Mac** : clic droit sur `NumSocial.app` > **Ouvrir** > **Ouvrir**
     (macOS bloque les applis non signées la première fois).

Une page s'ouvre dans ton navigateur : passe à
[installer sur la calculatrice](#tutoriel--installer-sur-la-calculatrice).

## Tutoriel : installation en compilant sur ton Mac

Il faut `git`, `make`, `python3` et le kit de compilation ARM de NumWorks.

```bash
git clone https://github.com/derim00/numchat.git && cd numchat
./scripts/build-firmware.sh
```

Le script télécharge Epsilon, applique le patch et compile 4 images
(N0120/N0115, slots A/B), en quelques minutes chacune. S'il manque le
compilateur ARM, il affiche la commande du kit officiel de NumWorks à lancer :
lance-la, puis relance le script.

Fabrique ensuite l'appli et ouvre-la :

```bash
.venv/bin/python3 app/build.py
open app/dist/NumSocial.app
```

## Tutoriel : installer sur la calculatrice

1. L'appli NumSocial est ouverte et sa page est affichée
   (`http://127.0.0.1:8737`).
2. Branche la calculatrice. Elle doit afficher **« Calculatrice connectée »**.
   Sinon : appuie sur le bouton **reset** au dos (avec un trombone), puis
   rebranche le câble.
3. La page affiche « Calculatrice prête pour l'installation » et lit toute seule
   la **version officielle** (sinon : Paramètres > À propos > Version sur la
   calculatrice).
4. Dans **Applis à installer**, coche **NumChat** (et **NumTube**, **NumScreen**
   si tu les veux aussi).
5. Clique **Installer sur la calculatrice**. Ne débranche pas pendant
   l'installation (environ 1 minute).
6. La calculatrice redémarre sur un écran « UNOFFICIAL SOFTWARE » : c'est
   normal. NumChat est en bas de l'écran d'accueil.

Le firmware officiel n'est jamais effacé : NumSocial va dans l'autre
emplacement de la mémoire. Un **reset au dos** ramène le firmware officiel.

## Tutoriel : première connexion à Snapchat

À faire une seule fois (la session est gardée ensuite).

1. Laisse l'appli NumSocial ouverte et la calculatrice branchée.
2. Ouvre **NumChat** sur la calculatrice : il se connecte tout seul à
   l'ordinateur.
3. Une fenêtre **Chrome** s'ouvre sur l'ordinateur, sur Snapchat Web. C'est un
   profil Chrome à part, réservé à NumChat :
   - Mac : `~/Library/Application Support/NumChat/chrome-profile`
   - Windows : `%LOCALAPPDATA%\NumChat\chrome-profile`
4. **Connecte-toi toi-même** dans cette fenêtre : identifiant, mot de passe,
   code reçu ou validation sur ton téléphone.
5. Dès que tes conversations s'affichent dans la fenêtre, elles apparaissent sur
   la calculatrice.
6. Laisse cette fenêtre ouverte. Tu peux la mettre derrière tes autres fenêtres.

Pour te déconnecter : déconnecte-toi dans cette fenêtre Chrome, ou supprime le
dossier du profil.

## Utilisation

Laisse l'appli NumSocial ouverte sur l'ordinateur pendant que tu utilises NumChat.

### Liste des conversations

| Touche | Effet |
| --- | --- |
| `▲` `▼` | choisir une conversation |
| `OK` ou `EXE` | l'ouvrir |
| retour | quitter NumChat (écran d'accueil de l'appli) |

Les conversations non lues sont en jaune, avec un point, et passent en premier.
Sous le nom : le statut Snapchat (« Nouveau chat », « Reçu »…), ou le dernier
message lu pendant la session.

### Conversation

| Touche | Effet |
| --- | --- |
| lettres | alpha est verrouillé au départ |
| `alpha` | passer aux chiffres, et revenir aux lettres |
| `shift` | majuscule (sous alpha verrouillé : verrouillage majuscules) |
| `Ans` | juste après une lettre : fait défiler ses accents (e → é → è → ê → ë → e, a → à → â → ä, c → ç…) |
| `×10ˣ` | apostrophe |
| `⌫` | efface ; `shift` + `⌫` vide tout le message |
| `EXE` | envoie le message |
| `▲` `▼` | défile d'une ligne ; `◀` `▶` d'une page |
| `OK` | sans texte tapé : ouvre le snap marqué « OK : voir » (le plus bas à l'écran) |
| retour | revient à la liste |

L'indicateur en bas à droite rappelle le mode : `abc`, `ABC`, `a`, `A`, `123`.

### Snaps

- Un snap que tu peux voir s'affiche en jaune : `[snap] OK : voir`.
- `OK` l'ouvre en plein écran. Une vidéo défile sans son (la NumWorks n'en a
  pas), en petit et à quelques images par seconde.
- Le snap se ferme tout seul à la fin de son minuteur, ou avec retour.
- Il est alors consommé : l'expéditeur voit « Ouvert », et il ne peut plus
  être revu.

## Comment ça marche

```
NumWorks (appli NumChat)  ⇄ USB ⇄  appli NumSocial (passerelle)  ⇄  Chrome (profil NumChat)  ⇄  Snapchat Web
      clavier, écran                  protocole TMIA, emojis,            piloté par Playwright
                                      images des snaps                   (connect_over_cdp)
```

- **Navigateur** : la passerelle lance un vrai Google Chrome, normalement, avec
  un profil dédié et un port de débogage limité à `127.0.0.1`, puis s'y attache
  avec Playwright. La page ne voit pas les marqueurs habituels de
  l'automatisation (`navigator.webdriver` reste faux).
- **Lecture** : la passerelle lit la page déjà affichée, sans fabriquer de
  requêtes. Elle se repère aux attributs d'accessibilité et aux identifiants
  (`role=listitem`, `title-<id>`, `ul#cv-<id>`, `<time datetime>`), pas aux
  classes CSS générées, qui changent à chaque version de Snapchat Web.
- **Envoi** : le texte est tapé touche par touche dans la zone de saisie, à un
  rythme humain, puis validé avec Entrée.
- **USB** : la calculatrice se présente comme un périphérique USB à interface
  propriétaire (VID 0x0483, PID 0xA292), pilotée avec libusb, sans pilote à
  installer (descripteurs WinUSB pour Windows). Trames `TMIA` (magic, version,
  type, longueur, données, CRC-16) reprises de
  [Termia](https://github.com/Matinthereal/termia).
- **Rester connecté** : si l'appli rend la main à la boucle d'événements
  d'Epsilon, la calculatrice disparaît du bus USB quelques secondes plus tard.
  NumChat reste donc dans ses propres boucles tant que la session est ouverte :
  il lit le clavier lui-même (`Ion::Keyboard::popState`) et gère shift/alpha
  comme Epsilon. C'est ce qui permet de recevoir des messages pendant qu'on écrit.
  Un signal de vie part toutes les 4 secondes.
- **Accents** : les polices NumWorks dessinent une lettre puis un accent
  combinant. Le texte voyage donc en Unicode décomposé (NFD) ; la passerelle le
  recompose (NFC) avant de le taper dans Snapchat.
- **Emojis** : Chrome dessine chaque emoji dans un canvas en mémoire (jamais
  ajouté à la page), la passerelle le réduit en 14×14 et l'envoie à la
  calculatrice. Dans le texte, l'emoji devient un caractère à usage privé
  (U+E000 + n) qui renvoie à cette image (32 emplacements).
- **Snaps** : la passerelle ouvre le snap dans Snapchat Web, capture le média
  affiché (en mémoire seulement), le réduit pour tenir dans 320×240 et l'envoie
  ligne par ligne en RGB565. Chaque image est acquittée par la calculatrice
  avant l'envoi de la suivante.

### Protocole (trames 0x60 à 0x72)

| Type | Sens | Contenu |
| --- | --- | --- |
| `0x60` List | calc → PC | demande la liste |
| `0x61` Conversation | PC → calc | index, drapeaux (non lu), id, nom, extrait |
| `0x62` ListDone | PC → calc | nombre de conversations |
| `0x63` Open | calc → PC | id de la conversation |
| `0x64` Message | PC → calc | drapeaux (moi, avis, média, historique, visible), expéditeur, [n° de média], texte |
| `0x65` OpenDone | PC → calc | historique envoyé |
| `0x66` Send | calc → PC | texte tapé (NFD) |
| `0x67` SendResult | PC → calc | succès ou erreur |
| `0x68` Close | calc → PC | conversation quittée |
| `0x69` Status | PC → calc | message d'état |
| `0x6A` / `0x6B` | ↔ | Ping / Pong |
| `0x6C` Emoji | PC → calc | emplacement, 14×14 pixels RGB565 |
| `0x6D` View | calc → PC | n° du snap à ouvrir |
| `0x6E` MediaStart | PC → calc | largeur, hauteur |
| `0x6F` MediaRows | PC → calc | n° d'image, ligne, nombre de lignes, fin, pixels |
| `0x70` MediaEnd | PC → calc | encore visible ?, raison |
| `0x71` MediaAck | calc → PC | n° d'image affichée |
| `0x72` ViewStop | calc → PC | retour pendant le snap |

## Tests sans calculatrice

```bash
cd tests
../.venv/bin/python3 fake_numchat.py                       # messagerie de démo : liste, envoi, emojis, snaps
../.venv/bin/python3 fake_numchat.py --snapchat NOM        # vrai Snapchat, lecture seule
../.venv/bin/python3 fake_numchat.py --snapchat NOM "salut"   # envoie vraiment « salut » à NOM
../.venv/bin/python3 fake_numscreen.py                     # NumScreen : image de test, rien n'est cliqué
```

L'appli et la passerelle acceptent aussi `NUMCHAT=demo` (ou `./numsocial
--numchat demo`) : la calculatrice parle alors à une messagerie de démonstration
(une conversation qui répond en écho, une autre qui écrit toutes les 20 s et
envoie un snap photo et vidéo de test), sans toucher à Snapchat.

## Limites connues

- Snapchat Web change souvent. Si NumChat ne voit plus les conversations ou les
  messages, c'est que la page a changé : il faudra adapter `numchat_snap.py`.
- Seules les conversations affichées par Snapchat Web (celles qui tiennent dans
  la fenêtre) arrivent sur la calculatrice. Agrandis la fenêtre Chrome pour en
  avoir plus.
- Les 48 derniers messages d'une conversation sont gardés sur la calculatrice.
- Pas de vocaux (la NumWorks n'a pas de son), pas d'envoi de photos.
- La version Windows est compilée et vérifiée par GitHub, mais n'a pas encore
  été essayée avec une vraie calculatrice.

## Avertissements et licence

- Projet non officiel, sans lien avec NumWorks ni Snap Inc. NumWorks et Snapchat
  sont des marques déposées.
- L'automatisation de Snapchat Web est contraire aux conditions d'utilisation de
  Snapchat : tu l'utilises à tes risques, pour ton propre compte.
- Un firmware non officiel ne peut pas passer en mode examen.

Le code de ce dépôt est sous licence
[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.fr),
comme Termia dont il reprend le lien USB. Voir [LICENSE](LICENSE).
