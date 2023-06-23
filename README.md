Допустим нода defund. Сделаем резервные копии файлов.<br>

```
noda=".defund" - здесь нужно поставить папку ноды
cp /root/$noda/config/config.toml /root/$noda/config/myconfig.toml
cp /root/$noda/config/app.toml /root/$noda/config/myapp.toml
cp /root/$noda/config/client.toml /root/$noda/config/myclient.toml
```
noda2 - для второй ноды
```
proxy_app1=36658
rpc_laddr1=36657
pprof_laddr1=6061
p2p_laddr1=36656
prometheus_listen_addr1=36660
api_address1=1327
web_address1=9190
grpc_address1=9191
```
для 3 ноды
```
proxy_app1=46658
rpc_laddr1=46657
pprof_laddr1=6062
p2p_laddr1=46656
prometheus_listen_addr1=46660
api_address1=1337
web_address1=9290
grpc_address1=9291
```

Для 4 ноды
```
proxy_app1=56658
rpc_laddr1=56657
pprof_laddr1=6063
p2p_laddr1=56656
prometheus_listen_addr1=56660
api_address1=1347
web_address1=9390
grpc_address1=9391
```

Для 5 ноды
```
proxy_app1=60558
rpc_laddr1=60557
pprof_laddr1=6064
p2p_laddr1=60556
prometheus_listen_addr1=60560
api_address1=1357
web_address1=9490
grpc_address1=9491
```

для 6 ноды
```
proxy_app1=60658
rpc_laddr1=60657
pprof_laddr1=6065
p2p_laddr1=60656
prometheus_listen_addr1=60660
api_address1=1367
web_address1=9590
grpc_address1=9591
```
Для 7 ноды
```
proxy_app1=60758
rpc_laddr1=60757
pprof_laddr1=6066
p2p_laddr1=60756
prometheus_listen_addr1=60760
api_address1=1377
web_address1=9690
grpc_address1=9691
```
В config.toml сначала меняем порты:
#proxy_app = “tcp://127.0.0.1:26658”
#rpc laddr = “tcp://127.0.0.1:26657”
#pprof_laddr = “localhost:6060”
#p2p laddr = “tcp://0.0.0.0:26656”
#prometheus_listen_addr = “:26660”

Вводим:
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:$proxy_app1\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:$rpc_laddr1\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:$pprof_laddr1\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:$p2p_laddr1\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":$prometheus_listen_addr1\"%" $HOME/$noda/config/config.toml
```

Теперь изменим app.toml
#api address = “tcp://0.0.0.0:1317”
#grpc address = “0.0.0.0:9090”
#grpc-web address = “0.0.0.0:9091”

```
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:$web_address1\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:$grpc_address1\"%; s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:$api_address1\"%" $HOME/$noda/config/app.toml
```

# client.toml
#node = “tcp://localhost:26657” 36657

```
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%node = \"tcp://localhost:$rpc_laddr1\"%" $HOME/$noda/config/client.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:$p2p_laddr1\"/" $HOME/$noda/config/config.toml
```

sudo systemctl restart defundd && sudo journalctl -u defundd -f -o cat
defundd status 2>&1 | jq
