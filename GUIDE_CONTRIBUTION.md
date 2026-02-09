# Guide de Contribution pour la Traduction de SpellForce: Conquest of Eo

Merci de votre intérêt pour améliorer la traduction française du jeu ! Ce document explique comment modifier les fichiers et proposer des corrections, en se basant sur les standards officiels de modding du jeu.

## 📂 Emplacement des Fichiers

Le fichier de traduction principal se trouve ici :
`...\Spellforce - Conquest of Eo\content\translation.FR\Localization\localization.FR.csv`

## 📝 Format Technique du Fichier

Le respect strict de ce format est **CRITIQUE** pour que le jeu ne plante pas.

- **Encodage** : Le fichier DOIT être en **UTF-8 SANS BOM** (Byte Order Mark).
- **Séparateur** : Point-virgule (`;`).
- **En-têtes** : La première ligne doit toujours commencer par `Id;Text`.

### Structure d'une ligne
```csv
Clé_Unique;Texte Traduit
```

## ⛔ Règles de Syntaxe (À LIRE ABSOLUMENT)

1. **PAS de Point-virgule (`;`) dans le texte** : Le jeu l'utilise pour séparer la clé du texte. Si vous mettez un `;` dans votre traduction, cela cassera la ligne.
   - *Mauvais* : `Un objet puissant; il brille.`
   - *Bon* : `Un objet puissant, il brille.` ou `Un objet puissant : il brille.`
2. **Sauts de ligne** : Utilisez `\n` pour faire un retour à la ligne. N'appuyez pas sur "Entrée".
3. **Ne touchez PAS aux Clés** : La partie gauche avant le `;` ne doit jamais changer.

## 🤖 Macros et Variables (Ne pas traduire !)

Le jeu utilise des codes entre accolades `{}` ou crochets `[]` pour insérer des valeurs dynamiques. **Vous ne devez jamais les traduire ou les modifier.**

### Macros de Valeurs
Ces macros formatent les nombres automatiquement :
- `{value:p}` : Affiche un pourcentage (ex: 0.12 devient `12%`).
- `{value:+-}` : Ajoute un signe + ou - (ex: 3 devient `+3`).
- `{value:c}` : Ajoute une couleur bonus/malus (ex: 3 devient `[bonus]3[/]`).
- `{value:r}` : Pour les ressources, affiche `∞` si négatif.

### Macros de Texte
Ces macros gèrent les espaces autour des noms :
- `{name:+fs}` : Ajoute une espace *avant* le nom s'il n'est pas vide (fs = front space).
  - *Exemple* : `Bonjour{name:+fs}` donne "Bonjour Paul" (si nom=Paul) ou "Bonjour" (si vide).
- `{name:+bs}` : Ajoute une espace *après* le nom s'il n'est pas vide (bs = back space).

**Règle d'or** : Copiez-collez exactement les parties comme `{0}`, `{value:c}`, `[bonus]`, `[/]`.

## 🎨 Texte Enrichi (Rich Text)

Le jeu utilise **TextMeshPro**. Vous pouvez utiliser certaines balises pour le style, mais avec parcimonie :
- `<b>Texte</b>` : Gras
- `<i>Texte</i>` : Italique
- `[c]Texte[/]` : Couleurs spécifiques au jeu (ex: `[bonux]`, `[malus]`, `[iarmor]`)

## 🛠️ Outils Recommandés

- **Visual Studio Code** (Recommandé) :
  - Installez l'extension "Rainbow CSV" pour lire facilement le fichier.
  - Vérifiez en bas à droite que l'encodage est "UTF-8".
- **Notepad++** :
  - Menu `Encodage` -> `Convertir en UTF-8 (sans BOM)`.

## 🧪 Tester vos Modifications

1. Ouvrez `localization.FR.csv`.
2. Faites vos modifications.
3. Sauvegardez.
4. Si le jeu est lancé, il faut souvent le redémarrer pour que les changements de textes soient pris en compte (ou recharger le mod si le menu le permet).
