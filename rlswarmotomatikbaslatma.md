### Gensyn Proje İzleme

Bu depo, **rl-swarm** projesini bir **gensyn screen** oturumu içinde çalıştırırken izleyen bir monitörleme scriptini içerir. Script, proje çıktı üretmeyi durdurursa bunu otomatik olarak algılar ve yeniden başlatır.

---

### 1. Başlangıç Kurulumu

Sisteminizde **screen** ve **bash** yüklü olduğundan emin olun.  
Depoyu klonlayın (veya dosyaları indirin) ve proje klasörüne geçin.

`rl-swarm` projesini **gensyn** adlı bir screen oturumunda başlatmak için:
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
# === Gensyn Monitör Scripti ===
# 'gensyn' screen oturumunu izler ve çıktı yoksa yeniden başlatır.

LOG_TMP="/tmp/gensyn_last.log" # Screen çıktısının önceki anlık görüntüsünü tutar
SCN="gensyn" # Projeyi çalıştıran screen oturumunun adı

# Screen çıktısının ilk anlık görüntüsünü al
screen -S "$SCN" -X hardcopy "$LOG_TMP"

echo "Gensyn monitörü başlatılıyor... Her 60 saniyede aktivite kontrol edilecek."

while true; do
sleep 60 # 60 saniye bekle

# Screen çıktısının yeni anlık görüntüsünü al
NEW_TMP="/tmp/gensyn_new.log"
screen -S "$SCN" -X hardcopy "$NEW_TMP"

# Önceki anlık görüntü ile yeni görüntüyü karşılaştır
if cmp -s "$LOG_TMP" "$NEW_TMP"; then
# Değişiklik yoksa, proje takıldı. Yeniden başlat.
echo "Son bir dakikada çıktı tespit edilmedi. Proje yeniden başlatılıyor..."
screen -S "$SCN" -X stuff $'\003' # Ctrl+C göndererek mevcut süreci durdur
sleep 1
screen -S "$SCN" -X stuff $'\e[A' # Önceki komutu çağırmak için Up Arrow
sleep 1
screen -S "$SCN" -X stuff $'\n' # Enter ile tekrar çalıştır
sleep 1
else
# Çıktı değiştiyse, proje normal çalışıyor
echo "Aktivite tespit edildi. Proje normal çalışıyor."
fi

# Son anlık görüntü dosyasını bir sonraki kontrol için güncelle
mv "$NEW_TMP" "$LOG_TMP"
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
