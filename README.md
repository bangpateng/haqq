<p style="font-size:14px" align="right">
<a href="https://t.me/bangpateng_group" target="_blank">Join our telegram <img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/></a>
<a href="https://bangpateng.com/" target="_blank">Visit our website <img src="https://user-images.githubusercontent.com/38981255/184068977-2d456b1a-9b50-4b75-a0a7-4909a7c78991.png" width="30"/></a>
</p>

<p align="center">
  <img height="100" height="auto" src="https://user-images.githubusercontent.com/38981255/187036471-e23ab080-2e03-46b7-8513-23e1f6612b4a.png">
</p>

# HAQQ TESTNET

## Tutorial Official

https://docs.haqq.network/guides/validators/setup.html

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
sudo journalctl -u haqqd -f -o cat
```
Biarkan Hingga Beberapa Menit, Sampai Log Nya Jalan dan Mendapatkan Block Height

```
haqqd status 2>&1 | jq .SyncInfo
```

## Buat dompet

Untuk membuat dompet baru Anda dapat menggunakan perintah di bawah ini. Jangan lupa simpan mnemonicnya
dan Untuk Keyring Pharse Buat Kata Sandi Bebas
```
haqqd keys add $WALLET --keyring-backend file
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

Jalankan Perintah Untuk Mendapatkan Private Key
```
haqqd keys unsafe-export-eth-key $WALLET --keyring-backend file
```
- Jangan Lupa Masukan Password Keyring Yang Kalian Buat Saat membuat Wallet di Awal dan Import Privatekey ke Metamask
- Kirim Saldo Faucet Yg ada di Wallet pertama pas claim Faucet kirim ke Address 0x.. Yang Private Keynya Baru Kalian Import

## Buat Validator

Sebelum Membuat Validator Silahkan Check Dulu Asset Saldo Kalian Ada Atau Tidak
```
haqqd query bank balances $HAQQ_WALLET_ADDRESS
```
```
haqqd tx staking create-validator \
  --amount 1000000aISLM \
  --pubkey $(haqqd tendermint show-validator) \
  --moniker $NODENAME \
  --chain-id $HAQQ_CHAIN_ID \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1000000" \
  --gas-prices="0.025aISLM" \
  --from $WALLET \
  --node https://rpc.tm.testedge.haqq.network:443
  --keyring-backend file
```
## Edit Validator

```
haqqd tx staking edit-validator \
 --chain-id $HAQQ_CHAIN_ID \
 --identity="EB7784D8888B8552" \
 --details="Ente Emang Kadang Kadang Ente" \
 --website="www.bangpateng.com" \
 --from $WALLET \
 --fees 0.025aISLM \
 --keyring-backend file
  ```

## Nambah Delegate

Ingat 1000000 aISLM = 1 ISLM
```
haqqd tx staking delegate YOUR_VALOPER_ADDRESS 10000000aISLM --from=$WALLET --chain-id $HAQQ_CHAIN_ID --gas-prices=0.025aISLM --keyring-backend file
```
### Cara Mengetahui Valoper Address

- Check di Web : https://exp.nodeist.net/
- Cari Nama Validator Kalian
- Copy Link Address Paling Belakang, Itu Address Valoper Kalian


## Delet Node
```
sudo systemctl stop haqqd
sudo systemctl disable haqqd
sudo rm /etc/systemd/system/haqq* -rf
sudo rm $(which haqqd) -rf
sudo rm $HOME/.haqqd* -rf
sudo rm $HOME/haqq -rf
sed -i '/HAQQ_/d' ~/.bash_profile
```
