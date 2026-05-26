# MAIster-Mind V1 (Version Windows via WSL)
*Par Selim Boukhari* — [LinkedIn](https://www.linkedin.com/in/selim-boukhari-6356b949/)

**MAIster-Mind** est une usine à code automatisée qui pilote l'agent IA OpenCode via `tmux` dans un environnement WSL 2. Son but est d'automatiser le développement (du besoin fonctionnel jusqu'au code final) à travers un pipeline structuré et validé par l'humain, évitant ainsi la dépendance exclusive aux modèles distants les plus coûteux.

> 📦 Pour l'installation initiale, voir `INSTALL.md`.

---

## 🚀 Architecture des Scripts

L'usine propose 2 scripts selon ta cible et ton objectif (Code seul vs Code + Tests) :

| Script | Gestion des Tests |
|---|---|
| `MAIsterMind-code` | Production uniquement |
| `MAIsterMind-code` | Production + Tests |

---

## 🛠️ Choix du modèle

Le modèle se configure directement dans OpenCode (via `/model` dans le TUI ou dans `.opencode/opencode.json`). 
* **Cloud :** Assure-toi d'avoir configuré ton provider avec ta clé API dans ton environnement WSL.
* **Local :** Lance Ollama en arrière-plan avec ton modèle avant d'exécuter un binaire. Assure-toi qu'Ollama est accessible depuis WSL avant d'exécuter un script local.

---


## 📂 Structure Minimale du Projet

```text
├── need.md                     ← Le brief fonctionnel précis (À CRÉER)
├── plan.md                     ← Plan séquentiel (Généré par l'IA)
├── blackboard.yaml             ← Suivi des phases (Généré par l'IA)
├── refactoring_report.md       ← Rapport d'audit final (Généré par l'IA)
├── .agents/skills/             ← Directives techniques par domaine (architecture, frontend...)
├── .opencode/agents/factory.md ← Configuration de l'agent OpenCode automatisé
├── MAIsterMind-code             ← Compiled orchestrator binary (CODE ONLY)
└── MAIsterMind-test             ← Compiled orchestrator binary (CODE + TESTS)
```

---

## 🛠️ Le Pipeline en 4 Étapes

Chaque script exécute automatiquement les étapes suivantes :

1. **Planification :** Analyse de `need.md` et génération de `plan.md`.
2. **Blackboard :** Conversion du plan en un fichier YAML structuré (`blackboard.yaml`).
3. **Production :** Boucle itérative de code (et/ou tests) avec critique automatique et retries.
4. **Refactoring :** Audit de fin et nettoyage de la codebase avec rapport final.

---

## 💻 Utilisation au Quotidien

*Toutes les commandes s'exécutent dans ton terminal WSL (Ubuntu), dans le dossier du projet.*

### 0. Pour l'usage d'un modèle local

Commence par rendre Ollama accessible depuis WSL. Le plus simple est de lancer Ollama côté Windows, puis de vérifier depuis le terminal WSL que l'API répond correctement.

Tant que la configuration d'Ollama reste active et accessible depuis WSL, tu peux exécuter le binaire depuis le terminal WSL pour lancer MAIster-Mind avec ton modèle local.

### 1. Préparer le besoin
Crée et décris ton besoin fonctionnel sans ambiguïté dans `need.md`.

### 2. Lancer l'orchestrateur
Exécute le binaire de ton choix depuis le terminal WSL. Exemple pour le cloud avec tests :

```bash
./MAIsterMind-test
```

### 3. Valider le Blackboard (Human-in-the-loop)
Le script s'arrête après l'étape 2 et attend ta validation. Tu peux :
* Taper `y` pour lancer la production.
* Taper `n` pour tout stopper.
* **Optionnel :** Modifier directement le fichier `blackboard.yaml` pour ajuster les phases de l'IA avant de taper `y`.

### 4. Suivre ou Stopper l'IA
* **Suivre en direct :** Ouvre un second terminal WSL et tape :

```bash
tmux attach -t oc-factory
```

  *(Pour sortir de l'affichage sans couper l'IA : Ctrl + B puis D).*
* **Arrêter le processus :** Fais simplement un `Ctrl + C` dans le terminal de l'orchestrateur pour fermer proprement la session.

---

## 💡 Commandes Utiles & Dépannage

* **Suivre l'avancement du YAML :** `watch -n 2 cat blackboard.yaml`
* **Forcer la fermeture de l'IA :** `tmux kill-session -t oc-factory`
* **Reprendre après un crash :** Relance le binaire, les phases marquées `DONE` seront automatiquement ignorées.
