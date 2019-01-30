# VM(debian 9) 생성

```sh
# 리소스 그룹 만들기
az group create --name {group_name} --location KoreaCentral

# VM(debian 9) 만들기
az vm create --resource-group {group_name} --name {VM_name} --image credativ:Debian:9:latest  --admin-username {User_name} --admin-password P{Password} --size Standard_DS1_v2

```



# Edge 생성

```sh
# IoT 기능 확장
az extension add --name azure-cli-iot-ext

# IoT Hub 만들기
az iot hub create --resource-group {group_name} --name {hub_name} --sku S1

# IoT Edge 만들기
az iot hub device-identity create --hub-name {hub_name} --device-id {edge_name} --edge-enabled

#IoT Edge 연결 문자열 출력
az iot hub device-identity show-connection-string --device-id {edge_name} --hub-name {hub_name}

```



# Run-Time 설치

```sh
# curl, https 설치
sudo apt-get -y install apt-transport-https curl
	
# debian 리포지토리 구성
curl https://packages.microsoft.com/config/debian/9/prod.list > ./microsoft-prod.list

sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# 리포지토리 액세스 공개키 설치
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg

sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/

# 업데이트
sudo apt-get update

# Moby 설치에 필요한 파일 설치
wget "http://ftp.se.debian.org/debian/pool/main/o/openssl/libssl1.0.0_1.0.2l-1~bpo8+1_amd64.deb"

sudo dpkg -i libssl1.0.0_1.0.2l-1~bpo8+1_amd64.deb

# Moby 설치
sudo apt-get -y install moby-engine

# Moby용 CLI 명령 설치
sudo apt-get -y install moby-cli

```



# Deamon 설치

```sh
# Edge Deamon 설치
sudo apt-get update

sudo apt-get -y install iotedge

# 연결 문자열 Connect
sudo nano /etc/iotedge/config.yaml

# 재시작으로 적용
sudo systemctl restart iotedge

```



# 상태 확인

```sh
# Edge 실행 여부 확인
sudo systemctl status iotedge

# Module 실행 여부 확인
sudo iotedge list

# 로그 확인
sudo iotedge logs {Module_name} -f

```

