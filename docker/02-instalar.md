# Instalando o Docker Engine no Debian 13

> **Observação:** Esta anotação é destinada a distribuições baseadas em Debian, como Debian, Ubuntu e Linux Mint.

---

## Método rápido (script oficial)

Este método utiliza o script oficial disponibilizado pelo Docker. É prático para laboratórios, máquinas de teste e ambientes de desenvolvimento.

### 1. Atualizar o sistema

```bash
sudo apt update && sudo apt upgrade
```

### 2. Baixar o script de instalação

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
```

### 3. Executar o instalador

```bash
sudo sh get-docker.sh
```

### 4. Adicionar o usuário ao grupo `docker`

Isso permite executar comandos Docker sem utilizar `sudo`.

```bash
sudo usermod -aG docker $USER
```

### 5. Atualizar os grupos da sessão

Você pode encerrar e iniciar a sessão novamente ou executar:

```bash
newgrp docker
```

### 6. Verificar a instalação

```bash
docker --version
```

Opcionalmente, execute o container de teste:

```bash
docker run hello-world
```

---

## Método recomendado

Para ambientes de produção ou máquinas de uso contínuo, recomenda-se instalar o Docker Engine utilizando o repositório oficial do Docker, conforme a documentação oficial.

Documentação:

- https://docs.docker.com/engine/install/debian/

---

## Observações

- O Docker Engine instala o **Docker Daemon (`dockerd`)**, o **Docker CLI (`docker`)** e outros componentes necessários para executar containers.
- O grupo `docker` concede permissões para controlar o Docker Engine. Usuários pertencentes a esse grupo possuem privilégios elevados sobre o sistema.
- 
