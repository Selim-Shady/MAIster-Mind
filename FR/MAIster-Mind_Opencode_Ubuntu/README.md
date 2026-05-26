# MAIster-Mind V1 (Version Ubuntu / Debian — BINAIRE COMPILÉ)
*Par Selim Boukhari* — [LinkedIn](https://www.linkedin.com/in/selim-boukhari-6356b949/)

**MAIster-Mind** est une usine à code automatisée qui pilote l'agent IA OpenCode via `tmux` directement sous Ubuntu. Son but est d'automatiser le développement (du besoin fonctionnel jusqu'au code final) à travers un pipeline structuré et validé par l'humain, évitant ainsi la dépendance exclusive aux modèles distants les plus coûteux.

> 📦 Voir `INSTALL.md` pour la configuration minimale.

---

## 🚀 Architecture des Scripts

L'usine propose 2 **binaires compilés** selon ta cible et ton objectif (Code seul vs Code + Tests) :

| Binaire | Gestion des Tests | Commande |
|---|---|---|
| `MAIsterMind-code` | Production uniquement | `./MAIsterMind-code` |
| `MAIsterMind-test` | Production + Tests | `./MAIsterMind-test` |

---

## 🛠️ Choix du Modèle

Le modèle se configure directement dans OpenCode (via `/model` dans le TUI ou dans `.opencode/opencode.json`).
* **Cloud :** Assure-toi d'avoir configuré ton provider.
* **Local :** Lance Ollama en arrière-plan avec ton modèle avant d'exécuter un binaire. 

---

## 📂 Structure Minimale du Projet

```text
├── need.md                      ← Le brief fonctionnel précis (À CRÉER)
├── plan.md                      ← Plan séquentiel (Généré par l'IA)
├── blackboard.yaml              ← Suivi des phases (Généré par l'IA)
├── refactoring_report.md        ← Rapport d'audit final (Généré par l'IA)
├── .agents/skills/              ← Directives techniques par domaine (architecture, frontend...)
├── .opencode/agents/factory.md  ← Configuration de l'agent OpenCode automatisé
├── MAIsterMind-code             ← Binaire orchestrateur compilé (CODE SEUL)
└── MAIsterMind-test             ← Binaire orchestrateur compilé (CODE + TESTS)
```

---

## 💻 Utilisation au Quotidien

*Toutes les commandes s'exécutent dans ton terminal Ubuntu, dans le dossier du projet.*

### 1. Préparer le besoin
Crée et décris ton besoin fonctionnel sans ambiguïté dans `need.md`. Sois extrêmement précis et indique où trouver les fichiers à référencer.

### 2. Lancer l'orchestrateur directement (pas de venv nécessaire !)
Exécute directement le binaire compilé de ton choix. Exemple pour le cloud avec tests :
```bash
./MAIsterMind-test
```

### 3. Valider le Blackboard (Human-in-the-loop)
Le script s'arrête après l'étape 2 et attend ta validation. Tu peux :
* Taper `y` pour lancer la production.
* Taper `n` pour tout stopper (la session tmux sera tuée).
* **Optionnel :** Modifier directement le fichier `blackboard.yaml` dans un autre terminal pour ajuster les phases de l'IA avant de taper `y`.

### 4. Suivre ou Stopper l'IA
* **Suivre en direct :** Ouvre un second terminal et tape :
  ```bash
  tmux attach -t oc-factory
  ```
  *(Pour sortir de l'affichage sans couper l'IA : Ctrl + B puis D).*
* **Arrêter le processus :** Fais simplement un `Ctrl + C` dans le terminal de l'orchestrateur pour fermer proprement la session.

> ⚠️ **Important :** Ne laisse pas le code produit devenir une boîte noire, prends toujours le temps de le relire !

---

## 💡 Commandes Utiles & Dépannage

* **Suivre l'avancement du YAML :** `watch -n 2 cat blackboard.yaml`
* **Forcer la fermeture de l'IA :** `tmux kill-session -t oc-factory`
* **Reprendre après un crash :** Relance le binaire, les phases marquées `DONE` seront automatiquement ignorées.
* **Le binaire ne s'exécute pas ?** Vérifie que tu es dans le bon répertoire et que le binaire a les permissions d'exécution.

---

## 🔒 Protection du Code Source

Ce binaire a été compilé avec **Nuitka** pour protéger la propriété intellectuelle de la logique de l'orchestrateur.
- Le code source **n'est pas accessible** depuis le binaire
- Toutes les dépendances Python (PyYAML, etc.) sont **intégrées** dans le binaire
- Le binaire est **autonome** et ne nécessite pas d'avoir Python installé sur le système cible

---

## Optionnel: Forcer l'usage d'un modèle

Aller dans .opencode
Intégrer le modèle dans opencode.json

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "opencode/deepseek-v4-flash"
}
```