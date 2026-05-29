# Ubuntu 24.04 Development Environment Setup Guide

## Docker + Docker Compose + NVM + Node.js + PNPM

---

# 1. System Update

Ubuntu-г шинэчилж үндсэн хэрэгслүүдийг суулгана.

```bash
sudo apt update
sudo apt upgrade -y

sudo apt install -y \
  curl \
  wget \
  git \
  unzip \
  nano \
  vim \
  build-essential \
  ca-certificates \
  gnupg
```

Шалгах:

```bash
git --version
curl --version
```

---

# 2. Docker Engine суулгах

## 2.1 Docker Repository нэмэх

```bash
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
| sudo gpg --dearmor \
-o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo ${UBUNTU_CODENAME:-$VERSION_CODENAME}) stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

## 2.2 Docker суулгах

```bash
sudo apt update

sudo apt install -y \
docker-ce \
docker-ce-cli \
containerd.io \
docker-buildx-plugin \
docker-compose-plugin
```

---

## 2.3 Docker шалгах

```bash
docker --version
docker compose version
```

Test:

```bash
sudo docker run hello-world
```

---

## 2.4 Docker-г sudo шаардлагагүй болгох

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
```

Шинэ group идэвхжүүлэх:

```bash
newgrp docker
```

Шалгах:

```bash
docker run hello-world
```

---

## 2.5 Docker Service автоматаар асаах

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Шалгах:

```bash
sudo systemctl status docker
```

---

# 3. NVM (Node Version Manager) суулгах

NVM нь олон Node.js version удирдах зориулалттай.

---

## 3.1 NVM суулгах

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

Shell reload:

```bash
source ~/.bashrc
```

эсвэл

```bash
source ~/.zshrc
```

---

## 3.2 Шалгах

```bash
nvm --version
```

---

## 3.3 Node.js LTS суулгах

```bash
nvm install --lts
nvm use --lts
```

---

## 3.4 Node.js 22 суулгах

```bash
nvm install 22
nvm use 22
```

---

## 3.5 Default version болгох

```bash
nvm alias default 22
```

---

## 3.6 Version шалгах

```bash
node -v
npm -v
```

---

## 3.7 Суулгасан Node хувилбарууд

```bash
nvm ls
```

---

## 3.8 Боломжтой бүх хувилбар

```bash
nvm ls-remote
```

---

## 3.9 Version устгах

```bash
nvm uninstall 18
```

---

# 4. PNPM суулгах

PNPM нь NPM-ээс хурдан бөгөөд disk space бага ашигладаг.

---

## 4.1 Corepack идэвхжүүлэх

```bash
corepack enable
```

---

## 4.2 PNPM суулгах

```bash
corepack prepare pnpm@latest --activate
```

---

## 4.3 Шалгах

```bash
pnpm -v
```

---

## 4.4 Альтернатив арга

```bash
npm install -g pnpm
```

---

# 5. PNPM ашиглах

## Шинэ төсөл

```bash
mkdir my-project
cd my-project

pnpm init
```

---

## Package суулгах

```bash
pnpm add axios
```

---

## Dev Dependency

```bash
pnpm add -D typescript eslint prettier
```

---

## Package устгах

```bash
pnpm remove axios
```

---

## Update

```bash
pnpm update
```

---

## Install

```bash
pnpm install
```

эсвэл

```bash
pnpm i
```

---

## Script ажиллуулах

```bash
pnpm dev
```

эсвэл

```bash
pnpm run dev
```

---

# 6. Git Project ажиллуулах

Repository татах:

```bash
git clone <repository-url>

cd <project>
```

---

## Dependency суулгах

```bash
pnpm install
```

эсвэл

```bash
npm install
```

---

## Environment тохируулах

```bash
cp .env.example .env
```

```bash
nano .env
```

---

# 7. Docker Compose ашиглах

Project бүтэц:

```text
project/
├── Dockerfile
├── docker-compose.yml
├── package.json
├── pnpm-lock.yaml
├── .env
└── src/
```

---

## Build

```bash
docker compose build
```

---

## Run

```bash
docker compose up -d
```

---

## Build + Run

```bash
docker compose up -d --build
```

---

## Logs харах

```bash
docker compose logs -f
```

---

## Container жагсаалт

```bash
docker ps
```

---

## Зогсоох

```bash
docker compose down
```

---

## Volume устгах

```bash
docker compose down -v
```

---

# 8. Dockerfile (Node + PNPM)

```dockerfile
FROM node:22-alpine

RUN corepack enable

WORKDIR /app

COPY package.json pnpm-lock.yaml ./

RUN pnpm install --frozen-lockfile

COPY . .

RUN pnpm build

CMD ["pnpm", "start"]
```

---

# 9. Docker Compose Example

```yaml
services:

  api:
    build: .
    container_name: api

    ports:
      - "3000:3000"

    env_file:
      - .env

    restart: unless-stopped

    depends_on:
      - postgres

  postgres:
    image: postgres:16

    environment:
      POSTGRES_DB: app_db
      POSTGRES_USER: app_user
      POSTGRES_PASSWORD: secret

    ports:
      - "5432:5432"

    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

---

# 10. Monorepo (PNPM Workspace)

Project бүтэц:

```text
apps/
  api/
  web/

packages/
  shared/
```

---

## pnpm-workspace.yaml

```yaml
packages:
  - apps/*
  - packages/*
```

---

## Install

```bash
pnpm install
```

---

## Тодорхой App ажиллуулах

```bash
pnpm --filter api dev
```

---

# 11. Docker Commands

## Container

```bash
docker ps
docker ps -a
```

---

## Image

```bash
docker images
```

---

## Volume

```bash
docker volume ls
```

---

## Network

```bash
docker network ls
```

---

## Shell рүү орох

```bash
docker compose exec api sh
```

эсвэл

```bash
docker compose exec api bash
```

---

# 12. PNPM Cache

Store Path:

```bash
pnpm store path
```

---

Cache цэвэрлэх:

```bash
pnpm store prune
```

---

Хэмжээ харах:

```bash
du -sh $(pnpm store path)
```

---

# 13. Firewall

Port нээх:

```bash
sudo ufw allow 3000/tcp
```

PostgreSQL:

```bash
sudo ufw allow 5432/tcp
```

---

Шалгах:

```bash
sudo ufw status
```

---

# 14. Project Startup Checklist

```bash
git clone <repo>

cd <repo>

cp .env.example .env

nano .env

pnpm install

docker compose up -d --build

docker compose logs -f
```

---

# 15. Recommended Production Environment

```text
Ubuntu 24.04 LTS
│
├── Git
├── Docker Engine
├── Docker Compose v2
├── NVM
├── Node.js 22 LTS
├── PNPM
├── PostgreSQL
├── Redis
├── Nginx
└── VS Code
```

---

# 16. Environment Verification

```bash
docker --version
docker compose version

nvm --version

node -v
npm -v

pnpm -v
```

Хэрэв дээрх бүх команд амжилттай ажиллаж байвал Ubuntu 24.04 хөгжүүлэлтийн орчин бүрэн бэлэн болсон байна.
