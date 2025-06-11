# .config/aqua

らきさんの[officel/config_aqua](https://github.com/officel/config_aqua/tree/main)を参考に作成

## .bashrc

```bash
# cli version manager aqua
export PATH="$(aqua root-dir)/bin:$PATH"
export AQUA_GLOBAL_CONFIG=${XDG_CONFIG_HOME:-$HOME/.config}/aqua/aqua.yaml
```

## alias

```bash
alias aq='aqua'
alias aqcd="cd ${XDG_CONFIG_HOME:-$HOME/.config}/aqua/"
alias aqgi='aqua generate -i -o $AQUA_GLOBAL_CONFIG'
alias aqia='aqua install --all'
alias aqli='aqua list --installed --all | sort'
alias aqup='aqua update'
```

## task

```bash
$ aqcd
aqua $ task
task: Available tasks for this project:
* aqua:git:          auto git, use -- COMMIT TITLE                    (aliases: ag)
* aqua:update:       Run aqua update, install, list for globally      (aliases: au)

aqua $ task ls
task: Available tasks for this project:
* _git:
* _git:auto:
* _git:gh:
* aqua:git:           auto git, use -- COMMIT TITLE                    (aliases: ag)
* aqua:update:        Run aqua update, install, list for globally      (aliases: au)
* default:
* util:list:                (aliases: ul, ls)
* util:summary:             (aliases: us, la)

aqua $ task au
<omit> aqua up が実行される

aqua $ task ag
<omit> git add aqua.yaml から commit, push, gh pr create, gh pr merge まで自動化
```

## direnv
### 必要に応じて設定
- .direnv と .env で GitHub token を設定
  - https://blog.p1ass.com/posts/direnv-dotenv/
- [Tips｜aqua CLI Version Manager 入門](https://zenn.dev/shunsuke_suzuki/books/aqua-handbook/viewer/tips#github_token%2C-aqua_github_token-%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%97%E3%81%A6-rate-limit-%E3%82%92%E5%9B%9E%E9%81%BF%E3%81%99%E3%82%8B)

## Taskfileとの連携
チームで使う場合と個人で使う場合は、以下のようにincludesにチーム用のTaskfileを読み込んで利用する。
https://taskfile.dev/usage/#including-other-taskfiles
```yml
# Taskfile.yml
includes:
  tests: ${HOME}/Taskfile.dist.yml

env:
  GREETING: Hey, there!

tasks:
  greet:
    cmds:
      - echo $GREETING
```
呼び出し
```bash
# Taskfile.dist.yml
task tests:au
task tests:ag

# Taskfile.yml
task greet
```
