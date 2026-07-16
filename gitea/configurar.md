
# Configurando o Gitea com Podman Compose

O **Gitea** é um serviço de gerenciamento de código-fonte leve, auto-hospedado e de código aberto, funcionando como uma alternativa ao GitHub, GitLab e Bitbucket.

Neste guia será utilizado:

* Podman
* Podman Compose
* SQLite (banco de dados padrão)
* SSH para envio dos repositórios

> especificamente estou usando o BazziteOS como sistema operacional

---

## Criando a pasta do projeto

```bash
mkdir -p ~/gitea
cd ~/gitea
```

---

## Criando o arquivo `docker-compose.yml`

> Apesar do nome do arquivo ser `docker-compose.yml`, ele funciona normalmente com o **Podman Compose**.

```yaml
services:
  gitea:
    image: docker.io/gitea/gitea:1.24

    container_name: gitea
    # restart: unless-stopped - para iniciar junto com o sistema
    restart: "no"

    environment:
      USER_UID: 1000
      USER_GID: 1000
      TZ: America/Sao_Paulo

    volumes:
      # Pasta onde ficarão armazenados:
      # - repositórios
      # - banco de dados
      # - configurações
      # - anexos
      - /home/SEU_USUARIO/gitea/data:/data:z

      # Sincroniza o horário do container com o sistema
      - /etc/localtime:/etc/localtime:ro

    ports:
      # Interface Web
      - "3000:3000"

      # SSH
      # A porta 22 do container será acessível pela porta 2222
      # para não entrar em conflito com o SSH da máquina.
      - "2222:22"
```

## Observações

* Utilize uma **versão específica** da imagem em vez de `latest`, evitando atualizações inesperadas.
* O sufixo `:z` é necessário em distribuições com **SELinux** (Fedora, Bazzite, Silverblue etc.) para permitir que o container grave na pasta.

---

## Iniciando o serviço

```bash
podman-compose up -d
```

O comando irá:

* criar o container (caso ainda não exista);
* iniciar o Gitea em segundo plano.

---

## Gerenciando o serviço

Parar o serviço:

```bash
podman-compose stop
```

Iniciar novamente:

```bash
podman-compose start
```

Ver os logs:

```bash
podman-compose logs
```

Ver os logs em tempo real:

```bash
podman-compose logs -f
```

Remover os containers:

```bash
podman-compose down
```

> O comando `down` remove apenas os containers. Os dados permanecem preservados na pasta montada em `/data`.

---

## Primeiro acesso

Abra o navegador:

```html
http://localhost:3000
```

Na tela de instalação:

* Banco de dados: **SQLite3**
* Caminho do banco: mantenha o padrão
* URL do Gitea: ajuste conforme sua rede (ou mantenha `localhost` para uso local)
* Crie o usuário administrador
* Clique em **Install Gitea**

Após a instalação, o Gitea estará pronto para uso.

---

## Configurando acesso SSH

## Gerando uma chave SSH

Caso ainda não possua uma chave:

```bash
ssh-keygen -t ed25519 -C "seuemail@gmail.com"
```

---

## Copiando a chave pública

```bash
cat ~/.ssh/id_ed25519.pub
```

Exemplo:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx seuemail@gmail.com
```

---

## Adicionando a chave no Gitea

No Gitea:

```bash
Perfil
    └── Configurações
            └── Chaves SSH / GPG
```

Cole a chave pública e salve.

---

## Testando a conexão

```bash
ssh -T -p 2222 git@localhost
```

Na primeira conexão será exibida uma mensagem semelhante a:

```text
Are you sure you want to continue connecting (yes/no)?
```

Digite:

```text
yes
```

Se tudo estiver correto, o Gitea responderá com uma mensagem de boas-vindas.

---

## Trabalhando com repositórios

Existem duas formas mais comuns.

---

## 1. Criando um novo projeto local

Crie um repositório vazio no Gitea pela interface web.

Depois:

```bash
touch README.md

git init

git branch -M main

git add .

git commit -m "Primeiro commit"

git remote add origin ssh://git@localhost:2222/SEU_USUARIO/meu-projeto.git

git push -u origin main
```

---

## 2. Clonando um repositório existente

Caso o projeto já exista no Gitea:

```bash
git clone ssh://git@localhost:2222/SEU_USUARIO/meu-projeto.git
```

Entre na pasta:

```bash
cd meu-projeto
```

Faça alterações normalmente:

```bash
git add .

git commit -m "Minha alteração"

git push
```

---

## Utilizando HTTP em vez de SSH

Também é possível utilizar HTTP.

Adicionar o remoto:

```bash
git remote add origin http://localhost:3000/SEU_USUARIO/meu-projeto.git
```

Ou clonar:

```bash
git clone http://localhost:3000/SEU_USUARIO/meu-projeto.git
```

Entretanto, para uso frequente, **SSH costuma ser mais prático**, pois evita informar usuário e senha (ou token) a cada envio.

---

## Utilizando GitHub e Gitea ao mesmo tempo

Um mesmo projeto pode possuir mais de um repositório remoto.

Adicionar o GitHub:

```bash
git remote add github git@github.com:usuario/projeto.git
```

Adicionar o Gitea:

```bash
git remote add gitea ssh://git@localhost:2222/usuario/projeto.git
```

Verificar os remotos configurados:

```bash
git remote -v
```

Enviar para o GitHub:

```bash
git push github main
```

Enviar para o Gitea:

```bash
git push gitea main
```

---

## Estrutura dos dados

Ao utilizar o volume:

```text
/home/SEU_USUARIO/gitea/data
```

serão armazenados:

* todos os repositórios Git;
* banco de dados SQLite;
* usuários;
* organizações;
* configurações;
* anexos;
* chaves SSH.

Fazer backup dessa pasta é suficiente para preservar praticamente toda a instalação.

---
