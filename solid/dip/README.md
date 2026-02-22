# Principe d'inversion des dépendances (DIP)

## Motivation

En OOP, les modules de haut niveau contiennent la logique métier importante. Si ces modules dépendent directement des modules de bas niveau, un simple changement dans un détail d'implémentation peut casser toute l'application.

## Problème

Regardons le morceau de code suivant :

```java
// Classe de bas niveau — détail d'implémentation
class MySQLDatabase {
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}

// Classe de haut niveau — logique métier
class UserService {
    private MySQLDatabase database;

    public UserService() {
        // UserService crée lui-même sa dépendance — couplage fort
        this.database = new MySQLDatabase();
    }

    public void saveUser(String user) {
        database.save(user);
    }
}

class Main {
    public static void main(String args[]) {
        UserService service = new UserService();
        service.saveUser("Alice");
        // Tout semble fonctionner...
        // Mais si on veut remplacer MySQL par MongoDB, il faut modifier UserService directement
    }
}
```

On peut voir que `UserService` est directement couplé à `MySQLDatabase`. Si on veut changer la base de données, on est obligé de modifier la logique métier — ce qui viole le principe ouvert/fermé en même temps. Un autre problème encore plus évident serait le suivant :

```java
// Impossible de tester UserService sans une vraie base de données
// Aucun moyen d'injecter un mock ou une alternative
class UserServiceTest {
    public static void main(String args[]) {
        UserService service = new UserService(); // crée toujours un MySQLDatabase
        service.saveUser("Test"); // on ne peut pas contrôler le comportement
    }
}
```

## Principe

Le principe d'inversion des dépendances énonce deux règles :

1. **Les modules de haut niveau ne doivent pas dépendre des modules de bas niveau.** Les deux doivent dépendre d'abstractions.
2. **Les abstractions ne doivent pas dépendre des détails.** Ce sont les détails qui doivent dépendre des abstractions.

En d'autres termes, `UserService` ne doit pas savoir que c'est MySQL qui stocke les données — il doit seulement savoir qu'il existe quelque chose capable de stocker des données.

## Solution

On introduit une interface qui représente le contrat, et on injecte la dépendance depuis l'extérieur.

```java
// L'abstraction — le contrat
interface Database {
    void save(String data);
}

// Détail d'implémentation A
class MySQLDatabase implements Database {
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}

// Détail d'implémentation B
class MongoDB implements Database {
    public void save(String data) {
        System.out.println("Saving to MongoDB: " + data);
    }
}

// Classe de haut niveau — dépend uniquement de l'abstraction
class UserService {
    private Database database;

    // La dépendance est injectée depuis l'extérieur
    public UserService(Database database) {
        this.database = database;
    }

    public void saveUser(String user) {
        database.save(user);
    }
}

class Main {
    public static void main(String args[]) {
        Database db = new MySQLDatabase(); // on choisit le détail ici
        UserService service = new UserService(db);
        service.saveUser("Alice");

        // Changer de base de données ne touche pas à UserService
        Database db2 = new MongoDB();
        UserService service2 = new UserService(db2);
        service2.saveUser("Bob");
    }
}
```

`UserService` ne connaît plus aucun détail d'implémentation. On peut changer, tester, ou remplacer la base de données sans jamais toucher à la logique métier.
