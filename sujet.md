# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

## Réponse :
En 2014, la chanson "Gangnam Style" de l'artiste sud-coréen Psy est devenue la première vidéo sur YouTube à dépasser les 2 147 483 647 vues. Ce nombre n'est pas arbitraire ; il représente la valeur maximale que peut stocker un entier signé sur 32 bits.

Description du bug :
YouTube utilisait un int (32 bits) pour compter le nombre de vues des vidéos. Ce type de variable peut stocker des valeurs allant de -2 147 483 648 à 2 147 483 647. Lorsque le compteur de vues de "Gangnam Style" a dépassé cette limite supérieure, il a provoqué un dépassement de capacité (overflow), entraînant un affichage incorrect du nombre de vues.

Bug local ou global :
Le bug était global car il concernait le système de comptage de vues de toute la plateforme YouTube. Cependant, il ne s'est manifesté que lorsqu'une vidéo a atteint un nombre de vues exceptionnellement élevé, ce qui était le cas unique de "Gangnam Style".

Répercussions pour les clients/consommateurs et l'entreprise :
Pour les utilisateurs, le bug a été principalement une curiosité sans impact majeur sur l'expérience globale de la plateforme. Pour YouTube, en revanche, cela a mis en évidence une limitation technique nécessitant une correction rapide. L'entreprise a dû mettre à jour son système de comptage en passant à un entier de 64 bits, capable de gérer un nombre de vues bien plus élevé, évitant ainsi que le problème ne se reproduise.

Tests et découverte du bug :
Il est évident que ce bug était prévisible, et que des tests aurait pu le mettre en évidence. C'est juste que Youtube ne s'attendait pas à avoir des vidéos dépassant ce nombre de vue.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

## Réponse
Analyse du bug COLLECTIONS-697 dans Apache Commons Collections

La classe FixedSizeList du projet Apache Commons Collections est destinée à créer une liste dont la taille est fixe, empêchant l'ajout ou la suppression d'éléments après sa création. Cependant, il a été découvert que si l'on conserve une référence à la liste originale utilisée pour créer la FixedSizeList et que l'on modifie cette liste sous-jacente (par exemple en y ajoutant ou supprimant des éléments), ces modifications se reflètent également dans la FixedSizeList. Cela permet donc indirectement de changer la taille de la FixedSizeList, contournant ainsi sa contrainte de taille fixe.

Classification du bug :

Le bug est local, car il est spécifique à la classe FixedSizeList et n'affecte pas les autres composants du projet.

Solution apportée :

Plutôt que de modifier le comportement de la classe, les développeurs ont choisi d'améliorer la documentation (JavaDoc) pour informer les utilisateurs de ce comportement. La documentation mise à jour avertit désormais explicitement que :

    La FixedSizeList reflète les modifications apportées à la liste sous-jacente.
    Il est déconseillé de modifier la liste originale après avoir créé une FixedSizeList, afin de préserver la contrainte de taille fixe.

Cette approche permet d'éviter des modifications potentiellement risquées du code existant, tout en informant clairement les utilisateurs du comportement réel de la classe.

Ajout de tests :

Oui, un test unitaire a été ajouté pour illustrer et vérifier ce comportement. Le test consiste à :

    Créer une liste originale et y ajouter quelques éléments.
    Créer une FixedSizeList à partir de cette liste.
    Modifier la liste originale en y ajoutant un nouvel élément.
    Vérifier que la taille de la FixedSizeList a également augmenté, confirmant que la modification de la liste sous-jacente affecte la FixedSizeList.

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

## Réponse :

Netflix a mis en place des protocoles de test en production connus sous le nom de "Chaos Engineering". Une des expériences clés est l'utilisation de "Chaos Monkey", un outil qui arrête aléatoirement des instances de machines virtuelles hébergeant des services de production. L'objectif est de tester la résilience du système face à des pannes soudaines.

Les exigences pour ces expériences :

    Exécution en production : Les tests sont effectués dans l'environnement réel pour capturer les interactions authentiques des utilisateurs.
    Automatisation : Les expériences sont automatisées pour être exécutées continuellement, garantissant que le système reste résilient face aux changements constants.
    Hypothèses sur le comportement stable : Formuler des hypothèses basées sur des métriques clés du système, comme le nombre de démarrages de streaming par seconde (SPS).
    Variation d'événements réels : Les tests doivent simuler des conditions réelles, telles que des pannes de serveurs ou des pics de trafic inattendus.

Les variables observées :

    Métriques de comportement stable : Principalement le SPS, qui indique si les utilisateurs peuvent commencer à regarder des vidéos sans problème.
    Métriques fines : Latence des requêtes, utilisation du CPU, etc., pour détecter des dégradations non perceptibles par l'utilisateur final.
    Réactions du système : Comment le système se dégrade ou s'adapte face aux pannes injectées.

Les principaux résultats obtenus :

    Amélioration de la résilience : Les services sont conçus pour tolérer les pannes d'instances individuelles sans affecter l'expérience utilisateur.
    Détection proactive des faiblesses : Les tests ont permis d'identifier et de corriger des points faibles avant qu'ils ne causent des incidents majeurs.
    Culture de tolérance aux pannes : Les ingénieurs intègrent dès le départ la résilience dans la conception des services.

D'autres grandes entreprises comme Amazon, Google, Microsoft et Facebook appliquent des techniques similaires pour tester la résilience de leurs systèmes distribués.


4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested?

Avantages principaux d'une spécification formelle pour WebAssembly :

    Sécurité accrue : Une spécification formelle élimine les ambiguïtés, garantissant que le code s'exécute de manière prévisible et sécurisée sur toutes les plateformes.

    Fiabilité et cohérence : Elle assure que toutes les implémentations respectent les mêmes règles, offrant une expérience utilisateur homogène quel que soit le navigateur.

    Interopérabilité et portabilité : WebAssembly peut fonctionner de manière identique sur différents environnements, facilitant le déploiement d'applications multiplateformes.

    Optimisation et performance : Les développeurs peuvent optimiser les moteurs d'exécution en toute confiance, sachant que les comportements sont bien définis.

    Facilitation de la vérification formelle : Permet l'utilisation d'outils formels pour prouver des propriétés de sécurité et de performance, renforçant la robustesse du langage.

Même avec une spécification formelle, les tests restent essentiels :

    Valider les implémentations réelles : Assurer que chaque moteur respecte fidèlement la spécification dans des scénarios pratiques.
    Détecter les bugs : Identifier et corriger les erreurs spécifiques à une implémentation.
    Garantir la performance : Vérifier que les optimisations respectent les attentes de performance définies.
    Assurer l'interopérabilité : Tester les interactions entre différentes implémentations et environnements.

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Réponse :

Selon Conrad Watt, la spécification mécanisée de WebAssembly avec Isabelle présente plusieurs avantages clés. Elle offre une formalisation précise et rigoureuse, améliorant la fiabilité et la sécurité en éliminant les ambiguïtés de la spécification initiale. Cette approche a permis d’identifier et de corriger des lacunes dans la spécification originale, renforçant ainsi sa solidité. De plus, la spécification mécanisée a conduit au développement d’artefacts supplémentaires, tels qu’un interpréteur exécutable vérifié et un vérificateur de types formellement prouvé. Pour vérifier la spécification, l’auteur a utilisé des preuves formelles dans Isabelle et des tests de fuzzing différentiel comparant l’interpréteur vérifié aux implémentations industrielles. Toutefois, cette nouvelle spécification ne supprime pas la nécessité de tests pratiques, qui restent essentiels pour valider les implémentations réelles et garantir leur robustesse et performance.
