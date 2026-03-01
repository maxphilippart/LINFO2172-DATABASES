# Notes - Fonctions SQL

## Sélection et Filtrage

Afin de selectionner des éléments présent dans une database on utilise le querry SELECT suivi de l'element du tableau que l'on souhaite selectionner.

On utilise un semi-colon pour différencier les querries.

### Exemple basique :
```sql
SELECT * FROM database;
```

Ce query récupère **tous les colonnes** (avec `*`) de la table `database`.

### DISTINCT

Plusieurs noms peuvent apparaître plusieurs fois dans une database, le mot clé DISTINCT sers à selectionner chaque élément 1 fois.

### Exemple basique : 
```sql
SELECT DISTINCT country FROM database;
```

### Count : 

C'est une fonction qui peut être utilisé pour compter le nombre d'éléments d'une colone. Elle peut être utilisé avec DISTINCT pour ne pas compter les doublons.

### Exemple basique : 
```sql
SELECT COUNT(DISTINCT country) FROM database;
```

## Where

Utilisé pour ne prendre que les éléments qui remplissent certaines condition cité par WHERE.

### Exemple basique : 
```sql
SELECT * FROM Customers
WHERE Country='Mexico';
```
Ne selectionnera que les éléments de CUSTOMERS dont la country sera égal à Mexico.

### Operator : 

WHERE utilise de nombreux operator différents.
Les plus classique comme: > , < , = , <= , >=

MAIS

On trouve des spécifiques à SQL, comme <> qui est égal à NOT.

Egalement nous avons BETWEEN qui est utilisé pour trouver des éléments entre deux éléments

### Exemple basique : 

```sql
SELECT * FROM Products
WHERE Price BETWEEN 50 AND 60;
```

Nous pouvons également mettre NOT au début pour donc faire l'inverse et c'est utilisable avec la fonction IN suivante.

### IN

Enfin nous aurons IN qui permet de faire plusieurs conditions pour retourner plusieurs valeurs possibles au lieu de 1 avec le =.

### Exemple basique : 

```sql
SELECT * FROM Customers
WHERE City IN ('Paris','London');
```

Cela permet de prendre tous les customers avec les villes Paris ET London. C'est en gros une simplification de plusieurs OR.

En ajoutons NOT devant nous aurons NOT IN qui ferait en sorte de retourné toute valeurs qui ne serait une des valeurs mentionnée.

### Exemple basique : 

```sql
SELECT * FROM Customers
WHERE City NOT IN ('Paris','London');
```

IN peut également être utilisé pour nester des querryrequest de cette manière ci.

### Exemple basique : 

```sql
SELECT * FROM Customers
WHERE CustomerID IN (SELECT CustomerID FROM Orders);
```

Ici on selectionnera toutes les lignes de Customers pour lequels la CustomerID est présente dans le tableau Orders. Donc d'abord il prends tous les CustomersID de Orders et si celles de Customers correspond il renvoit.

## ORDER BY

Permet de rendre les resultats en étant ordonné d'une certaine manière. Il a plusieurs moyen d'attendre différents tris.

### DESC

Permet de rendre les résulats de manière descendant au lieu de ascendant par défault.

De manière général SQL se charge de lui même de rendre en ordre alphabétique pour des string et numéraire pour des nombres.

Nous pouvons ordonner plusieurs colonne, celles-ci s'ordonnerons d'abord par la première choisi et ensuite lorsque il y a plusieurs doublons, la deuxième colonne triera les doublons à son tour.

### Exemple basique : 

```sql
SELECT * FROM Customers
ORDER BY Country, CustomerName;
```
Ici, d'abord l'ordre de rangement sera en fonction des country, mais si il y a plusieurs country celles-ci seront triés en fonction de CustomerName. 

On peut également utiliser DESC et ASCEµ pour trié plusieurs colones en simultané.

### Exemple basique : 

