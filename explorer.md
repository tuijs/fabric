# hyperledger fabric explorer

```bash
# 启动网络
cd ~/fabric-samples/first-network
echo y| ./byfn.sh down
echo y| ./byfn.sh up -s couchdb

# 下载源码
cd ~
# git clone --depth=1 https://github.com/hyperledger/blockchain-explorer.git
wget -c http://ftp.x:12/weiyi/fabric/blockchain-explorer.tar.xz
tar -xvf blockchain-explorer.tar.xz
rm -f blockchain-explorer.tar.xz

# 查看网络管理员私钥
ls ~/fabric-samples/first-network/crypto-config/peerOrganizations/org1.example.com/users/Admin\@org1.example.com/msp/keystore/
'
7af8e6de5753be1d310ddb7c730a6d208c9bf62c8db9499aea55824e6eb3d409_sk
'

# 配置网络管理员私钥及 explorer 管理员账户密码
sudo yum install -y jq
cp -f ~/blockchain-explorer/examples/net1/connection-profile/first-network.json ~/blockchain-explorer/examples/net1/connection-profile/first-network.json.bak
cat ~/blockchain-explorer/examples/net1/connection-profile/first-network.json.bak | jq '  
.organizations.Org1MSP.adminPrivateKey.path="/tmp/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/keystore/7af8e6de5753be1d310ddb7c730a6d208c9bf62c8db9499aea55824e6eb3d409_sk"' | jq '.client.adminCredential.id="weiyi"' | jq '.client.adminCredential.password="Docker2019##"' > ~/blockchain-explorer/examples/net1/connection-profile/first-network.json

# 拷贝证书至 blockchain-explorer
cp -rf ~/fabric-samples/first-network/crypto-config/* ~/blockchain-explorer/examples/net1/crypto/

# 部署 explorer, 通过 http://<your ip>:8080 访问 explorer web 前端
cd ~/blockchain-explorer/
docker-compose up -d
```
