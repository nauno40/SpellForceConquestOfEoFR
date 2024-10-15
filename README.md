
# Système de Localisation

Le système de localisation est basé sur le concept de mapper un identifiant (id) à un texte et d'avoir différentes bases de données pour chaque langue.

## Code de langue

Les codes de langue actuellement pris en charge sont :

- **Codes à deux lettres**
  - EN, DE, RU
- **Codes à quatre lettres avec un tiret**
  - zh-CN, zh-TW

Veuillez utiliser le code de langue standard (ISO 639) pour éviter les confusions et augmenter la compatibilité.

Les codes à deux lettres doivent être préférés aux codes à quatre lettres.
Exemple : DE au lieu de de-DE

## Base de données d'identifiants de localisation

La base de données des identifiants pour une langue spécifique est créée en :

- Créant un fichier texte, se terminant par .{LANGUAGECODE}.csv dans le dossier Localization de votre répertoire Mod.
  - Exemple : MyMod.EN.csv
- Chaque ligne dans le fichier .csv est une entrée constituée de l'ID, d'un point-virgule et du texte réel qui sera affiché dans le jeu.
  - Exemple : #Dwarf_MoleRider.NameId;Moleriders
- Si un ID existe plus d'une fois, le texte affiché sera remplacé par le dernier selon l'ordre des packages.
- En tant que solution de repli, si un package a une localisation en anglais mais pas de localisation pour la langue actuellement sélectionnée, la localisation anglaise sera affichée dans le jeu.

## Exemple de localisation

Fichier de base du jeu : Localization/com.ownedbygravity.sf.base.EN.csv
```
#Dwarf_MoleRider.NameId;Moleriders
```

Fichier de mod : Localization/myloca_1.EN.csv
```
NewItem.Name;The awesome item
NewItem.Description;The description for an awesome item
#Dwarf_MoleRider.NameId;Moldy Riders
```

Fichier de mod : Localization/myloca_1.DE.csv
```
NewItem.Name;Der coole Gegenstand
NewItem.Description;Die Beschreibung des coolen Gegenstand
```

Si la langue est l'anglais, le texte affiché sera :
```
#Dwarf_MoleRider.NameId -> Moldy Riders
NewItem.Name -> The awesome item
NewItem.Description -> The description for an awesome item
```

- #Dwarf_MoleRider.NameId est pris du dernier fichier chargé. Comme myloca_1.EN.csv est chargé après la localisation du jeu de base, celui-ci est pris.
- NewItem.Name et NewItem.Description sont pris de myloca_1.EN.csv car ils ne sont pas présents dans la localisation de base et que .EN.csv correspond au code de langue actuel.
- myloca_1.DE.csv est complètement ignoré car il ne correspond pas au code de langue EN pour l'anglais.

## Règles de fichier texte de localisation

- Utiliser Excel pour ouvrir directement ce fichier .csv peut être tentant, mais cela détruira tout, car les paramètres par défaut d'Excel ne correspondent pas à notre format de fichier !
- Tout ce qui se trouve entre [ ] et { } sont des macros qui NE DOIVENT PAS être traduites.
- La première ligne DOIT toujours commencer par Id;Texte.
- Le fichier NE DOIT PAS contenir de marque d'ordre d'octets (BOM).
- Le fichier DOIT être un fichier texte UTF-8.
- Tapez 
 pour ajouter un saut de ligne.
- Les points-virgules NE DOIVENT PAS être utilisés dans aucun texte.
- Les colonnes doivent être séparées par des points-virgules ( ; ).
- Il doit y avoir au moins deux colonnes dans chaque ligne.

## Macros de texte de localisation

### Substitutions avec { }

Les { } sont des valeurs substituables qui sont remplacées par le jeu. Selon l'ID, différentes macros sont disponibles.

L'utilisation de base est un identifiant textuel pour déterminer quelle "variable" insérer ici.
Exemple : {who} ou {value}

Vous pouvez repositionner les entrées { } n'importe où dans votre texte sans avoir à respecter l'ordre dans lequel elles apparaissent, si votre langue nécessite un ordre différent.

Certaines variables peuvent avoir des sous-entrées (par exemple : des infobulles d'articles) auxquelles vous pouvez accéder individuellement en utilisant la syntaxe {a.b} ou {a.b.c} et en connaissant leurs noms.

Les variables peuvent avoir un formatage supplémentaire en utilisant la syntaxe {name:format}. Plusieurs des éléments suivants peuvent être combinés :

- **p**
  - {value:p} si la valeur est un nombre, le nombre sera multiplié par 100 et un signe % sera ajouté.
  - Exemple : la valeur est 0,12 {value:p} affichera "12%".
  
- **+−**
  - {value:+-} Si la valeur est un nombre, un signe plus sera ajouté avant un nombre positif.
  - Exemple : la valeur est 3, {value:+-} affichera "+3".
  
- **c**
  - {value:c} Si la valeur est un nombre, les macros de couleur [bonus] ou [malus] seront ajoutées en fonction du signe du nombre.
  - Exemple : la valeur est 3, {value:c} affichera "[bonus]3[/]". Si la valeur est -2, {value:c} affichera "[malus]-2[/]".

- **i**
  - Idem que {value:c} mais avec des couleurs inversées. Donc les nombres positifs auront [malus] et les nombres négatifs auront [bonus].
  
- **r**
  - {value:r} si la valeur est un nombre négatif, affichez ∞ à la place.
  - Exemple : la valeur est -1, cela affichera "∞". La valeur est 2, cela affichera "2".
  
- **n**
  - {value:n} Si la valeur est négative, ajoutez une coloration [malus].
  - Exemple : la valeur est 1, cela affichera "1", la valeur est -2, cela affichera "[malus]-2[/]".

- **+fs**
  - {name:+fs} ajoutera un espace avant quel que soit le nom si le nom ne résout pas à aucun texte.
  - Exemple : "Bonjour{name:+fs}, comment ça va ?" Si le nom est dave, le texte contiendra "Bonjour <espace> dave, comment ça va ?". Si le nom est <vide>, le texte contiendra "Bonjour, comment ça va ?".
  
- **+bs**
  - {name:+bs} ajoutera un espace après quel que soit le nom si le nom ne résout pas à aucun texte.
  - Exemple : "Bonjour {name:+bs}, comment ça va ?" Si le nom est dave, le texte contiendra "Bonjour dave <espace>, comment ça va ?". Si le nom est <vide>, le texte contiendra "Bonjour , comment ça va ?".

Vous pourrez peut-être identifier les substitutions disponibles, par exemple les infobulles d'articles, en regardant le contenu des fichiers .odl.

### Macros []

Les macros [] existent sous deux formes différentes :

- **Macros simples** → [iarmor]
- **Macros de regroupement** → [bonus]+3 dégâts[/]

Il existe un certain nombre de macros prédéfinies. Actuellement, il n'y a aucun moyen d'en ajouter plus via le modding.