```sql
SELECT * FROM Customers
ORDER BY Country ASC, CustomerName DESC;
```

## Opérations Logiques

SQL supporte les operation logique AND, OR et NOT pour les formules de WHERE.
Elles peuvent être utilisé comme suit.

### OR :


```sql
SELECT * FROM Customers
WHERE City = 'Berlin' OR CustomerName LIKE 'G%' OR Country = 'Norway';
```

### AND :


```sql
SELECT * FROM Customers
WHERE Country = 'Brazil'
AND City = 'Rio de Janeiro'
AND CustomerID > 50;
```

### NOT :


```sql
SELECT * FROM Customers
WHERE NOT Country = 'Spain';
```

Ils peuvent également être combinés.

### Exemple de combinaisons : 

```sql
SELECT * FROM Customers
WHERE Country = 'Spain' AND CustomerName LIKE 'G%' OR CustomerName LIKE 'R%';
```

## Insertion

On utilise le mot clé INSERT INTO pour insérer de nouvelles lignes dans le tableau.

### Exemple : 

INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);

On insère les éléments a même index que la colone (col1 pour value1 ect..).
Si on insère dans toutes les colonnes (on a un élément pour chaque colonne) on peut juste npter les valeurs.

comme ceci : 

INSERT INTO table_name
VALUES (value1, value2, value3, ...);

## NULL value

