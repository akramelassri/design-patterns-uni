# Design Patterns Projet

**Réalisé par:**

- **Mohamed Anas Charkaoui**
- **Akram El Assri**

Ce dépot contient le projet pour le module de _Design Pattern_. Il met l'accent sur l'étude et l'application des principes _SOLID_ et quelques _Patrons de Conception_ couramment utilisés.

## Introduction

Ecrire du code robuste, maintenable, et extensible est une nécessité pour le développement des logiciels de qualité. En effet, du _spaghetti code_ ou du code mal conçu devient:

- Difficile a comprendre.
- Difficile d'ajouter des fonctionnalites sans casser d'autre
- Impossible d'ajouter des tests.
- Difficile de changer l'implementation des composants logiciels.

D'où l'importance de la bonne réalisation d'un programme.

Un code elegant est distingué par deux choses principaux :

- Un faible couplage
- Une haute Cohésion

C'est quoi le couplage et la cohesion? Et quelle est leur importance dans le development des applications?
Couplage
le couplage est le concept de dependance entre deux modules/composants d'un code. Si le changement d'une partie necessite qu'on change significativement l'autre on parle d'haut couplage. Par consequent un seul changement peut se repercuter dans tout le code.

Cohesion
l'homogenite et la relation semantique entre partie de code. on vise de grouper le code avec haute cohesion et distancer le code avec faible cohesion
example:

pour atteindre un code qui est robuste et clean,il existe un exemple de pratique et concepts comme SOLID, GoF design patterns

## SOLID

Un ensemble de 5 pratiques qui compose l'acronyme solid,.

- [Single Responsibility Principle](./solid/srp/README.md)
- [Open Closed Principle](./solid/ocp/README.md)
- [Liskov's Substitution Principle](./solid/lsp/README.md)
- [Interface Segregation Principle](./solid/isp/README.md)
- [Dependency Inversion Principle](./solid/dip/README.md)

## Design Patterns

Ces patterns sont des solutions a des problemes courrament recontre
