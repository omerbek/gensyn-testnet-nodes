### BlockAssist Node Setup Guide

Minecraft AI assistant node running on WSL with GUI support.

### Prerequisites
- Windows 10/11 with WSL2
- Ubuntu in WSL
- VcXsrv X Server for Windows
- Python 3.10+

### Installation Steps

### 1. Install VcXsrv
Download and install from [sourceforge.net/projects/vcxsrv](https://sourceforge.net/projects/vcxsrv/)

### 2. WSL Setup
```bash
# Remove old installation
rm -rf ~/blockassist
```

# Clone repository
```
git clone https://github.com/gensyn-ai/blockassist.git
cd blockassist
```

# Run setup script
```
./setup.sh
```

3. Install pyenv and Python 3.10
```
curl -fsSL https://pyenv.run | bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
source ~/.bashrc
```
# Install dependencies and Python
```
sudo apt update
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```
```
pyenv install 3.10.0
pyenv global 3.10.0
```
### 4. Install Node.js and Yarn
```
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
nvm install --lts
nvm alias default 'lts/*'
nvm use default
```
```
corepack enable
corepack prepare yarn@stable --activate
```
### 5. Install Poetry and Dependencies
```
curl -sSL https://install.python-poetry.org | python3 -
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
pip install psutil readchar rich
```
### 6. Configure X11 Display

# Install X11 tools
```
sudo apt-get update -y
sudo apt-get install -y x11-apps x11-xserver-utils
```
# Set display (replace with your Windows IP)

export DISPLAY=192.168.1.xx:0
# Test X11 (you will see two eyes :) )
```
xeyes
```

### 7. Setup OpenGL Environment

```
export LIBGL_ALWAYS_SOFTWARE=1
export MESA_LOADER_DRIVER_OVERRIDE=llvmpipe
export LIBGL_ALWAYS_INDIRECT=1
export MESA_GL_VERSION_OVERRIDE=2.1
export _JAVA_OPTIONS='-Xms512m -Xmx2g -Dorg.lwjgl.opengl.Display.allowSoftwareOpenGL=true'
```

### 8. Login and Run BlockAssist
```
cd modal-login
yarn install
yarn dev
```
###  Access http://localhost:3000 to login, then:
```
cd ~/blockassist
python3 -m venv blockassist-venv
source blockassist-venv/bin/activate
pip install -e .
python3 run.py
```

###  Daily Startup
```
cd blockassist
source blockassist-venv/bin/activate
python3 run.py
```
