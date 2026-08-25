# Cheet sheet

- update_at: 2026-08

## Powershell

```powershell
# Excute Windows Powershell as Administrator

winget update && winget upgrade winget
winget install gsudo bootandy.dust
gsudo winget install Microsoft.PowerShell
gsudo winget update && gsudo winget upgrade --all

# add WSL
wsl --list --verbose
wsl --install --distribution Debian
wsl -d Debian
```

```ini
# code C:\Users\{user}\.wslconfig

[wsl2]
memory=10GB
swap=2GB
networkingMode=mirrored
firewall=false

[boot]
systemd=true
```

## Linux apt-based distribution

| Image | Update cycle |
|-|-|
| Ubuntu non-LTS | 6 months |
| Debian stable slim | 2 years |

```bash
# === apt ===

# upgrade distribution dependencies
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y

mkdir -p w # make a user's working root
# exclude recommend dependencies when apt install
echo 'APT::Install-Recommends "false";' | sudo tee /etc/apt/apt.conf.d/99no-recommends

# host information
uname -a # linux kernel version
cat /etc/os-release # apt distribution version
cat /etc/apt/sources.list
sudo apt list --manual-installed
# remove source repository
sudo rm -f /etc/apt/sources.list.d/{repogitory}.list 

# purge when unuse cloud-init and vim

sudo apt purge cloud-init
sudo rm -rf /etc/cloud /var/lib/cloud
sudo apt install nano && sudo apt purge vim-tiny vim

# === ssh ===

sudo apt install ssh
ssh-keygen -t ed25519 # type "Enter" to continue
cat ~/.ssh/id_ed25519.pub # copy and paste to 3rd party inputs
chmod 600 ~/.ssh/id_ed25519

# === tmux ===

sudo apt install tmux -y
tmux new -A -s main # detouch using Ctrl + b -> d

# === git ===

sudo apt update && sudo apt install git
git config --global user.name "Andyou"
git config --global user.email "andyou@animagram.jp"

git diff --numstat branch1 branch2 # to confirm branches diff counts # count diff
sudo chown 1000:1000 -R . && git clean -fd # checkout aborting diffs

# === uv ===

curl -LsSf https://astral.sh/uv/install.sh | sh
```

## Docker

- [Docker: installation](https://docs.docker.com/engine/install)
- [Docker: rootless](https://docs.docker.com/engine/security/rootless/)

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin # docker-compose-plugin
docker login -u "" -p ""

docker ps -aq | xargs -r docker stop
docker ps -aq | xargs -r docker rm

docker system df             # disk usage
docker system prune          # delete unused 
sudo rm -rf /var/lib/docker/ /var/lib/containerd # delete all
```

note: containerd.io docker-ce docker-ce-cli docker-buildx-plugin for minimum install

```powershell
# optimize vhdx in Powershell
Optimize-VDisk -Path "Local\Docker\wsl\ext4.vhdx" -Mode Full
gsudo pwsh -NoProfile -Command "Optimize-VHD -Path 'C:AppData\Local\wsl\{token}\ext4.vhdx' -Mode Full"
```

## Nginx

```bash
sudo apt update && sudo apt upgrade -y && sudo apt install nginx

# === Server distribution ===
sudo mkdir -p /var/www/html/{project}/
sudo chown -R {user}:{user} /var/www/html/project}/
sudo chmod -R 755 /var/www/html/project}/
sudo systemctl edit nginx # See below (/etc/systemd/system/nginx.service.d/override.conf)
sudo systemctl enable nginx && sudo nginx -t && sudo systemctl reload-or-restart nginx

# === Local distribution ===
sudo local.conf /etc/nginx/conf.d/local.conf && sudo cp local.snippet.conf /etc/nginx/snippets/local.conf && sudo nginx -t && sudo systemctl reload-or-restart nginx
```

```nginx
# sudo systemctl edit nginx
# /etc/systemd/system/nginx.service.d/override.conf
[Service]
Restart=on-failure
RestartSec=5s
StartLimitIntervalSec=60
StartLimitBurst=3

# sudo nano /etc/nginx/nginx.conf

# Local distribution
user user;
worker_processes 1;
```

```nginx
# local.conf (/etc/nginx/conf.d/local.conf)

# === Template ===
server {
    listen 127.0.0.1:{port};
    server_name localhost;
    root /home/{user}/w/{repository}/distribution;
    include snippets/local.conf;
}
```

```nginx
# local.snippet.conf (/etc/nginx/snippets/local.conf)

absolute_redirect off;

autoindex on;
charset utf-8;

# === No cache ===
add_header Cache-Control "no-store, no-cache, must-revalidate" always;
etag off;
if_modified_since off;
open_file_cache off;
sendfile off;

# === MIME ===
default_type text/plain;

# === Exclusion ===
location ~ /\. {
    deny all;
}

location / {
    index index.html;
    try_files $uri $uri/ =404;
}
```

## Cloudflared

```bash
# Issue temporary https domain
tmux new -A -s tmp && cloudflared tunnel --url http://localhost:{port} # detouch using Ctrl + b -> d
```

## Rust

- [rustup: managemer](https://rust-lang.org/tools/install/)
- [cargo docs: toml specs](https://doc.rust-lang.org/cargo/reference/manifest.html)
- [wasm-pack: wasm builder](https://wasm-bindgen.github.io/wasm-pack/installer/)

## Github Actions

- [Github: workflow syntax](https://docs.github.com/ja/actions/reference/workflows-and-actions/workflow-syntax)
- [Github: runners' spec](https://github.com/actions/runner-images)

```yaml
name: "workflow name"

on:
    workflow_dispatch: # manual excution
    release:
        types: [published] # when released
    push:
        tags:
            - "*.*.*" # when tags created
        branches:
            - "main"

permissions:
    contents: "read"
    actions: "write" # when uploading Github Artifact

env: # refer as $variable in "run:"
    ENV_VAR:   ${{vars.ENV_VAR}}
    SECRET_KEY: ${{secrets.SECRET_KEY}}

jobs:
    deploy: # job name
        timeout-minutes: 30
        runs-on: "ubuntu-latest" # check host specs in documents 
        environment:
            name: github-pages
            url: ${{ steps.deployment.outputs.page_url }}
        # needs, if, stratergy
        steps:
            - name: "git checkout"
              uses: actions/checkout@v7

            - name: "upload artifact"
              # check major version in documents or automate with dependabot
              uses: actions/upload-artifact@v7
              with:
                name: "artifact_name"
                path: "./examples/target/"
                retention-days: 7

            - name: "setup Github Pages"
              uses: actions/configure-pages@v6

            - name: "upload artifact as Github Pages"
              uses: actions/upload-pages-artifact@v5
              with:
                path: "./examples"

            - name: "deploy to GitHub Pages"
              id: "deployment"
              uses: actions/deploy-pages@v5
```

## Github Dependabot

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/" # .github/workflows/*.yml
    schedule:
      interval: "weekly"
  - package-ecosystem: "cargo"
    directory: "/" # ./
    schedule:
      interval: "weekly"
```

---

## National holidays (JP)

```bash
# "昭和30年（1955年）から令和9年（2027年）国民の祝日（csv形式：22KB）"
curl -o syukujitsu.csv "https://www8.cao.go.jp/chosei/shukujitsu/syukujitsu.csv"
iconv -f SHIFT-JIS -t UTF-8 syukujitsu.csv | awk -F, '$1 ~ /^2026/ {print $1 "\t" $2}' > national_holidays_2026.tsv
rm syukujitsu.csv
```
