# Contributing to AutoVisit

Ce document définit comment le travail passe d'un ticket Jira à du code mergé dans `main`. Il existe pour que chaque contribution — même en solo — suive la même discipline qu'une vraie équipe d'ingénierie imposerait. Lis-le une fois, puis suis-le à chaque ticket.

---

## 1. Stratégie de branches

`main` reflète toujours l'état actuel et fonctionnel du projet. Personne ne commit dessus directement — tout le travail se fait sur des branches de fonctionnalité à courte durée de vie.

### Nommage des branches

```text
feature/AV-<numéro-ticket>-<slug-court>
```

Exemples :

- `feature/AV-13-repo-structure`
- `feature/AV-24-slot-constraint-engine`
- `feature/AV-31-counter-visit-state-machine`

Autres préfixes, utilisés uniquement quand ils s'appliquent réellement :

| Préfixe    | Quand l'utiliser                                                                         |
| ---------- | ---------------------------------------------------------------------------------------- |
| `feature/` | Nouvelle fonctionnalité liée à une Story ou Task Jira (le cas par défaut — presque tout) |
| `fix/`     | Correction de bug liée à un ticket Bug Jira                                              |
| `chore/`   | Outillage, configuration, maintenance sans impact utilisateur                            |
| `docs/`    | Changements de documentation uniquement, non liés à une story de code                    |

### Règles

- Une branche = un ticket Jira. Ne mélange pas plusieurs tâches sans rapport dans la même branche.
- Crée toujours ta branche depuis le dernier `main` :

```bash
git checkout main && git pull && git checkout -b feature/AV-XX-slug
```

- Garde les branches courtes — ouvre la PR dès que le travail est prêt pour la revue, ne laisse pas une branche vivre plus de quelques jours.
- Supprime la branche une fois mergée.

---

## 2. Messages de commit — Conventional Commits

Chaque message de commit suit ce format :

```text
<type>(AV-<numéro-ticket>): <description courte à l'impératif>
```

### Types

| Type       | Signification                                                        |
| ---------- | -------------------------------------------------------------------- |
| `feat`     | Nouvelle fonctionnalité ou capacité utilisateur                      |
| `fix`      | Correction de bug                                                    |
| `test`     | Ajout ou correction de tests, aucun changement de code de production |
| `refactor` | Changement de code qui ne corrige ni n'ajoute rien de fonctionnel    |
| `docs`     | Documentation uniquement                                             |
| `chore`    | Outillage, dépendances, configuration CI, structure de projet        |

### Exemples

```text
feat(AV-24): add constraint-based slot search engine
test(AV-31): add counter-visit window expiry test with fixed clock
fix(AV-27): prevent booking beyond lane capacity
docs(AV-17): add ADR-003 for package-by-layer convention
chore(AV-13): initial repo structure and branch strategy
```

### Règles

- Toujours référencer le numéro de ticket Jira — c'est ce qui relie le commit à _pourquoi_ le changement existe.
- Utiliser l'impératif ("add", pas "added" ni "adds") — cohérent avec les conventions de Git lui-même.
- Garder la ligne de résumé sous ~72 caractères. Ajouter un corps en dessous si plus d'explication est nécessaire.
- Un changement logique par commit quand c'est possible. Ne pas regrouper des changements sans rapport.

---

## 3. Processus de Pull Request

**Aucun code n'atteint `main` sans passer par une Pull Request — même en travaillant seul.** La PR est la porte de qualité : c'est là que la CI Jenkins tourne, que le diff reçoit un second regard, et que la revue Tech Lead a lieu.

### Avant d'ouvrir une PR

- [ ] Le code implémente les critères d'acceptation du ticket (vérifie la description Jira)
- [ ] Les tests pertinents sont écrits et passent localement
- [ ] Les règles `ArchUnit` passent toujours, si applicable
- [ ] Aucun secret, identifiant, ou fichier `.env` inclus dans le diff
- [ ] Les messages de commit suivent la convention ci-dessus

### Template de description de PR

```markdown
Closes AV-XX

## What

[Une ou deux phrases décrivant le changement.]

## Why

[Le problème résolu ou l'exigence remplie. Référence la section de spec si pertinent.]

## How tested

[Tests unitaires / intégration / vérification manuelle. Sois précis.]
```

### Règles

- Une branche = un ticket Jira. Ne mélange pas plusieurs tâches sans rapport dans la même branche.
- Crée toujours ta branche depuis le dernier `main` :

```bash
git checkout main && git pull && git checkout -b feature/AV-XX-slug
```

- Garde les branches courtes — ouvre la PR dès que le travail est prêt pour la revue, ne laisse pas une branche vivre plus de quelques jours.
- Supprime la branche une fois mergée.

---

### Règles

- Toujours référencer le numéro de ticket Jira — c'est ce qui relie le commit à _pourquoi_ le changement existe.
- Utiliser l'impératif ("add", pas "added" ni "adds") — cohérent avec les conventions de Git lui-même.
- Garder la ligne de résumé sous ~72 caractères. Ajouter un corps en dessous si plus d'explication est nécessaire.
- Un changement logique par commit quand c'est possible. Ne pas regrouper des changements sans rapport.

---

### Règles

- Une story Jira = une PR, dans la mesure du possible. Ne pas regrouper plusieurs stories sans rapport.
- Le titre de la PR doit correspondre au résumé du ticket ou en découler clairement.
- Passer le ticket Jira en **"Revue en cours"** dès l'ouverture de la PR.
- Attendre l'approbation de revue avant de merger — ne jamais auto-merger sans revue, même sous pression de temps.
- Utiliser un **squash merge** vers `main` pour garder un historique lisible (un commit propre par story sur `main`, le détail complet reste sur la branche/PR).
- Supprimer la branche après le merge.

---

## 4. Definition of Done

Un ticket n'est **jamais** "Terminé" tant que **tout** ce qui suit n'est pas vrai :

1. Les critères d'acceptation du ticket Jira sont remplis.
2. Les tests automatisés pertinents existent et passent (unitaires / intégration / architecture, selon le ticket).
3. Le seuil de couverture de code est maintenu (dès que JaCoCo est actif à partir du Sprint 1).
4. Le quality gate SonarQube est vert, aucun nouveau blocker/critical (dès que SonarQube est actif à partir du Sprint 1).
5. PR ouverte, revue, et approuvée.
6. Documentation pertinente mise à jour (README, ADR, `DESIGN-PATTERNS.md`, selon le cas).
7. Mergé dans `main` avec la CI verte.
8. Le ticket Jira est passé à **"Terminé"**.

Si l'un de ces points n'est pas vrai, le ticket reste ouvert — résiste à l'envie de le marquer Terminé juste parce que le code "marche sur ta machine".

---

## 5. Résumé du flux (la boucle à répéter à chaque ticket)

1. Passer le ticket Jira en **"En cours"**.
2. `git checkout -b feature/AV-XX-slug` depuis le dernier `main`.
3. Écrire le code et les tests.
4. Commit en Conventional Commits, en référençant le ticket.
5. Push de la branche, ouverture d'une PR avec le template ci-dessus.
6. Passer le ticket Jira en **"Revue en cours"**.
7. Soumettre la PR pour revue.
8. Traiter les retours de revue s'il y en a.
9. Merger (squash) après approbation.
10. Passer le ticket Jira en **"Terminé"**. Supprimer la branche.
