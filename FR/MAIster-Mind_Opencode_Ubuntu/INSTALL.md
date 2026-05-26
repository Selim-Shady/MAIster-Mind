# =========================================================================
# UBUNTU / DEBIAN: AI FACTORY CONFIGURATION (COMPILED BINARY)
# =========================================================================

# 1. System update and installation of basic tools
sudo apt update && sudo apt install -y tmux curl ca-certificates

# 2. Install Node.js 22 (required for some dependencies)
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs

# 3. Global installation of OpenCode and authentication
# Install opencode: https://opencode.ai/docs/en

# 4. If needed, add permission on your scripts
# As example: chmod +x MAIsterMind-code
