---
marp: true
theme: uncover
class: invert
---

Rédiger des tests pour son application
5/6 - Automatiser leur lancement
===


---

## Objectif et solution

**Objectif**: Rendre obligatoire la qualité du travail que l'on produit


**solution**: Associer les tests au versioning
**Git** est le logiciel de gestion de versions le plus connu

---

## Les étapes conseillées

- Avant commit:
  - tests unitaires
  - tests d'intégration

- Avant push ou pull request:
  - tous les tests


NB: **Si échec, alors opération annulée**

---

## Husky

Module npm afin de greffer des déclencheurs sur git.


- crée un répertoire `.husky` à la racine du projet,
- renseigne ce répertoire dans notre configuration Git locale (`git config core.hooksPath '.husky'`)
- place des fichiers en équivalence des scripts de hooks (fichiers `pre-commit`, etc...)


---

### Mise en place

```bash
# On installe le module
npm install --save-dev husky
```

```bash
# On crée une ligne dans package.json
# "prepare": "husky install"
npm set-script prepare "husky install"
```

```bash
# Création du dossier .husky
# Et configuration Git locale
npm run prepare
```

---

### Hook Pré Commit

Ajout dans `scripts` de `package.json`
```json
    "test:unit": "jest",
    "test:integration": "jest",
```

Puis
```bash
npx husky add .husky/pre-commit \
'npm run test:unit && npm run test:integration'
```

La commande de tests se lancera avant chaque commit

---

### Hook Pré Push

Ajout dans `scripts` de `package.json`
```json
"test:e2e": "cypress",
"test": "npm run test:unit && npm run test:integration && npm test:e2e",
```

Puis
```bash
npx husky add .husky/pre-push 'npm run test'
```
