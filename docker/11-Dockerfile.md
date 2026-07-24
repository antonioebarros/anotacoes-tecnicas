

# Dockerfile

Um **Dockerfile** é um arquivo de texto que descreve, passo a passo, como construir uma imagem de container.

## Vantagens

- ✅ **Automação**: a imagem é construída sempre da mesma forma.

- ✅ **Reprodutibilidade**: qualquer pessoa obtém exatamente o mesmo ambiente.

- ✅ **Versionamento**: o Dockerfile pode ser versionado com Git, permitindo acompanhar alterações.

- ✅ **Legibilidade**: toda a configuração da imagem fica documentada em um único arquivo.

- ✅ **Colaboração**: facilita o trabalho em equipe, pois todos utilizam o mesmo ambiente.

- ✅ **Portabilidade**: a mesma imagem pode ser executada em qualquer máquina com Docker ou Podman.

- ✅ **Infraestrutura como Código (IaC)**: a configuração do ambiente passa a fazer parte do código do projeto.

---

## Exemplo

```dockerfile
# Imagem base
FROM ubuntu:22.04

# Executa comandos durante a construção da imagem
RUN apt-get update && apt-get install -y \
    python3.11 \
    python3.11-dev \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Define o diretório de trabalho
WORKDIR /app

# Copia os arquivos do projeto para dentro da imagem
COPY . .

# Instala as dependências da aplicação
RUN python3.11 -m pip install -r requirements.txt

# Documenta a porta utilizada pela aplicação
EXPOSE 8083

# Define variáveis de ambiente
ENV LOGOMARCA="ae"
ENV FOTO="https://avatars.githubusercontent.com/u/227850709?v=4"
ENV NOME="Antonio Eduardo"
ENV IDADE="30"
ENV EMAIL="antoniodev@gmail.com"
ENV PROFISSAO="DevOps"
ENV SITE="www.antoniodevops.com"

# Comando executado quando o container iniciar
CMD ["python3.11", "app.py"]
```

---

# Construindo a imagem

```bash
docker build -t minha-app-python:v1 .
```

### Explicação

- `build` → constrói uma imagem.

- `-t` → atribui um nome (tag) à imagem.

- `minha-app-python` → nome da imagem.

- `v1` → versão (tag).

- `.` → utiliza o diretório atual como contexto de construção.

---

# Criando um container a partir da imagem

```bash
docker run -it --name teste-app -p 8083:8083 minha-app-python:v1
```

### Explicação

- `run` → cria e inicia um container.

- `-it` → modo interativo com terminal.

- `--name teste-app` → nome do container.

- `-p 8083:8083` → mapeia a porta do host para a porta do container.

- `minha-app-python:v1` → imagem utilizada para criar o container.

---

# Fluxo de trabalho

```text
Código da aplicação
        │
        ▼
Dockerfile
        │
        ▼
docker build
        │
        ▼
Imagem
        │
        ▼
docker run
        │
        ▼
Container
        │
        ▼
Aplicação em execução
```




