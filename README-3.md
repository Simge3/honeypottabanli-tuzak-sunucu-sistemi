
Honeypot Tabanlı Tuzak Sunucu Sistemi

## Proje Özeti

CyberTrap, saldırgan davranışlarını gözlemlemek ve analiz etmek amacıyla oluşturulmuş düşük etkileşimli bir honeypot sistemidir. Bu sistem, gerçek sistemlere zarar gelmeden önce kötü niyetli aktiviteleri tespit etmeyi ve bu aktivitelerin kayıt altına alınarak incelenmesini amaçlamaktadır. CyberTrap, özellikle SSH ve HTTP servisleri üzerinden gelen saldırıları simüle ederek saldırganların davranışlarını analiz etme imkânı sunar.

## Projenin Amacı

- Saldırı türlerini ve saldırgan davranış kalıplarını analiz edebilmek
- Gerçek sistemlerin güvenliğini artırmak için önleyici bilgiler elde etmek
- Siber tehdit istihbaratı sağlamak amacıyla log kayıtları toplamak
- Eğitim ve araştırma amaçlı siber saldırı senaryoları oluşturmak

## Kullanılan Teknolojiler

| Bileşen              | Teknoloji                         |
|----------------------|----------------------------------|
| Programlama Dili     | Python 3.x                        |
| Honeypot Framework   | Cowrie (SSH/Telnet Honeypot)     |
| Sanallaştırma        | VirtualBox veya Docker           |
| Loglama              | JSON, TXT log dosyaları          |
| İşletim Sistemi      | Ubuntu Server 20.04              |

## Kurulum Adımları

### 1. Gereksinimler

- Python 3.8+
- Git
- Virtualenv (opsiyonel)
- Cowrie için gerekli bağımlılıklar: Twisted, OpenSSH keys, etc.

### 2. Kurulum

```bash
sudo apt update && sudo apt install git python3 python3-venv python3-pip -y
git clone https://github.com/cowrie/cowrie.git
cd cowrie
python3 -m venv cowrie-env
source cowrie-env/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

### 3. Yapılandırma

```bash
cp etc/cowrie.cfg.dist etc/cowrie.cfg
```

### 4. Başlatma

```bash
bin/cowrie start
```

### 5. Logların Görüntülenmesi

```bash
tail -f var/log/cowrie/cowrie.log
```

## Proje Mimarisi

```
+----------------------------+
|       Saldırgan           |
+------------+--------------+
             |
             ▼
+----------------------------+
|    Honeypot Sunucusu      |
|  (Cowrie ile SSH Simülasyonu) |
+----------------------------+
             |
             ▼
+----------------------------+
|      Log Toplama ve       |
|      Olay Kayıtları       |
+----------------------------+
```

## Özellikler

- SSH üzerinden parola deneme (brute force) saldırılarını algılama
- HTTP istekleri ve komut yürütme girişimlerini kaydetme
- Saldırgan IP adreslerinin tespiti ve sınıflandırılması
- Oturumların tam kaydı (zaman damgası, komut, IP vb.)

## Güvenlik Önlemleri

- Honeypot sistemi izole bir ağ üzerinde çalıştırılmalıdır.
- Gerçek sistemlerle bağlantı kurulmasına izin verilmemelidir.
- Sadece eğitim ve analiz amacıyla kullanılmalıdır.
- Uygulama varsayılan ağ portlarında değil, özel portlar üzerinden hizmet vermelidir.

## Geliştirme ve Genişletme

- Yüksek etkileşimli honeypot sistemlerine geçiş (örn. Dionaea, Conpot)
- Web arayüzü ile log analizi görselleştirme
- Gerçek zamanlı uyarı sistemi entegrasyonu (örn. Slack, Telegram bot)

## Örnek Python Honeypot Simülasyonu

```python
from http.server import BaseHTTPRequestHandler, HTTPServer
import logging

class HoneypotHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        logging.info(f"GET request from {self.client_address[0]} for {self.path}")
        self.send_response(200)
        self.send_header('Content-type', 'text/html')
        self.end_headers()
        self.wfile.write(b"This is a honeypot system. Your activity is being logged.")

def run(server_class=HTTPServer, handler_class=HoneypotHandler, port=8080):
    logging.basicConfig(filename='honeypot.log', level=logging.INFO)
    server_address = ('', port)
    httpd = server_class(server_address, handler_class)
    logging.info(f"Starting honeypot server on port {port}")
    httpd.serve_forever()

if __name__ == '__main__':
    run()
```

## Lisans

Bu proje MIT lisansı kapsamında sunulmuştur. Detaylar için LICENSE dosyasını inceleyiniz.

## Katkı Sağlayanlar

- Siber güvenlik topluluğu

