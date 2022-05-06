# Kyve Node Kurulumu

### Sistem Gereksinimi 

- 1vCPU
- 4GB RAM
- 1GB DISK
- Ubuntu 20.04

_Yukarıda belirtilen sistem gereksinimlerini karşılayacak bir vps üzerinden node kurulumlarını gerçekleştirebilirsiniz._

### Kyve Testnet Sayfası;
https://app.kyve.network/

### Keplr Wallet
https://wallet.keplr.app/

### Arweave dosyasını indirmek için kullanılan site;
https://faucet.arweave.net/

- İndirilen dosyanın adını 'arweave.json' yapmalısınız.

- İndirilen dosyası /root/kyve/ klasörüne winscp veya filezilla aracılığı ile yüklemelisiniz. 
``` 
sudo apt update 
sudapt install unzip
mkdir -p /root/kyve
.....
```
- Aşağıdaki kodları yazarken bu mkdir -p /root/kyve aşamasından sonra dosya yüklemesini gerçekleştirin

### 

### Ayrı Ayrı Yazılacak Kodlar
``` 
sudo apt update 
sudapt install unzip
mkdir -p /root/kyve
cd /root/kyve && wget https://github.com/kyve-org/near/releases/download/v0.0.0/kyve-near-linux.zip && jar xvf kyve-near-linux.zip
chmod +x kyve-near-linux
cd /root/kyve && ./kyve-near-linux --version
```

### Bu aşamada mnmonic yazan yere cüzdanı oluştururken verilen 12 adet kelimeyi yazacaksınız
```
sudo tee /etc/systemd/system/kyved.service > /dev/null <<EOF
[Unit]
Description=Kyve Node
After=network-online.target
[Service]
User=root
WorkingDirectory=/root/kyve/
ExecStart=/root/kyve/kyve-near-linux -m "mnmonic" -k /root/kyve/arweave.json -p 6 -v -s 200
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

- -v -s '200' kısmındaki 200 sayısı stake edeceğiniz token sayısını gösterir, elinizdeki tokenlerin %99'unu stake etmeniz önerilir.

### Düğümü Başlatma Kodları

```
sudo systemctl daemon-reload
sudo systemctl enable kyved
sudo systemctl start kyved
```

### Logları Görüntüleme
```
journalctl -f -u kyved
```
