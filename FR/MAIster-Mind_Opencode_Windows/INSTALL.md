# =========================================================================
# WINDOWS: À EXÉCUTER UNIQUEMENT DANS LE TERMINAL WSL (UBUNTU)
# =========================================================================

# 1. Mise à jour du système et installation des outils de base
sudo apt update && sudo apt install -y tmux curl ca-certificates git build-essential

# 2. Installation de Node.js 22
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs

# 3. Installation globale d'OpenCode
# Installe opencode: https://opencode.ai/docs/fr
