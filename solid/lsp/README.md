# Principe de substitution de Liskov (LSP)

## Motivation

On travaille toujours en OOP avec les classes dérivées, mais il faut faire attention — ce qui semble comme un simple héritage peut casser l'application.

## Problème

Regardons le morceau de code suivant:

```java
// On crée la classe de base
class Rectangle {
    protected int m_width;
    protected int m_height;

    public void setWidth(int width){ m_width = width; }
    public void setHeight(int height){ m_height = height; }
    public int getWidth(){ return m_width; }
    public int getHeight(){ return m_height; }
    public int getArea(){ return m_width * m_height; }
}

// On crée la classe carré qui redéfinit les méthodes setWidth et setHeight
class Square extends Rectangle {
    public void setWidth(int width){
        m_width = width;
        m_height = width;
    }
    public void setHeight(int height){
        m_width = height;
        m_height = height;
    }
}

class UserTest {
    private static Rectangle getNewRectangle() {
        // Comme un carré est dans ce cas un rectangle, ceci semble juste
        return new Square();
    }

    public static void main(String args[]) {
        Rectangle r = UserTest.getNewRectangle();
        r.setWidth(5);
        r.setHeight(10);
        // L'utilisateur assume que c'est un rectangle
        // il assume qu'il peut définir la largeur et la hauteur indépendamment
        System.out.println(r.getArea());
        // il est surpris du résultat de 100 alors qu'il attendait 50
    }
}
```

On peut voir que l'utilisateur obtient un résultat inattendu parce que les méthodes redéfinies ne fonctionnent plus de la même manière. La classe `Rectangle` établit un contrat implicite : `setWidth` et `setHeight` sont indépendantes. `Square` brise ce contrat en liant les deux dimensions ensemble. Un autre problème encore plus évident serait le suivant :

```java
// Ce code lèverait une erreur logique ou une exception
void testRectangle(Rectangle r) {
    r.setWidth(5);
    r.setHeight(10);
    assert r.getArea() == 50; // échoue si r est un Square
}
```

## Principe

Le principe de substitution de Liskov énonce que **toute classe dérivée doit pouvoir remplacer sa classe de base sans modifier le comportement correct du programme**. En d'autres termes, si ton code fonctionne avec un `Rectangle`, il doit fonctionner exactement de la même façon quand on lui passe un `Square` à la place — sans surprises, sans hypothèses brisées.

Formellement : si `S` est un sous-type de `T`, alors tout objet de type `T` peut être remplacé par un objet de type `S` sans altérer le bon fonctionnement du programme.

## Solution

Au lieu de faire hériter `Square` de `Rectangle`, les deux doivent hériter d'une abstraction commune — par exemple une interface `Shape` avec uniquement `getArea()`. Ainsi, aucune substitution incorrecte n'est impliquée et aucun contrat n'est brisé.

```java
interface Shape {
    int getArea();
}

class Rectangle implements Shape {
    protected int m_width, m_height;
    public Rectangle(int width, int height){
        m_width = width;
        m_height = height;
    }
    public int getArea(){ return m_width * m_height; }
}

class Square implements Shape {
    protected int m_side;
    public Square(int side){ m_side = side; }
    public int getArea(){ return m_side * m_side; }
}
```

Chaque classe respecte maintenant son propre contrat, et aucune substitution incorrecte n'est possible.