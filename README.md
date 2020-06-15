# Electro-Encephalogramme


>Créateur=Yoann Ochietti, Yann Aublet
Description projet=Outil de captation des ondes cérébrales
Ingrédient=Plaque de cuivre, Résistances, Condensateurs, Potentiomètre, AD620AN, TL084CN, électrodes, gel conducteur, batterie 9 volts, fil audio 25mm
Outil utilisé=Fraiseuse CNC, Outils d'électronique
Logiciel utilisé=Fritzing, Aspire, Mach3, InkScape, Adobe Illustrator
Status=En développement
License=GPL
Image principale projet=pcb_eeg.jpg
Image projet=Schema_eeg.jpg, pcb_eeg.jpg, pcb_eeg_2.jpg, Mapping_pour_electrodes.png
Taille image=Moyenne
Cadre=Inspiré par l'emergence d'EEG (électro-encéphalogramme) destinés au grand publique (Emotiv [[http://emotiv.com/]]), ainsi que par des initiatives open source/hardware (openEEG [[http://openeeg.sourceforge.net/doc/]]), j'ai décidé de construire ma propre instance d'un EEG conçu par un individu au pseudonyme de cah6 et dont les plans sont disponibles sur le site web Instructables à l'url suivant: [[http://www.instructables.com/id/DIY-EEG-and-ECG-Circuit/]].
Usage=L'objectif est d'appliquer le EEG, une fois construit, à des fins d'entrainement mental (par la méthode de "feedback" neuronal) ainsi que comme contrôleur pour des applications faites/modifiées sur mesure.

Il est important de noter que, dans son design actuel, l'EEG se veut un capteur d'ondes Alpha et Beta (entre ~8 et ~30 Hz), et laisse donc de côté les ondes de plus basses (Theta, Delta) et plus hautes (Gamma) fréquences qui sont pertinentes à l'analyse des phase du sommeil. L'EEG est donc principalement conçu pour discriminer entre un état de relaxation (ondes alpha de 8 à 12 Hz) et de concentration (ondes bêta de 12 à 30 Hz).
|Description_projet=Électroencéphalogrammme
}}
## Fonctionnement
Le circuit en tant que tel est essentiellement une série de filtre et d'amplificateurs qui servent à enlever le plus de bruit possible du signal analogue obtenu par des électrodes avant de le digitaliser.

Une fois le signal amplifié une première fois, on le fait passer au travers d'un filtre coupe bande à 60Hz, un filtre passe-haut à 7Hz, un passe bas a 31 Hz, un passe haut a 1 Hz, un autre amplificateur et, finalement, un autre coupe bande a 60 Hz pour retirer le bruit additionnel qui serait survenu depuis le premier filtre. Après le traitement, le signal est envoyé dans l'entrée audio d'un ordinateur et est ainsi digitalisé via une carte audio. Alternativement, le signal peut être visualisé à l'aide d'un oscilloscope.

3 électrodes sont utilisées pour interfacer le circuit avec le sujet: 2 pour mesurer une différence de potentiel entre deux points et une troisième pour établir la mise à terre. Le positionnement des électrodes est un facteur important dans la nature du signal détecté. cah6 suggère la mesure des ondes dans le lobe occipital (responsable pour la vision) à l'arrière de la tête en raison de leur forte amplitude et la facilité de les moduler consciemment. À cette fin, le positionnement est donné par rapport à certains os du crâne et au schéma ci-dessous:
![Mapping_pour_electrodes](https://user-images.githubusercontent.com/65183668/84677686-5efa3100-af2f-11ea-81c2-34a478a4de25.png)

Le concepteur original du EEG a aussi construit un programme qui sert à lire l'information dans la carte audio d'un ordinateur pour exposer l'amplitude du signal par bande de fréquence, permettant ainsi la comparaison des "quantités" d'ondes alpha et bêta. Toutefois, le code fournit sur Github par cah6 présente plusieurs problèmes et semble incomplet. Une des étapes du projet consisterait donc à soit réparer ou réécrire le code du program qui analyse le signal. 

## Circuit
![Schema_eeg](https://user-images.githubusercontent.com/65183668/84677621-438f2600-af2f-11ea-8b7c-e28e8b842ec7.jpg)


Pour commencer, j'ai monté le circuit ci-dessus, disponible en plus haute résolution sur la page Instructable, sur une platine d'experimentation ("breadboard"). Une fois monté, j'ai vérifié que le circuit était au moins rudimentairement fonctionnel branchant son output dans un oscilloscope alors que les électrodes était placées sur mon avant-bras (une en dessous, une au dessus) et celle pour la mise à terre sur mon genou (que, j'estime, avais une activité électrique assez basse). Il aurait été préférable de mesurer les voltages au niveau de la tête, comme c'est l'objectif du circuit, mais j'étais réticent à l'idée de connecter à ma tête un circuit connecté au 120 Volts (via l'oscilloscope). En temps normal, l'EEG serait branché dans un ordinateur portable qui serait débranché du 120 Volts  et il n'y aurait pas ce problème. L'oscilloscope m'a ainsi permis de vérifier l'efficacité du circuit dans l'absence d'un programme pour lire les données.

Mes test étant terminés avec succès (quand je forçais mon avant-bras, l'oscilloscope affichait une élévation substantielle dans l'amplitude du courant), j'ai décidé, encouragé par mes collègues, de reproduire le circuit sur dans un circuit imprimé, plutôt que de d'utiliser une simple plaque de cuivre trouée. Pour ce faire, j'ai choisi d'utiliser une CNC (défonceuse) pour graver le circuit dans une plaquette de cuivre. Il me faudrait donc un fichier de Gcode (le language utilisé par la CNC), qui à son tour requiert plusieurs étapes préalables. D'abord, il fallait numériser le circuit affiché plus haut, ce qui à été plutôt simple grace à Fritzing, un logiciel gratuit conçu à cette fin. 

Une fois le schéma dans le programme, une disposition de circuit imprimé est automatiquement généré par Fritzing. Il faut toutefois quand même retravailler la disposition des composantes sur la plaque pour que les traces automatiquement générées soit un minimum organisées. J'ai donc placé les composantes de manières à reproduire le plus possible la topographie du schéma en mettant côte à côte les pièces qui le sont dans le schéma.

La forme du circuit imprimé confirmée, il me fallait exporter mon fichier format dans un format qui pourrait ensuite être transformé en Gcode (Fritzting n'offrant pas cette option). Après plusieurs expérimentations avec les différents formats d'export, PDF, étant un fichier vectoriel plutôt universel, semblait se prêter le mieux aux opérations à venir. 

Même si PDF est un format vectoriel, les vecteurs du fichier généré par Fritzing requierait une quantité considérable de formatage s'ils allaient êtres transformés en "chemins" (paths) de CNC. J'ai donc ouvert le fichier dans Adobe Illustrator pour traiter les vecteurs en question. Il a d'abord fallu transform les chemins qui formaient les traces en chemins de //contour//. De plus, j'ai du "joindre" (join) les vecteurs pour en faire des chemins unis et éviter des coupes qui auraient coupés les connections entre les "pads" et les traces. (Les images ci-dessous illustrent ces opérations.???)

Une fois les vecteurs bien formatés, il suffit d'ouvrir le pdf dans Aspire, un logiciel qui génère le Gcode à partir de fichiers vectoriels. Il suffit d'y inscrire les dimensions du matériel (la plaque de cuivre à découper) et les paramètres de découpe. 2 chemins, chacun avec leurs propres paramètres ont étés suffisant pour faire toutes les gravures nécessaires: un chemin pour le contour des traces et un autre pour percer les trous (si la plaque est à deux côtés, l'autre côté ne nécessite que les chemins de contour). Pour ce qui est de la bite utilisée, une "end mill" de 0.8 mm de diamètre à été appropriée pour moi, mais une "v-bite" permet des traits encore plus fin si cela est nécessaire. Il faut s'assurer que le trait est assez fin pour séparer tous les "pads" qui doivent l'être, tout en conservant assez de surface sur les pads pour pouvoir percer un trou de 0.8mm de diamètre. De plus, si le PCB est à deux côtés, il faut faire attention à conserver le zéro en retournant la plaque de cuivre pour graver l'autre côté,faute de quoi les trous et les "pads" risque de ne pas êtres alignés d'un côté à l'autre. Après quelques tests (image en bas a gauche), j'ai finalement réussi à avoir un résultat satisfaisant (image en bas à droite).

![Pcb_eeg](https://user-images.githubusercontent.com/65183668/84677571-307c5600-af2f-11ea-9204-ff258ba2ae2f.jpg)
![Pcb_eeg_2](https://user-images.githubusercontent.com/65183668/84677574-32461980-af2f-11ea-8edd-50b86f8d6acc.jpg)

## Logiciel
Le logiciel utilisé, pour exposer les amplitudes par fréquence, opère une transformation du fourrier qui consiste à prendre l'onde reçue et à la décomposer en ondes sinusoïdales pures dont l'amplitude respective est telle que lorsque "additionnées", les ondes sinusoïdales résultent en l'onde reçue.

Cette partie du projet reste à développer.

## Planification
- Monter le circuit sur une plaque de prototypage (breadboard)
--Tester circuit avec un oscilloscope
- Transformer en circuit imprimé
--Numériser le circuit avec Fritzing
--Transformer en Gcode pour graver avec une CNC
***Exporter de Fritzing en PDF
***Importer dans Adobe Illustrator pour paufiner la vectorisation
***Importer le résultat dans Aspire qui génère le Gcode a partir du pdf vectorisé
--Tester avec un oscilloscope
- Retaper software ou trouver un substitut
