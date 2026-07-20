

# Conceitos do Docker

## Container

Um **container** é uma instância em execução de uma imagem. Ele executa um ou mais processos isolados do restante do sistema utilizando recursos do kernel Linux, como **namespaces** e **cgroups**.

Cada container possui seu próprio:

- sistema de arquivos;

- processos;

- pilha de rede;

- variáveis de ambiente;

- usuários e permissões (isolados).

Ao ser criado, o container reutiliza as camadas somente leitura da imagem e recebe uma camada gravável exclusiva.

Em arquiteturas modernas, é comum que cada container tenha uma única responsabilidade, como:

- servidor web;

- banco de dados;

- API;

- cache;

- fila de mensagens.

---

## Container Engine

Um **Container Engine** é um software responsável por criar, executar e gerenciar containers.

Ele faz a interface entre o usuário e os recursos do sistema operacional necessários para a execução dos containers.

Exemplos:

- Docker Engine

- Podman

- containerd

- CRI-O

Todos utilizam recursos do kernel Linux para isolar processos.

---

## Docker Engine

O **Docker Engine** é uma implementação de um Container Engine.

Ele é a plataforma responsável por construir imagens, criar containers e gerenciar redes, volumes e outros recursos do Docker.

O Docker Engine é composto principalmente por:

- **Docker Daemon (`dockerd`)**

- **Docker API**

- **Docker CLI (`docker`)**

---

## Docker Daemon (`dockerd`)

É o serviço executado em segundo plano que gerencia praticamente todo o ciclo de vida do Docker.

Ele recebe solicitações da API (normalmente enviadas pelo Docker CLI) e realiza operações como:

- construir imagens;

- criar containers;

- iniciar e parar containers;

- remover containers;

- gerenciar redes;

- gerenciar volumes;

- baixar e enviar imagens para registries.

---

## Docker CLI (Command Line Interface)

É a interface de linha de comando utilizada pelo usuário.

Ela envia solicitações ao Docker Daemon através da Docker API.

Exemplos:

```bash
docker run
docker build
docker ps
docker images
docker pull
docker push
```

O CLI não executa containers diretamente; ele apenas solicita operações ao Docker Daemon.

---

## Docker API

É a interface utilizada para comunicação com o Docker Engine.

O Docker CLI utiliza essa API, mas qualquer aplicação também pode utilizá-la.

Isso permite criar ferramentas gráficas, scripts ou sistemas de automação que controlam o Docker.

Fluxo simplificado:

```
Docker CLI
      │
      ▼
 Docker API
      │
      ▼
 Docker Daemon
      │
      ▼
Kernel Linux
```

---

## Imagem Docker

Uma **imagem Docker** é um modelo imutável composto por diversas camadas (**layers**) somente leitura.

Ela contém tudo o que uma aplicação precisa para ser executada:

- sistema de arquivos;

- binários;

- bibliotecas;

- dependências;

- configurações.

Uma imagem não executa processos.

Quando um container é criado:

1. o Docker reutiliza as camadas da imagem;

2. adiciona uma camada gravável exclusiva;

3. inicia o processo principal definido na imagem.

### Analogia com Orientação a Objetos

- **Imagem** → Classe

- **Container** → Objeto (instância da classe)

É apenas uma analogia didática; tecnicamente os conceitos são diferentes.

---

## Registry

Um **Registry** é um serviço responsável por armazenar e distribuir imagens Docker.

Ele funciona de maneira semelhante a um repositório Git, mas para imagens de containers.

Principais comandos:

```bash
docker pull
docker push
```

Os registries podem ser públicos ou privados.

---

## Docker Hub

O **Docker Hub** é o registry público oficial do Docker.

Além dele, existem diversos outros registries, como:

- GitHub Container Registry (GHCR)

- Amazon Elastic Container Registry (ECR)

- Google Artifact Registry

- Azure Container Registry (ACR)

- Harbor (self-hosted)

---

## Relação entre os componentes

```text
                 docker run nginx
                        │
                        ▼
                 Docker CLI (cliente)
                        │
                        ▼
                  Docker API
                        │
                        ▼
              Docker Daemon (dockerd)
                        │
      ┌─────────────────┴─────────────────┐
      │                 │                 │
      ▼                 ▼                 ▼
   Imagens          Containers       Redes/Volumes
                        │
                        ▼
                 Kernel Linux
          (namespaces + cgroups)
```
