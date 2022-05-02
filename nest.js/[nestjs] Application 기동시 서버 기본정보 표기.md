# 서버 기본정보 표기
- 백엔드 개발 시 가장 먼저 작업하는 것은 Application 정보를 출력하는 것이다.
- Application 정보라고 하면 node version, cpu core, host name, host path, architecture, platform 등등등 Application 정보와 서버 정보이다.
- 관점에 따라 다를 수 있지만 Application 정보와 server 정보를 출력하는 것은 가장 중요하다고 생각한다.
- 그이유는 내가 env를 local인지 development, production환경에 맞게 설정 정보를 셋팅 했는지 확인 할 수 있고, cpu core 갯수에 따라 node instance를 셋팅 할 수 있기 때문이다.
- 그리고 또 가장 중요한 이유는 ...Spring에 대한 향수 이기 때문이다ㅎㅎㅎ

![](https://velog.velcdn.com/images/hong-brother/post/d900ba07-3766-4b5f-a207-1b404358994f/image.png)

- Spring으로 개발하던 시절 항상 Spring은 배너와 함께 출력이 되고 해서 뭔가 tomcat을 올리고 내리고 하는 맛이 있었는데.... node는 기동 속도가 빠르기도 하고 spring처럼 출력하는 로그가 많치 않으니 실행을 하고 나서도 내가 실행을 했나 라고 의심을 들기도 했다ㅎㅎㅎㅎ

## Spring 처럼 따라하기
- configuration.ts
```javascript
import * as os from 'os';
import * as fs from 'fs';
import * as yaml from 'js-yaml';
import {join} from 'path';
import {Logger} from '@nestjs/common';
import * as ip from 'ip';

const YAML_CONFIG_FILENAME = 'config.yml';

export default () => {
    const config = yaml.load(
        // start:dev
        fs.readFileSync(join(__dirname, YAML_CONFIG_FILENAME), 'utf8'),
        // start:build-dev
        // fs.readFileSync(join(__dirname, "config",YAML_CONFIG_FILENAME), 'utf8'),
    );
    const port = config[process.env.NODE_ENV || 'local']['common']['http-port'];
    const myIp = ip.address();
    // initialize
    Logger.log(',--.    ,--. ,--.                ,---.                   ,--.                    ,--.                      ,--.   \n' +
        '|  |    `--\' |  |,-.   ,---.    \'   .-\'   ,---.  ,--.--. `--\' ,--,--,   ,---.    |  |-.   ,---.   ,---.  ,-\'  \'-. \n' +
        '|  |    ,--. |     /  | .-. :   `.  `-.  | .-. | |  .--\' ,--. |      \\ | .-. |   | .-. \' | .-. | | .-. | \'-.  .-\' \n' +
        '|  \'--. |  | |  \\  \\  \\   --.   .-\'    | | \'-\' \' |  |    |  | |  ||  | \' \'-\' \'   | `-\' | \' \'-\' \' \' \'-\' \'   |  |   \n' +
        '`-----\' `--\' `--\'`--\'  `----\'   `-----\'  |  |-\'  `--\'    `--\' `--\'\'--\' .`-  /     `---\'   `---\'   `---\'    `--\'   \n' +
        '                                         `--\'                          `---\'                                      \n' +
        '${application.title} ${application.version}\n' +
        'Powered by Spring Boot ${spring-boot.version}')
    Logger.log('')
    Logger.log(
        '========================================================================================================',
    );
    Logger.log(`node version: ${process.version}`);
    Logger.log(`node env: ${process.env.NODE_ENV || 'local'}`);
    Logger.log(`cpu core: ${os.cpus().length}`);
    Logger.log(`host platform: ${process.platform}`);
    Logger.log(`host architecture: ${process.arch}`);
    Logger.log(`hostname: ${os.hostname()}`);
    Logger.log(`user home: ${os.userInfo().username}`);
    Logger.log(`user home directory: ${os.userInfo().homedir}`);
    Logger.log(`api ip: ${myIp}`);
    Logger.log(`http://${myIp}:${port}/api-docs`);
    Logger.log(
        '========================================================================================================',
    );

    return config;
};

```

```bash

[NestWinston] Info      5/3/2022, 1:42:59 AM [NestFactory] Starting Nest application... - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM ,--.    ,--. ,--.                ,---.                   ,--.                    ,--.                      ,--.   
|  |    `--' |  |,-.   ,---.    '   .-'   ,---.  ,--.--. `--' ,--,--,   ,---.    |  |-.   ,---.   ,---.  ,-'  '-. 
|  |    ,--. |     /  | .-. :   `.  `-.  | .-. | |  .--' ,--. |      \ | .-. |   | .-. ' | .-. | | .-. | '-.  .-' 
|  '--. |  | |  \  \  \   --.   .-'    | | '-' ' |  |    |  | |  ||  | ' '-' '   | `-' | ' '-' ' ' '-' '   |  |   
`-----' `--' `--'`--'  `----'   `-----'  |  |-'  `--'    `--' `--''--' .`-  /     `---'   `---'   `---'    `--'   
                                         `--'                          `---'                                      
${application.title} ${application.version}
Powered by Spring Boot ${spring-boot.version} - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM  - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM ======================================================================================================== - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM node version: v16.14.2 - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM node env: local - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM cpu core: 10 - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM host platform: darwin - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM host architecture: arm64 - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM hostname: hsnamui-MacBookPro.local - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM user home: hsnam - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM user home directory: /Users/hsnam - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM api ip: 192.168.0.135 - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM http://192.168.0.135:8082/api-docs - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM ======================================================================================================== - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM http://192.168.0.135:8082/api-docs - {}
[NestWinston] Info      5/3/2022, 1:42:59 AM start service - {}

```
- Application 시작시 위 로그 처럼 node version, node, env 정보, host 등등 많은 정보들이 출력 되는것을 확인 할 수 있다.
- 실제로 내가 만든 application을 다른 서버에 배포할때 위와 같은 출력 정보들은 상당히 많은 도움이 된다.
