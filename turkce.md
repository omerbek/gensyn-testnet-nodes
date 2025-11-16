## TÜRKÇE README.md
# Gensyn Testnet Düğüm Kurulum Rehberi

Bu depo, üç farklı Gensyn testnet düğümü için kapsamlı kurulum rehberleri içerir:
- **RL-Swarm** - GPU tabanlı pekiştirmeli öğrenme düğümü
- **BlockAssist** - Minecraft AI asistan düğümü (WSL kurulumu)
- **CodeAssist** - Yerel AI kodlama asistanı

## 📋 Ön Gereksinimler

### Sistem Gereksinimleri
- **RL-Swarm**: NVIDIA GPU (RTX 3090/4090 önerilir), Ubuntu
- **BlockAssist**: WSL2 ile Windows 10/11, Python 3.10+
- **CodeAssist**: Minimum 8GB RAM, Docker, Python 3.8+

### Ortak Bağımlılıklar
- Git
- Python 3.8+
- Node.js & Yarn
- CUDA (GPU düğümleri için)

## 🚀 Hızlı Başlangıç

Düğümünüzü seçin ve özel kurulum rehberini takip edin:

### [RL-Swarm Kurulumu](./rl-swarm/kurulum.md)
Gensyn ağı için GPU hızlandırmalı pekiştirmeli öğrenme düğümü.

### [BlockAssist Kurulumu](./blockassist/kurulum.md)
WSL üzerinde GUI desteği ile çalışan Minecraft AI asistan düğümü.

### [CodeAssist Kurulumu](./codeassist/kurulum.md)
Docker ve Ollama kullanan tamamen yerel AI kodlama asistanı.

## 🔧 Ortak Araçların Kurulumu

### Python & Sanal Ortam
sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-pip python3-venv -y
python3 -m venv gensyn-env
source gensyn-env/bin/activate

### Node.js & Yarn
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g yarn

### Git
sudo apt install git -y

## 📊 İzleme & Destek

- **Dashboard**: https://dashboard.gensyn.ai/
- **Telegram Bot**: @gensynImpek_bot
- **Dokümantasyon**: https://docs.gensyn.ai/

## 🤝 Katkıda Bulunma

Hata bildirimleri ve geliştirme önerileri için çekinmeden katkıda bulunun!

## 📝 Lisans

Bu proje MIT Lisansı altında lisanslanmıştır.

## ⚠️ Sorumluluk Reddi

Bu bir testnet kurulum rehberidir. Ana ağ dağıtımları için daima resmi Gensyn dokümantasyonunu takip edin.

## RL-SWARM KURULUM (kurulum.md)
# RL-Swarm Düğüm Kurulum Rehberi

Bu rehber, Gensyn testnet için RL-Swarm GPU düğümünü kurmanıza yardımcı olacaktır.

## Sistem Gereksinimleri
- NVIDIA GPU (RTX 3090/4090 önerilir)
- Ubuntu 20.04/22.04
- 75-100 GB disk alanı
- CUDA uyumlu sürücüler

## Kurulum Adımları

### 1. Sistem Bağımlılıklarını Yükleyin
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install screen curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip -y

### 2. Python & Node.js Kurulumu
sudo apt install -y python3 python3-pip python3.10-venv
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g yarn

### 3. RL-Swarm'ı Klonlayın ve Kurun
git clone https://github.com/gensyn-ai/rl-swarm.git
cd rl-swarm
git pull

### 4. Sanal Ortam Oluşturun
screen -S swarm
python3 -m venv .venv
source .venv/bin/activate

### 5. RL-Swarm'ı Çalıştırın
./run_rl_swarm.sh

### 6. Erişim için Ngrok Kurulumu
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
tar -xvzf ngrok-v3-stable-linux-amd64.tgz
mv ngrok /usr/local/bin/
ngrok config add-authtoken AUTH_TOKEN_BURAYA
ngrok http 3000

### 7. Kurulumu Tamamlayın
- Tarayıcınızda yönlendirilmiş ngrok URL'sine erişin
- Google hesabınızla giriş yapın
- Swarm ekranındaki talimatları takip edin

## Önemli Dosyalar
- `swarm.pem` - Düğüm kimlik dosyanız (BU DOSYAYI YEDEKLEYİN)

## Screen Yönetimi
screen -r swarm          # Swarm oturumuna yeniden bağlan
CTRL + A ardından D      # Screen oturumundan çık
screen -ls               # Tüm screen oturumlarını listele

## Sorun Giderme
- Durdurmak ve yeniden başlatmak için `CTRL + C` kullanın
- Düzenli güncelleme: `git fetch origin && git reset --hard origin/main`

