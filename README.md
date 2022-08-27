<p style="font-size:14px" align="right">
<a href="https://t.me/bangpateng_group" target="_blank">Join our telegram <img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/></a>
<a href="https://bangpateng.com/" target="_blank">Visit our website <img src="https://user-images.githubusercontent.com/38981255/184068977-2d456b1a-9b50-4b75-a0a7-4909a7c78991.png" width="30"/></a>
</p>

<p align="center">
  <img height="100" height="auto" src="https://user-images.githubusercontent.com/38981255/187036471-e23ab080-2e03-46b7-8513-23e1f6612b4a.png">
</p>

## Specification VPS

Untuk menjalankan node testnet, Anda memerlukan mesin dengan persyaratan perangkat keras minimum berikut:

4 atau lebih inti CPU fisik
Setidaknya 500GB penyimpanan disk SSD
Memori (RAM) minimal 32GB
Setidaknya bandwidth jaringan 100mbps

## Haqq Testnet

### Install Otomatis
```
wget -O haqq.sh https://raw.githubusercontent.com/bangpateng/haqq/main/haqq.sh && chmod +x haqq.sh && ./haqq.sh
```

## Muat Variable
```
source $HOME/.bash_profile
```
Selanjutnya Anda harus memastikan validator Anda menyinkronkan blok. Anda dapat menggunakan perintah di bawah ini untuk memeriksa status sinkronisasi
```
haqqd status 2>&1 | jq .SyncInfo
```

## Gunakan Snpashoot (Jika Log Belun Sinkron)

Instal lz4 jika diperlukan
```
apt-get install lz4
```
Unduh Snapshootnya:
```
curl -OL https://storage.googleapis.com/haqq-testedge-snapshots/haqq_latest.tar.lz4
```
Dekompresi snapshot ke lokasi database Anda. Lokasi database Anda akan mempengaruhi ~/.haqqd tergantung pada implementasi node Anda.
```
lz4 -c -d haqq_latest.tar.lz4 | tar -x -C $HOME/.haqqd
```

## Buat dompet

Untuk membuat dompet baru Anda dapat menggunakan perintah di bawah ini. Jangan lupa simpan mnemonicnya
```
haqqd keys add $WALLET
```
(OPSIONAL) Untuk memulihkan dompet Anda menggunakan frase seed
```
haqqd keys add $WALLET --recover
```
Untuk mendapatkan daftar dompet saat ini
```
haqqd keys list
```
## Simpan info dompet

Tambahkan dompet dan alamat valoper ke dalam variabel
```
HAQQ_WALLET_ADDRESS=$(haqqd keys show $WALLET -a)
HAQQ_VALOPER_ADDRESS=$(haqqd keys show $WALLET --bech val -a)
echo 'export HAQQ_WALLET_ADDRESS='${HAQQ_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export HAQQ_VALOPER_ADDRESS='${HAQQ_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```
## Minta token Testnet

Setelah Anda masuk ke ekstensi MetaMask, kunjungi Faucet : https://testedge.haqq.network/

membuka jendela baru untuk meminta token untuk testnet. Klik Connect Wallet Via Metamask
