# Klaytn Study 3주차(2019.10.01)
- helloworld contract 작성 및 배포

### truffle 설치하기

트러플은 스마트컨트랙트 작성을 도와주는 프레임워크이다

https://github.com/trufflesuite/truffle

Klaytn에서 사용하려면 4버전을 사용해야한다

`yarn global add truffle@4.1.16` 을 통해 truffle을 설치해준다.

### Init Truffle Project

`mkdir hello-contract && cd hello-contract`

을 통해 작업할 폴더를 생성해주고

`truffle init` 명령어를 통해 프로젝트를 초기화 해준다.

위 명령어를 치면 `contracts, migrations, test` 폴더 3개와 `truffle-config.js`이라는 파일이 생성된다.

`contracts` 폴더는 solidity contract 소스 폴더

`migrations` 폴더는 컨트랙트를 배포할때 사용되는 js 파일 폴더

`test`는 테스트 코드등 테스트 관련 폴더이다


### HelloWorld

HelloWorld.sol 파일을 contracts 폴더에 만들어주고 아래 내용을 입력한다.

```
pragma solidity ^0.4.23;

contract HelloWorld {
     string public str;

     constructor() public {
              str = "Hello, World";
     }

     function setName(string _str) public {
             str = _str;
     }

     function run() public view returns(string) {
             return str;
     }
}
````

##  migrations

우리가 위에서 작성한 solidity 파일을 import해줘야한다.

migrations 폴더 안의 1_initial_migration.js 파일 내용을

```
const Migrations = artifacts.require("./Migrations.sol");
const HelloWorld = artifacts.require("./HelloWorld.sol");
module.exports = function(deployer) {
  deployer.deploy(Migrations);
  deployer.deploy(HelloWorld);
};
```

이렇게 수정한다.


### config

truffle-config.js에 아래내용을 입력해준다.

```
// truffle-config.js
module.exports = {
  networks: {
      klaytn: {
          host: '127.0.0.1',
          port: 8551,
          from: (STUDY_2에서 생성한 계정주소), // enter your account address
          network_id: '1001', // Baobab network id
          gas: 20000000, // transaction gas limit
          gasPrice: 25000000000, // gasPrice of Baobab is 25 Gpeb
      },
  },
  compilers: {
    solc: {
      version: "0.5.6"    // Specify compiler's version to 0.5.6
    }
}
};
```

현재 로컬에서 실행준인 endpoint node를 통해서 컨트랙트를 배포할것이다

### 배포하기

`truffle deploy --network klaytn --reset` 명령어를 터미널에서 입력한다

```
➜  hello-contract truffle deploy --network klaytn --reset
Using network 'klaytn'.

Running migration: 1_initial_migration.js
Error encountered, bailing. Network state unknown. Review successful transactions manually.
Error: The method net_version does not exist/is not available
    at Object.InvalidResponse (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/web3/lib/web3/errors.js:38:1)
    at /Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/web3/lib/web3/requestmanager.js:86:1
    at /Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/packages/truffle-migrate/index.js:225:1
    at /Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/packages/truffle-provider/wrapper.js:134:1
    at XMLHttpRequest.request.onreadystatechange (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/web3/lib/web3/httpprovider.js:128:1)
    at XMLHttpRequestEventTarget.dispatchEvent (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/xhr2/lib/xhr2.js:64:1)
    at XMLHttpRequest._setReadyState (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/xhr2/lib/xhr2.js:354:1)
    at XMLHttpRequest._onHttpResponseEnd (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/xhr2/lib/xhr2.js:509:1)
    at IncomingMessage.<anonymous> (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/xhr2/lib/xhr2.js:469:1)
    at IncomingMessage.emit (events.js:203:15)
    at endReadableNT (_stream_readable.js:1145:12)
    at process._tickCallback (internal/process/next_tick.js:63:19)
```

맨 처음에 진행했을 때 이런 에러가 나왔었는데

이 에러는 `kend.conf`를 수정해 줘야한다. 

`kend.conf` 내용중 
`RPC_API="klay" # available apis: admin,debug,klay,miner,net,personal,rpc,txpool,web3` 이렇게 작성된 부분이 있는데 이부분을

`RPC_API="admin,debug,klay,miner,net,personal,rpc,txpool,web3" ` 이렇게 변경해주고 kend restart 명령어로 node를 재시작 해준다.

그런다음 다시 `truffle deploy --network klaytn --reset` 명령어를 터미널에서 입력한다 그러면 

```
➜  hello-contract truffle deploy --network klaytn --reset
Using network 'klaytn'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... undefined
Error encountered, bailing. Network state unknown. Review successful transactions manually.
Error: authentication needed: password or unlock
    at Object.InvalidResponse (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/web3/lib/web3/errors.js:38:1)
    at /Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/web3/lib/web3/requestmanager.js:86:1
    at /Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/packages/truffle-migrate/index.js:225:1
    at /Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/packages/truffle-provider/wrapper.js:134:1
    at XMLHttpRequest.request.onreadystatechange (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/web3/lib/web3/httpprovider.js:128:1)
    at XMLHttpRequestEventTarget.dispatchEvent (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/xhr2/lib/xhr2.js:64:1)
    at XMLHttpRequest._setReadyState (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/xhr2/lib/xhr2.js:354:1)
    at XMLHttpRequest._onHttpResponseEnd (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/xhr2/lib/xhr2.js:509:1)
    at IncomingMessage.<anonymous> (/Users/ms/.config/yarn/global/node_modules/truffle/build/webpack:/~/xhr2/lib/xhr2.js:469:1)
    at IncomingMessage.emit (events.js:203:15)
    at endReadableNT (_stream_readable.js:1145:12)
    at process._tickCallback (internal/process/next_tick.js:63:19)
```

이렇게 에러가 나오는데 이 에러는 현재 계정이 lock되어 있기 떄문이다. Study_2 에서 진행한 것처럼   `personal.unlockAccount('주소', 'passphrase', 300)` 로 계정을 unlock해준다.

계정을 unlock후 다시 `truffle deploy --network klaytn --reset` 명령어를 터미널에서 입력한다

```
Using network 'klaytn'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0xb6fe9a4682c77a8e7ca9071fab55a7e7907ef20b4752472e5580f119bc4cbf6c
  Migrations: 0x473bad8d09f07c7bab1b6dc4cbdeb892f3640250
  Deploying HelloWorld...
  ... 0xf9da0c3db1219cf57b8c5007361e01da94f2c428c9109927f9978746816c7c08
  HelloWorld: 0x8bab93d06bfcf3be8197a8e6bf74cac36417538b
Saving successful migration to network...
  ... 0x5b8c6b2b68f
```

그러면 성공적으로 migrate되었다고 나오고 contract 주소는 '0x8bab93d06bfcf3be8197a8e6bf74cac36417538b'로 나온다.