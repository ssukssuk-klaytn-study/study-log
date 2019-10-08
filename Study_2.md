# Klaytn Study 2주차 (2019.09.24)
- 로컬에서 테스트넷 Endpoint Node 실행하기 (Mac)
- console로 계정 생성 및 faucet 으로 테스트용 토큰 받기

### 로컬에서 테스트넷 Endpoint Node 실행하기 (Mac)

1. 다운로드

    `https://docs.klaytn.com/node/endpoint-node/installation-guide/download` 
    
    여기서 각자의 OS에 따라 받습니다.
    
2. 압축풀기 및 폴더생성

    위에서 받은 파일이 다운로드된 폴더로 이동해서

    `tar zxf ken-baobab-v1.1.1-3-darwin-amd64.tar.gz`

    명령어로 압축을 풉니다.

    또한 'mkdir -p ~/kend_home 을 통해서 홈디렉토리에 `kend_home`이라는 폴더를 생성해줍니다.

이 폴더는 블록체인 데이터가 저장될 폴더입니다.

3. 환경변수 등록하기

    PATH에 Klaytn Node경로를 넣어줘야합니다.

    각자 shell에 맞게 .zshrc 또는 .bashrc 등에 아래 내용을 추가해줍니다.

    ```
    export PATH=$PATH:"(압축푼 폴더경로)/bin"
    ```
    source .zshrc 등을 통해 적용시켜준후

    terminal에 `kend`를 쳤을떄 

    `Usages: kend {start|stop|restart|status}`

    이렇게 나오면 환경변수 등록이 완료된것입니다.

4. 설정파일 수정하기

    아까 압축 풀어서 생긴 ken-darwin-amd64 폴더 하위에 있는 conf라는 폴더 밑에 kend.conf라는 configuration 파일이 있는데 

    이 파일 내용중 DATA_DIR값을 ~/kend_home으로 설정해줍니다. 

    그외의 설정값들에 대한 정보는 

    `https://docs.klaytn.com/node/endpoint-node/operation-guide/configuration` 여기서 보실수 있습니다.

5. 노드 실행하기

    `kend_start`를 치면 

    `Starting kend: OK` 이렇게 뜨면 정상적으로 노드가 실행된것입니다.

    `tail -f ~/kend_home/logs/kend.out`  이렇게 치면 node의 로그를 볼수있습니다.


### ken console

  이더리움의 geth 처럼 klaytn도 javascript기반 콘솔을 제공하는데

  `ken attach ~/kend_home/klay.ipc` 이 명령어를 통해 klaytn javascript console에 접속할수있습니다.

  ```
  instance: Klaytn/v1.1.1/darwin-amd64/go1.12.5
  datadir: /Users/ms/kend_home
  modules: admin:1.0 debug:1.0 governance:1.0 istanbul:1.0 klay:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0
  ```
  이렇게 뜨면 접속된것입니다.

### Account 생성하기

  console을 통해 account를 생성해보겠습니다.

  `personal.newAccount()`를 치고 passphrase를 입력하면 계정이 생성되고 0x로 시작하는 20byte짜리 주소가 나옵니다.

  위에서 생성한 계정을 사용하려면 unlock을 해줘야합니다.

  `personal.unlockAccount('주소', 'passphrase', 300)`

  `klay.getBalance('주소')` 이렇게 해당 주소의 잔액을 가져올수있는데 현재는 0으로 뜰것입니다.

  밑에서 테스트용 토큰을 받아와보겠습니다.

### faucet을 통해 klay 충전하기

  `https://baobab.wallet.klaytn.com/faucet/` 을 들어가면 테스트넷용 klay 토큰을 받을수있습니다..

  테스트넷용 klay 토큰이기 떄문에 실제 가치를 갖고있지는 않습니다.

  위 링크로 접속후 Sign-in Useing Keystore File 탭을 클릭합니다.

  keystore파일은 kend_home/keystore에 있는 utc-로 시작하는 파일을 넣어주면 됩니다. 이파일은 console에서 `personal.newAccount를 통해 계정을 생성할떄 생긴 파일입니다.

  비밀번호는 account 생성할때 입력한 passphrase 입니다..

  그 후 체크박스를 클릭후 access 를 누르면 뜨는 화면에서 run Faucet 버튼을 클릭하면 5 klay를 받을수있습니다.

### 잔액 확인하기

  console에서 

  `klay.getBalance('주소')` 하면

  `5000000000000000000` 이렇게 나타나면 충전이 완료된것입니다. 

  위 잔액은 peb단위(https://docs.klaytn.com/klaytn/design/klaytn-native-coin-klay#units-of-klay)이므로 klay로 보려면 

  `klay.fromPeb(klay.getBalance('주소'), 'KLAY')`

  위 처럼 console에 치면 5로 보일것입니다.




















