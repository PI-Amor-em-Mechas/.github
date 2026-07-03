# 💇 Amor em Mechas

**Amor em Mechas** é um sistema de gestão para salões de beleza, desenvolvido como Projeto Integrador (PI). A solução combina uma API REST para gerenciamento de clientes, um módulo de reconhecimento facial para controle de ponto dos colaboradores e uma distribuição Linux customizada para facilitar a implantação do sistema.

---

## 🗂️ Repositórios

### [Backend](https://github.com/PI-Amor-em-Mechas/Backend)

Monorepo com dois projetos independentes:

| Módulo | Tecnologias | Descrição |
|--------|-------------|-----------|
| `api_rest/api-para-formulario` | Java 21, Spring Boot 3, JPA, MySQL | API REST para cadastro e gestão de clientes do salão |
| `reconhecimento_facial` | Python 3.10+, OpenCV, MediaPipe, SQLite | Controle de ponto via reconhecimento facial dos colaboradores |

### [Build-iso\_live\_hybrid\_debian](https://github.com/PI-Amor-em-Mechas/Build-iso_live_hybrid_debian)

Distribuição Linux customizada baseada em **Debian 12 (Bookworm)**, gerada com [`live-build`](https://salsa.debian.org/live-team/live-build) e entregue como **ISO híbrida** (boot via BIOS legacy ou UEFI, gravável em DVD ou pendrive).

- Desktop XFCE com Nginx, Java runtime e Python pré-instalados
- MariaDB configurado com bind local e inicialização segura no primeiro boot
- Firewall UFW ativo com política default-deny
- Planejado para embarcar a API REST e o módulo de reconhecimento facial quando concluídos

---

## 🏗️ Arquitetura geral

```
┌─────────────────────────────────────────┐
│           ISO Debian Customizada        │
│  ┌──────────────────┐  ┌─────────────┐  │
│  │  API REST Java   │  │  Módulo     │  │
│  │  Spring Boot     │  │  Python     │  │
│  │  :8080           │  │  Flask :5000│  │
│  └────────┬─────────┘  └──────┬──────┘  │
│           │                   │          │
│  ┌────────▼───────────────────▼──────┐  │
│  │           MariaDB (local)         │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
```

---

## 🚀 Como começar

### Pré-requisitos

- Git
- Java 21 e Maven (para a API REST)
- Python 3.10+ (para o reconhecimento facial)
- MySQL/MariaDB rodando em `localhost:3306`
- Webcam (para o módulo de reconhecimento facial)

### Clonar e rodar o backend

```bash
git clone https://github.com/PI-Amor-em-Mechas/Backend.git
cd Backend
```

**API REST:**

```bash
cd api_rest/api-para-formulario
./mvnw spring-boot:run   # Linux/macOS
mvnw.cmd spring-boot:run # Windows
```

**Reconhecimento facial:**

```bash
cd reconhecimento_facial
python -m venv .venv
source .venv/bin/activate   # Linux/macOS
# .venv\Scripts\Activate.ps1  # Windows
pip install -r requirements.txt
python src/app.py
```

### Build da ISO

> Requer Debian/Ubuntu com `live-build` instalado.

```bash
git clone https://github.com/PI-Amor-em-Mechas/Build-iso_live_hybrid_debian.git
cd Build-iso_live_hybrid_debian
sudo apt install -y live-build
chmod +x local/bin/relink-hooks.sh auto/config auto/build auto/clean
./local/bin/relink-hooks.sh
sudo lb clean && sudo lb config && sudo lb build
```

---

## 🛠️ Stack tecnológica

| Camada | Tecnologias |
|--------|-------------|
| Backend REST | Java 21, Spring Boot 3, Spring Data JPA, Bean Validation, Maven |
| Banco de dados | MySQL / MariaDB |
| Reconhecimento facial | Python 3.10+, OpenCV, MediaPipe, SQLite |
| Infraestrutura | Debian 12, live-build, XFCE, Nginx, systemd |
| Segurança | UFW (firewall), MariaDB bind local, credenciais geradas no primeiro boot |

---

## 📄 Licença

A definir.