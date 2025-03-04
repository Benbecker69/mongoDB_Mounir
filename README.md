# Ilias BENHARRAT | TP MongoDB - Gestion d'une Bibliothèque

## Partie 2 : Requêtes de lecture (Read)

### 1️. Liste des livres disponibles
```javascript
db.livres.find({ disponible: true });
```

### 2️. Livres publiés après l'an 2000
```javascript
db.livres.find({ annee_publication: { $gt: 2000 } });
```

### 3️. Trouver les livres d'un auteur spécifique
```javascript
db.livres.find({ auteur: "Élise Marquant" });
```

### 4️. Trouver les livres qui ont une note moyenne supérieure à 4
```javascript
db.livres.find({ note_moyenne: { $gt: 4 } });
```

### 5️. Liste des utilisateurs habitant dans une ville spécifique
```javascript
db.utilisateurs.find({ "adresse.ville": "Lyon" });
```

### 6️. Trouver les livres qui appartiennent à un genre spécifique
```javascript
db.livres.find({ genre: "Science-fiction" });
```

### 7️. Trouver les livres avec un prix inférieur à 15€ et une note moyenne supérieure à 4
```javascript
db.livres.find({ prix: { $lt: 15 }, note_moyenne: { $gt: 4 } });
```

### 8️. Trouver les utilisateurs qui ont emprunté un livre spécifique
```javascript
db.utilisateurs.find({ "livres_empruntes.titre": "Les Ombres du Passé" });
```

## Partie 3 : Mise à jour de documents (Update)

### 1️. Mettre à jour le titre d'un livre spécifique
```javascript
db.livres.updateOne({ titre: "Les Ombres du Passé" }, { $set: { titre: "Les Ombres du Futur" } });
```

### 2️. Ajouter un champ stock à tous les livres
```javascript
db.livres.updateMany({}, { $set: { stock: 5 } });
```

### 3️. Marquer un livre comme indisponible
```javascript
db.livres.updateOne({ titre: "L’Âme du Temps" }, { $set: { disponible: false } });
```

### 4️. Ajouter un nouvel emprunt à un utilisateur
```javascript
db.utilisateurs.updateOne(
  { email: "apolline.riviere@example.com" },
  {
    $push: {
      livres_empruntes: {
        livre_id: ObjectId("ID_LIVRE"),
        titre: "Le Dernier Songe d’Atlas",
        date_emprunt: new Date("2023-03-01"),
        date_retour_prevue: new Date("2023-04-01")
      }
    }
  }
);
```

### 5️. Changer l'adresse d'un utilisateur
```javascript
db.utilisateurs.updateOne(
  { email: "loic.garcia@gmail.com" },
  { $set: { "adresse.ville": "Paris" } }
);
```

### 6️. Ajouter un nouveau tag à un utilisateur
```javascript
db.utilisateurs.updateOne(
  { email: "eleonore.meunier@gmail.com" },
  { $addToSet: { tags: "magie" } }
);
```

### 7️. Mettre à jour la note moyenne d'un livre
```javascript
db.livres.updateOne({ titre: "Nébuleuses Mortelles" }, { $set: { note_moyenne: 4.4 } });
```

## Partie 4 : Suppression de documents (Delete)

### 1️. Supprimer un livre spécifique par son titre
```javascript
db.livres.deleteOne({ titre: "Sous le Ciel d’Émeraude" });
```

### 2️. Supprimer tous les livres d'un auteur spécifique
```javascript
db.livres.deleteMany({ auteur: "Marion Laforet" });
```

### 3️. Supprimer un utilisateur par son email
```javascript
db.utilisateurs.deleteOne({ email: "matthieu.boutin@gmail.com" });
```

## Partie 5 : Requêtes avancées et projection

### 1️. Liste des livres triés par note moyenne (ordre décroissant)
```javascript
db.livres.find().sort({ note_moyenne: -1 });
```

### 2️. Trouver les 3 livres les plus anciens
```javascript
db.livres.find().sort({ annee_publication: 1 }).limit(3);
```

### 3️. Compter le nombre de livres par auteur
```javascript
db.livres.aggregate([
  { $group: { _id: "$auteur", nombre_de_livres: { $sum: 1 } } }
]);
```

### 4️. Afficher uniquement le titre, l’auteur et la note moyenne des livres (sans l’_id)
```javascript
db.livres.find({}, { _id: 0, titre: 1, auteur: 1, note_moyenne: 1 });
```

### 5️. Trouver les utilisateurs ayant emprunté plus d’un livre
```javascript
db.utilisateurs.find({ "livres_empruntes.1": { $exists: true } });
```

### 6️. Rechercher les livres dont le titre contient un mot spécifique (ex: "Temps")
```javascript
db.livres.find({ titre: { $regex: "Temps", $options: "i" } });
```

### 7️. Trouver les livres dont le prix est entre 10€ et 20€
```javascript
db.livres.find({ prix: { $gte: 10, $lte: 20 } });
```

## Partie 6 : Modélisation des données (Mini-exercice)

### 1️. Création de la collection emprunts
```javascript
db.createCollection("emprunts");
```

### 2️. Insertion de 3 emprunts
```javascript
db.emprunts.insertMany([
  {
    utilisateur_id: ObjectId("ID_UTILISATEUR_1"),
    livre_id: ObjectId("ID_LIVRE_1"),
    date_emprunt: new Date("2023-03-02"),
    date_retour_prevue: new Date("2023-04-02"),
    date_retour_effective: null,
    statut: "en cours"
  },
  {
    utilisateur_id: ObjectId("ID_UTILISATEUR_2"),
    livre_id: ObjectId("ID_LIVRE_2"),
    date_emprunt: new Date("2023-02-15"),
    date_retour_prevue: new Date("2023-03-15"),
    date_retour_effective: new Date("2023-03-14"),
    statut: "retourné"
  },
  {
    utilisateur_id: ObjectId("ID_UTILISATEUR_3"),
    livre_id: ObjectId("ID_LIVRE_3"),
    date_emprunt: new Date("2023-02-25"),
    date_retour_prevue: new Date("2023-03-25"),
    date_retour_effective: null,
    statut: "en retard"
  }
]);
