# oai-5g-test
Repositório teste para implementação da arquitetura simulada de NR5G, com CN, gNB e UE, com RFSimulator.

## Como executar no Terminal:

### Passo 1: Aplicar a regra de firewall e iniciar o Core Network

``` 
sudo iptables-legacy -P FORWARD ACCEPT
sudo iptables -P FORWARD ACCEPT

cd /workspaces/oai-5g-test/oai-cn5g
docker compose down
docker compose up -d
``` 

### Verifique se todos os contêineres estão com status (healthy):

```
docker ps
```

### Passo 2: Executar a gNB (em um terminal)

```
cd /workspaces/oai-5g-test/openairinterface5g/cmake_targets/ran_build/build

sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf --gNBs.[0].min_rxtxtime 6 --rfsim --continuous-tx
```

#### (Aguarde até exibir [PHY] Command line parameters for OAI UE: ... e a conexão no log do AMF).

### Passo 3: Executar o nrUE (em outro terminal)

```
cd /workspaces/oai-5g-test/openairinterface5g/cmake_targets/ran_build/build

sudo ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 --ssb 516 --uicc0.imsi 001010000000001 --rfsim --rfsimulator.serveraddr 127.0.0.1
```