Certaines colonnes sont optionnels, lorsque l'on insère queluqe chose nous ne sommes pas obligé d'y insérer une valuer ce qui créé une NULL VALUE (qui ne s'affichera pas lorsque nous devons afficher des résultats).

On peut vérifier si une valeur est NULL ou  non NULL avec les commande IS NULL ou IS NOT NULL. Cela peut servir à ne prendre que les ligne ou certains points sont NULL ou bien inversément que les lignes où les points sont NON NULL.

### Exemple :

```sql
SELECT CustomerName, ContactName, Address
FROM Customers
WHERE Address IS NULL;
```

## UPDATE

Pour modifier une ligne existante nous faisons la commande UPDATE comme suit.

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

ATTENTION: Il est trés important de bien mettre le WHERE sans quoi l'entièreté du tableau sera mise à jour !!!

### Exemple basique : 

```sql
UPDATE Customers
SET ContactName='Juan'
WHERE Country='Mexico';
```

## DELETE

Tout comme update, ici le WHERE est trés important pour éviter de delete l'entièreté des éléments du tableau.
Il est utilisé pour supprimer certaines lignes.

### Exemple basique :

```sql
DELETE FROM table_name WHERE condition;
```
Pour supprimer tous les éléments du tableau sans supprimer le tableau on fait 

```sql
DELETE FROM Customers;
```

Si on désire supprimer le tableau

```sql
DROP TABLE Customers;
```

## SELECT TOP et LIMIT
Ceci permet de selectionner les x premiers éléments (qui peuvenet être accompagné d'une condtion WHERE). 

LE PROBLEME: Tous les RDBMS ne supporte pas SELECT TOP, certaines utilisent d'autrres moyen comme MySQL qui utilise LIMIT ou ORACLE qui utilise FETCH. 
Je vais vous donner les différentes manière en dessous.

### Exemple classique de SQL/MS server : 
Ici qui foncionne donc avec SELECT TOP.

```sql
SELECT TOP number|percent column_name(s)
FROM table_name
WHERE condition;
```

### Exemple classqiue avec MySQL : 

MySQL utilise donc ici LIMIT.

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
LIMIT number;
```

### Exemple classique avec Oracle : 

```sql
SELECT column_name(s)
FROM table_name
ORDER BY column_name(s)
FETCH FIRST number ROWS ONLY;
```

### PERCENT

Percent est un mot clé utilise avec SELECT TOP afin de choisir un pourcentage plutôt qu'un nombre. Il s'agit d'un mot clé utilisable dans les différents RDBMS

### Exemple classique avec MySQL: 

```sql
SELECT TOP 50 PERCENT * FROM Customers
WHERE country='BELGIUM';
```

## AGGREGAT

Des aggregats sont utilisé pour prendre des sets de valeurs et en retourné une seule. C'est souvent utilisé avec le GROUP BY du statemnt SELECT.

### MIN

Plus petit élément.

```sql
SELECT MIN(Price)
FROM Products;
```

### MAX

Plus grand élément.

```sql
SELECT MAX(Price)
FROM Products;
```

### COUNT

Nombre d'éléments.

```sql
SELECT COUNT(*)
FROM Products;
```

### SUM

Somme des éléments.

```sql
SELECT SUM(Quantity)
FROM OrderDetails;
```

### AVG

Moyenne.

```sql
SELECT AVG(Price)
FROM Products;
```
## Alias (AS)

Lorsque le résultats des aggregats est rendu la colonne ne porte pas de nom, AS permet de lui en attribué un. 
MAIS pas que.
C'est utilisable pour tous les resulats si nous voulons changer le nom des colonnes résultantes.


### Exemple basique : 

```sql
SELECT AVG(Price) AS [average price]
FROM Products;
```

Spécificité ici, nous utlisons des [] ce qui n'est pas obligatoire et qui ne sert uniquement si nous voulons conservés des espaces. Nous pourrions utilisés des guillemets également.

### Concatenation de résultats :

AS nous permet également de concat les résultats afin de rassembler plusieurs colones dans une (pour des adresses par exemples).

### Exemple basique : 

```sql
SELECT CustomerName, Address + ', ' + PostalCode + ' ' + City + ', ' + Country AS Address
FROM Customers;
```

Cela rassemblera les colones adresse, postal code, city et country dans une seule colonnes nommée Adress. Nous pouvons également rajouter des éléments à celles ci comme les virgules ou espaces.

## LIKE et Wildcards

La condition LIKE qui permet de rechercher un pattern et est utilisé dans WHERE s'utilise avec des Wildscards qui sont à savoir % et _ .

Le but des wildcards est de remplacé les bout de phrases que l'on a pas.
Pour être plus clair par exemple: "L_nd_n" _ sert à remplacé un charactère (chiffre ou lettre) quel qu'il soit et donc la recherche pourra donner London ou Landen.

Le wildcard % sert à remplacer des pans entier de mots que l'on aurait pas.

Par exemple: Si on cherche un caractère qui commence par "pr" on fera "pr%" car on n'a pas ce qui suit pr.
Si on veut quelque chose qui termine par "pr" nous ferons "%pr".
Si nous voulons un pattern qui se situe quelque part dans le mot nous ferons "%pr%" indiquant que nous n'avons ni le début ni la fin.

Les Wildcards peuvent être combinés également ce qui peut donner ceci par exemple: "pr__%". Ce qui veut dire que cela doit commencer par pr et avoir au moins une longeur de 4 caractères. (pre ne serait pas accepter par exemple).

### Exemple basique : 

```sql
SELECT * FROM Customers
WHERE City LIKE 'str%';
```

Ceci cherchera et retournera les City commencant par 'str'.

### Autres Wildcards :

NOTE: ceux-ci ne sont pas supportés dans MySQL et PostgreSQL.

Il exsite d'autres wildcards comme [] qui permet de chercher tous les caractères dans la branche (pas le mot contenu). Cela donnerai ceci: "[asp]%" qui chercherait tout mot commencant par "a","s" ou "p".

### -

Si nous y ajoutons un tiret (-) cela donnerai une range de caractère.

Exemple: "[a-f]%" chercherait tous caractère se trouvant entre a et f compris.

### ^

Nous trouvons également ^ qui permet dans une branche de faire un NOT. Donc ceci "[^sql]%" reviendrait à chercher tous caractères qui n'est pas S,Q ou L.

## Join


