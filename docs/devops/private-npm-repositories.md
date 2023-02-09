---
layout: default
title: Private NPM
parent: Setup DevOps
---

# NPM 프라이빗 저장소 만들기 (use verdaccio)

- EC2 `t3.nano` 인스턴스 올리고.

```sh
# install node v8
$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
$ sudo apt-get install -y nodejs
$ node --version
$ npm --version
> 6.4.1
$ npm i -g pm2 verdaccio
$ verdaccio
$ vi ~/.config/verdaccio/config.yml
listen:
  - 0.0.0.0:4873
$ sudo apt-get install -g pm2
$ pm2 start verdaccio
$ npm set always-auth true 
$ npm adduser
> lemon/lemon1212
```

- `Elastic IP` 로 고정ID 설정한 다음 `Route 53`에 `npm.lemoncloud.io` 구성함.


## 프로젝트내에서 다음과 같이 실행!

- 수동으로 project배포하고자 할때.

```sh
# 최초! 로그인하기.
$ npm set registry http://npm.lemoncloud.io:4873
$ npm login
$ npm whoami
> steve

# 프로젝트내에서 배포 시작.
$ npm publish --access=private --registry http://npm.lemoncloud.io:4873
```


## `package.json` 파일 업데이트.

- 설정파일에 다음과 같이 적용후 `$ npm publish`로 적용.

```json
{
  "!private": "NOTE - private npm 저장소에 배포함!",
  "private": false,
  "publishConfig": {
    "registry": "http://npm.lemoncloud.io:4873"
  },
  "files": [
    "dist/**/*"
  ],
}
```

## 생성한 패키지 제거

아래 명령어를 사용하시면 패키지를 verdaccio에서 제거할 수 있습니다. 단, 72시간이 지나면 npm에 문의하여 삭제하실 수 있습니다. (https://www.npmjs.com/support)

```sh
$ npm unpublish [package-name] --force
```
