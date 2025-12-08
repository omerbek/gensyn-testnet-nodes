### Gensyn Proje İzleme

Bu depo, **rl-swarm** projesini bir **gensyn screen** oturumu içinde çalıştırırken izleyen bir monitörleme scriptini içerir. Script, proje çıktı üretmeyi durdurursa bunu otomatik olarak algılar ve yeniden başlatır.

---

### 1. Başlangıç Kurulumu

Sisteminizde **screen** ve **bash** yüklü olduğundan emin olun.  
Depoyu klonlayın (veya dosyaları indirin) ve proje klasörüne geçin.

`rl-swarm` projesini **gensyn** adlı bir screen oturumunda başlatmak için: **Eğer başka bir screen içinde çalışıyorsa Ctrl+C ile durdurun ve o screeni kapatın.**
```
screen -S gensyn -dm bash -c 'printf "n\n\n" | bash run_rl_swarm.sh'
```

---

### 2. Monitör Scripti Oluşturma

Yeni bir terminal açın.

Nano ile monitör scriptini oluşturun:
```
nano monitor_gensyn.sh
```

`monitor_gensyn.sh` dosyasına aşağıdaki içeriği yapıştırın:
```
#!/bin/bash

###############################################
# Single Screen Auto-Monitoring Script
# Universal prompt detection + auto restart
# Works on all systems (GitHub safe)
###############################################

# === CONFIGURATION ===
SCREEN_NAME="gen"     # takip edilecek screen adı

# Evrensel shell prompt regex:
# Virtualenv olabilir / olmayabilir
# user@host:pwd şeklindeki tüm prompt'ları yakalar
PROMPT_REGEX="^\(.*\)\? *[A-Za-z0-9._-]\+@[A-Za-z0-9._-]\+:.*[#$]$"

# Log dosyaları
OLD="/tmp/${SCREEN_NAME}_last.log"
NEW="/tmp/${SCREEN_NAME}_new.log"

# Değişmeme sayacı (2 olursa kesin restart)
NOCHANGE=0

echo "Taking initial snapshot for screen '$SCREEN_NAME'..."
screen -S "$SCREEN_NAME" -X hardcopy "$OLD"

echo "Initial snapshot saved. Waiting 180 seconds before starting monitor..."
sleep 180


###############################################
# MAIN MONITORING LOOP
###############################################

while true; do

    # Yeni log al
    screen -S "$SCREEN_NAME" -X hardcopy "$NEW"

    ###############################################
    # 1) Prompt Görüldüyse — Kod Durmuştur → Restart
    ###############################################
    if grep -qE "$PROMPT_REGEX" "$NEW" 2>/dev/null; then
        echo "Prompt detected — restarting '$SCREEN_NAME' immediately!"
        
        screen -S "$SCREEN_NAME" -X stuff $'\003'   # Ctrl+C
        sleep 1
        screen -S "$SCREEN_NAME" -X stuff $'\e[A'   # Up Arrow
        sleep 1
        screen -S "$SCREEN_NAME" -X stuff $'\n'     # Enter
        sleep 1

        mv "$NEW" "$OLD"
        NOCHANGE=0
        continue
    fi


    ###############################################
    # 2) Log Hiç Değişmemişse
    ###############################################
    if cmp -s "$OLD" "$NEW"; then
        NOCHANGE=$((NOCHANGE + 1))
        echo "No change detected ($NOCHANGE cycle)."

        # 2 döngü üst üste durduysa → GPU aktif olsa bile restart
        if [ "$NOCHANGE" -ge 2 ]; then
            echo "Screen '$SCREEN_NAME' is stalled for 2 cycles — forcing restart!"
            
            screen -S "$SCREEN_NAME" -X stuff $'\003'
            sleep 1
            screen -S "$SCREEN_NAME" -X stuff $'\e[A'
            sleep 1
            screen -S "$SCREEN_NAME" -X stuff $'\n'
            sleep 1

            NOCHANGE=0
        else
            # Sadece bilgi amaçlı GPU python process sayısı
            PYTHON_COUNT=$(nvidia-smi --query-compute-apps=pid,process_name --format=csv,noheader | grep -c python)
            echo "First stall detected. Python GPU processes running: $PYTHON_COUNT"
        fi
    else
        echo "Activity detected — '$SCREEN_NAME' is running."
        NOCHANGE=0
    fi


    # log’u güncelle
    mv "$NEW" "$OLD"

    # 120 saniye sonra yeniden kontrol
    sleep 120
done

```

**Nano’da kaydedip çıkın (Ctrl+O, Enter, Ctrl+X).**

**Scripti çalıştırılabilir yapın:**
```
chmod +x monitor_gensyn.sh
```

---

### 3. Monitörü Çalıştırma

Yeni bir screen oturumu açın, adı **gencheck** olsun:
```
screen -S gencheck -dm bash -c './monitor_gensyn.sh'
```

Monitör çıktısını görmek için **gencheck** screen oturumuna bağlanın:
```
screen -r gencheck
```

Screen’den çıkarken monitörü durdurmak istemiyorsanız:
```
Ctrl+A ardından D
```
