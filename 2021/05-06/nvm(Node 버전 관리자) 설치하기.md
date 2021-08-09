# 설치 및 업데이트

아래 명령어를 실행하면 nvm이 설치됩니다.

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
# 또는
$ wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

성공적으로 설치되면 아래 경로에 폴더가 생긴다.

```bash
$ ls ~/.nvm
```

추가적으로 **NVM 관련 환경 설정**을 위해 profile 설정 파일(`~/.bash_profile`, `~/.zshrc`, `~/.profile` or `~/.bashrc`)에 아래 내용을 추가 합니다.

설정 후 쉘을 재시작 해줘야합니다. (또는 `source {profile 설정 파일}`을 해주면 현재 쉘에 적용된다.)

```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

---

참고 ([nvm 공식 github 바로가기](https://github.com/nvm-sh/nvm))