## BLOCKASSIST KURULUM (kurulum.md)
# BlockAssist Düğüm Kurulum Rehberi

WSL üzerinde GUI desteği ile çalışan Minecraft AI asistan düğümü.

## Ön Gereksinimler
- WSL2 ile Windows 10/11
- WSL'de Ubuntu
- VcXsrv X Sunucusu (Windows için)
- Python 3.10+

## Kurulum Adımları

### 1. VcXsrv Kurulumu
[sourceforge.net/projects/vcxsrv](https://sourceforge.net/projects/vcxsrv/) adresinden indirin ve kurun

### 2. WSL Kurulumu
# Eski kurulumu kaldır
rm -rf ~/blockassist

# Repository'yi klonla
git clone https://github.com/gensyn-ai/blockassist.git
cd blockassist

# Kurulum scriptini çalıştır
./setup.sh

### 3. pyenv ve Python 3.10 Kurulumu
curl -fsSL https://pyenv.run | bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
source ~/.bashrc

# Bağımlılıkları ve Python'u yükle
sudo apt update
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

pyenv install 3.10.0
pyenv global 3.10.0

### 4. Node.js ve Yarn Kurulumu
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
nvm install --lts
nvm alias default 'lts/*'
nvm use default

corepack enable
corepack prepare yarn@stable --activate

### 5. Poetry ve Bağımlılıkları Yükleyin
curl -sSL https://install.python-poetry.org | python3 -
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

pip install psutil readchar rich

### 6. X11 Display Ayarları
# X11 araçlarını yükle
sudo apt-get update -y
sudo apt-get install -y x11-apps x11-xserver-utils

# Display ayarı (Windows IP'niz ile değiştirin)
export DISPLAY=192.168.1.10:0

# X11 testi
xeyes

### 7. OpenGL Ortam Ayarları
export LIBGL_ALWAYS_SOFTWARE=1
export MESA_LOADER_DRIVER_OVERRIDE=llvmpipe
export LIBGL_ALWAYS_INDIRECT=1
export MESA_GL_VERSION_OVERRIDE=2.1
export _JAVA_OPTIONS='-Xms512m -Xmx2g -Dorg.lwjgl.opengl.Display.allowSoftwareOpenGL=true'

### 8. Giriş Yapın ve BlockAssist'i Çalıştırın
cd modal-login
yarn install
yarn dev

Giriş yapmak için `http://localhost:3000` adresine erişin, ardından:
cd ~/blockassist
python3 -m venv blockassist-venv
source blockassist-venv/bin/activate
pip install -e .
python3 run.py

## Günlük Başlatma
cd blockassist
source blockassist-venv/bin/activate
python3 run.py

## CODEASSIST KURULUM (kurulum.md)
# CodeAssist Kurulum Rehberi

Docker ve Ollama kullanan tamamen yerel AI kodlama asistanı.

## Sistem Gereksinimleri
- Minimum 8GB RAM (16GB önerilir)
- 10-20GB boş disk alanı
- Ubuntu 20.04/22.04 veya WSL2 ile Windows
- Docker desteği

## Kurulum Adımları

### 1. Sistem Güncellemesi
sudo apt update && sudo apt upgrade -y

### 2. Docker Kurulumu
sudo apt install docker.io -y
sudo systemctl enable docker
sudo service docker start
docker --version

### 3. Python ve PIP Kurulumu
sudo apt install python3 python3-pip -y
python3 --version
pip3 --version

### 4. UV Paket Yöneticisi Kurulumu
curl -LsSf https://astral.sh/uv/install.sh | sh
uv --version

### 5. CodeAssist'i Klonlayın ve Kurun
git clone https://github.com/gensyn-ai/codeassist.git
cd codeassist

### 6. CodeAssist'i Çalıştırın
uv run run.py

### 7. HuggingFace Token
İstendiğinde, kuruluma devam etmek için HuggingFace token'ınızı girin.

## Özellikler
- ✅ Kod yazma
- ✅ Hata ayıklama
- ✅ Dosya oluşturma
- ✅ Kodlama sorularını yanıtlama
- ✅ Uygulama & script oluşturma
- ✅ Tamamen özel ve yerel
- ✅ Hafif ve hızlı

## Kullanım
Kurulumdan sonra, CodeAssist tamamen yerel olarak Docker konteynırları ve Ollama modelleri kullanarak çalışır. Normal operasyon için internet bağlantısı gerekmez.

## Sorun Giderme
- Docker'ın çalıştığından emin olun: `sudo service docker status`
- Disk alanını kontrol edin: `df -h`
- Python versiyonunu doğrulayın: `python3 --version`
