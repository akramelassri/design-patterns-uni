# Design Patterns Projet

**Réalisé par:**

- **Mohamed Anas Charkaoui**
- **Akram El Assri**

Ce dépot contient le projet pour le module de _Design Pattern_. Il met l'accent sur l'étude et l'application des principes _SOLID_ et quelques _Patrons de Conception_ couramment utilisés.

## Introduction

Écrire du code robuste, maintenable et extensible est une nécessité pour le développement de logiciels de qualité. En effet, le spaghetti code ou un code mal conçu devient :

- Difficile à comprendre.
- Difficile d'ajouter des fonctionnalités sans casser d'autres parties.
- Impossible d'ajouter des tests.
- Difficile de changer l'implémentation des composants logiciels.

D'où l'importance d'une bonne conception et réalisation d'un programme.

Un code élégant se distingue par deux choses principales :

- Un faible couplage
- Une haute cohésion

Tous les principes et pratiques d'amélioration de la qualité du code visent principalement à réduire le couplage et à augmenter la cohésion.

### C'est quoi le couplage et la cohésion ? Et quelle est leur importance dans le développement des applications ?

#### Couplage

Le couplage est le concept qui décrit à quel point le changement d'une partie d'un système nécessitera le changement d'une autre partie. Un fort couplage signifie qu'un changement peut se répercuter sur d'autres zones du code.

Il faut savoir qu'un certain niveau de couplage est nécessaire pour la communication et la dépendance entre les composants d'un système. Ainsi, notre objectif est de le minimiser, en ne couplant les composants qu'à des interfaces ou des contrats spécifiques pour la communication entre eux (on parle alors de faible couplage), ce qui permet de changer l'implémentation d'une partie du système sans impacter les autres.

#### Cohésion

La cohésion représente la mesure dans laquelle une partie d’un code (par exemple un ensemble de classes) constitue une unité logique. Une haute cohésion vise à regrouper les éléments partageant une même logique ; cela peut être des classes regroupées dans un même module ou service, ou encore des fonctions regroupées dans une même classe.

Ceci permet qu'un changement dans une partie du système n'affecte que son unité logique.

Notre objectif principal est donc de regrouper les structures qui sont étroitement liées, tout en communiquant avec d'autres structures via des interfaces publiques bien définies, réalisant ainsi les concepts de haute cohésion et de faible couplage.

Plusieurs pratiques ont été développées pour atteindre cet objectif, parmi lesquelles on peut citer les principes SOLID et les design patterns du GoF.

## SOLID

Un ensemble de 5 pratiques qui compose l'acronyme solid

- [Single Responsibility Principle](./solid/srp/README.md)
- [Open Closed Principle](./solid/ocp/README.md)
- [Liskov's Substitution Principle](./solid/lsp/README.md)
- [Interface Segregation Principle](./solid/isp/README.md)
- [Dependency Inversion Principle](./solid/dip/README.md)

## Design Patterns

Ces patterns sont des solutions a des problemes courrament recontre
