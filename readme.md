## Sujet de projet standard (Plutus)
L'application Android Plutus qu'il vous est proposé de réaliser nous permettra de gérer nos finances personnelles.
Les fonctionnalités demandées sont associées à un niveau <N> : il est conseillé d'implanter l'application par étapes en commençant par s'intéresser aux fonctionnalités de niveau <0>, puis <1>, puis <2> et enfin <3>.

## Modèle de données
Carnet de comptes
L'application doit gérer un ou plusieurs carnets de comptes. Le carnet est une liste de transactions ainsi qu'un ensemble de contraintes de budget.

## Transaction
Une transaction est une opération financière qui peut être un revenu ou une dépense. Elle comprend les informations suivantes :

Un texte libre <0>. On pourra éventuellement proposer de formater le texte en utilisant des balises Markdown <1>.
Une date correspondant à l'instant où la transaction a eu lieu <0>.
Des étiquettes <0>. Il s'agit de mots-clés qui pourront nous permettre de classer et chercher plus rapidement la transaction.
Un montant <0> avec une devise <3>.
Des éléments multimédias <2>. Il s'agit d'images, de vidéos ou sons dont l'utilisateur peut lancer l'acquisition depuis l'application ou alors pouvant être sélectionnés parmi les éléments déjà enregistrés sur l'appareil.
Une géolocalisation <2>. Cela permet d'indiquer une position géographique liée à la transaction (e.g. localisation du magasin où a été réalisé une dépense)
Il doit être possible de créer une nouvelle transaction de zéro ou de copier une transaction existante que l'on modifie <0>.

## Etiquette
Une étiquette permet de catégoriser une transaction. L'utilisateur peut attribuer des mots-clés libres comme étiquettes <0> ; toutefois les étiquettes peuvent avoir un format spécifique pour leur conférer une sémantique particulière :

Une étiquette commençant par - (e.g. -food, -car) indique une catégorie de dépenses
Une étiquette commençant par + (e.g. +wage, +interest) indique une catégorie de revenus
Une étiquette commençant par = (e.g. =standard) indique le compte sur lequel a lieu la transaction ; il est possible aussi de spécifier deux comptes à la fois (e.g. =standard-savings) pour marquer une transaction qui est un virement entre deux comptes
L'étiquette @todo <2> sert à spécifier une transaction encore non réalisée (avec une date dans le futur)
On pourra proposer un jeu d'étiquettes prédéfini <2> afin de faciliter l'autocomplétion lorsque l'on place des étiquettes sur une transaction. L'usage antérieur d'une étiquette permet également l'autocomplétion <2>.

## Budget
Il est possible de définir un budget pour une étiquette donnée <1>. Un budget est un objectif de dépenses ou de revenus pour une certaine période (semaine, mois, année).

Par exemple si l'on définit une valeur de budget de 1000 euros sur 1 an pour -car, nous nous fixons pour objectif de ne pas dépasser 1000 euros de dépenses automobiles annuelles.

## Devise
Pour chaque transaction, nous associons un montant avec une devise. Pour simplifier, on peut considérer une unique devise pour l'implantation de l'application. On pourra ensuite supporter plusieurs devises <3> avec conversion automatique en récupérant des cours depuis une API web.

## Stockage et sauvegarde du carnet de comptes
Le carnet de comptes sera stocké sous la forme d'une base de données SQLite <0> qui pourra être exploitée directement ou en passant par un ORM (tel que Room). Les données multimédias ne seront pas stockées en base mais sous la forme de fichiers <2>.

Il doit être possible d'exporter tout ou partie (en filtrant par des étiquettes) du carnet de comptes sous la forme d'un fichier unique <1> (dont on réfléchira au format). Un tel fichier doit également être importable dans le carnet courant avec un mécanisme de fusion évitant tout doublon <2>. Le fichier exporté doit être chiffrable par un mot de passe secret <2> (l'usage d'une bibliothèque cryptographique est autorisée).

Vous pourrez optionnellement proposer un service de sauvegarde en ligne automatique du carnet de compte <3>. Un tel service pourra héberger le fichier exporté du carnet ; les données devront être chiffrées et uniquement déchiffrables par l'utilisateur (le service en ligne doit être de type zero knowledge : il ne doit posséder aucun moyen de déchiffrer les données).

## Interface graphique
Création et choix de carnets de compte
Plusieurs carnets de comptes peuvent être hébergés par l'application, il doit être possible de choisir le carnet courant ainsi que de créer, dupliquer ou supprimer un carnet existant <0>. L'export d'un carnet sous la forme d'un fichier <1>, l'import d'un carnet <1> (avec possible fusion des transactions <2>) est également réalisable.

## Création, visualisation et édition de transaction
Il doit être nécessaire de pouvoir écrire du texte libre concernant la transaction <0> (formatable en Markdown <1>), ajouter/modifier une géolocalisation <2>, le montant <0>, des images, vidéos et sons <2> ainsi que des étiquettes <0>. L'interface graphique facilite la saisie par exemple par autocomplétion des étiquettes <2>.

## Recherche de transactions et rapports
Les transactions doivent être recherchables par critères que sont l'étiquette <0>, une fourchette de date et de montant <1>, ainsi que la géolocalisation <2> ou la présence de mot-clé dans le texte de la transaction <2>. Pour nous aider, on peut visualiser une liste de toutes les étiquettes utilisées dans le carnet <0> (pour lancer une recherche sur celles-ci).

Les résultats obtenus doivent être triables par montant et/ou date <1> et également visualisables sur une carte pour ceux géolocalisables <3>.

La recherche doit être rapide par l'exploitation d'index notamment sur les étiquettes <1>, les dates et montants <2>.

Il est possible d'enregistrer des critères types (e.g. transactions de dépenses alimentaires avec l'étiquette -food réalisées en 2022 à moins de 2 km de Champs-sur-Marne portant sur un montant inférieur à 20 euros) afin de relancer rapidement des recherches <0>.

La recherche doit mener à l'obtention de la liste des transactions mais également à des rapports synthétiques concernant ces transactions :

Somme des montants des transactions sélectionnées par la recherche (ce qui permet d'obtenir le solde pour un compte) <0>
Diagramme des montants cumulés des transactions selon le temps avec comparaison possible avec le budget <1>
Répartition des transactions selon leur étiquette <1>
...
La recherche doit permettre aussi d'ajouter ou supprimer en masse des étiquettes sur les transactions du résultat (voire de supprimer les transactions) <1>.

## Notifications de dépassement de budget
Elles doivent être activables afin de pouvoir être prévenu lorsque nous dépassons un objectif de budget pour une catégorie de dépenses <2>. La notification apparaît dans le tiroir de notifications de l'appareil jusqu'à être annulée par l'utilisateur.

## Rappel de transactions
Lorsqu'une transaction a été enregistrée avec une géolocalisation, il est possible d'activer sur cet emplacement un rappel <2>. Cela entraînera une notification lorsque l'utilisateur se rendra de nouveau sur ce lieu (par exemple un magasin) : cette notification l'invitera à saisir une nouvelle transaction.

Les transactions étiquettées par @todo doivent également faire l'objet d'un rappel à proximité de la date de transaction <2>.

## Conditions de réalisation
Pour la réalisation du projet, il est demandé d'utiliser l'API Jetpack Compose avec le langage Kotlin. L'usage de bibliothèques externes supplémentaires est autorisé pour la sérialisation/désérialisation de données, la réalisation de diagrammes pour les rapports ainsi que le stockage de données (ORM).
