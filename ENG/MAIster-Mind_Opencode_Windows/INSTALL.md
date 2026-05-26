# =========================================================================
# WINDOWS: AI FACTORY CONFIGURATION (VIA WSL)
# =========================================================================

# 1. System update and installation of basic tools in WSL
sudo apt update && sudo apt install -y tmux curl ca-certificates git build-essential

# 2. Install Node.js 22 in WSL
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs

# 3. Global installation of OpenCode and authentication in WSL
# Install opencode: https://opencode.ai/docs/en
