# Volumes

Volumes permitem armazenar dados fora do sistema de arquivos do container.

Sem um volume, os dados criados dentro do container são perdidos quando ele é removido.

---

## Por que usar?

Sem volume:

```text
Container
┌──────────────────────┐
│ index.html           │
│ style.css            │
│ app.js               │
└──────────────────────┘
```

Ao remover o container:

```bash
docker rm meu-nginx
```

todos os arquivos são perdidos.

Com um volume:

```text
Host                     Container
┌──────────────┐         ┌─────────────────────────┐
│ index.html   │ ─────►  │ /usr/share/nginx/html   │
│ style.css    │         │                         │
│ app.js       │         │                         │
└──────────────┘         └─────────────────────────┘
```

Os arquivos permanecem no host e podem ser reutilizados por outros containers.

---

## Tipos de montagem

| Tipo           | Descrição                                |
| -------------- | ---------------------------------------- |
| **Bind Mount** | Utiliza uma pasta existente no host.     |
| **Volume**     | Diretório gerenciado pelo Docker.        |
| **tmpfs**      | Dados armazenados apenas na memória RAM. |

Na prática, **Bind Mount** e **Volume** são os mais utilizados.

---

## Bind Mount

Conecta uma pasta do host diretamente a uma pasta do container.

Sintaxe:

```bash
-v <host>:<container>
```

Exemplo:

```bash
docker run \
    -p 8080:80 \
    -v "$(pwd):/usr/share/nginx/html" \
    nginx
```

Mapeamento:

```text
Host                     Container
$(pwd)        ─────►      /usr/share/nginx/html
```

Alterações feitas nos arquivos do host ficam disponíveis imediatamente no container.

### Sintaxe do `-v`

```text
HOST:CONTAINER
```

Exemplos:

```bash
-v ~/site:/usr/share/nginx/html
-v /dados:/backup
-v $(pwd):/app
```

> Leia sempre da esquerda para a direita: **host → container**.

---

## Volume Nomeado

Permite que o Docker gerencie o armazenamento dos dados.

Criando um volume:

```bash
docker volume create meus-dados
```

Utilizando:

```bash
docker run \
    -v meus-dados:/dados \
    alpine
```

No Linux, os volumes normalmente ficam em:

```text
/var/lib/docker/volumes/
```

> Não é recomendado modificar esses arquivos manualmente.

---

## Quando usar?

| Situação                                      | Tipo recomendado   |
| --------------------------------------------- | ------------------ |
| Desenvolvimento (código, HTML, configurações) | **Bind Mount**     |
| Bancos de dados e dados persistentes          | **Volume Nomeado** |
| Dados temporários                             | **tmpfs**          |

---

## Exemplo

Estrutura:

```text
meu-site/
├── index.html
└── style.css
```

Executando:

```bash
cd meu-site

docker run \
    -d \
    -p 8080:80 \
    -v "$(pwd):/usr/share/nginx/html" \
    nginx
```

Resultado:

```text
Host                          Container

index.html ───────────────► /usr/share/nginx/html/index.html
style.css  ───────────────► /usr/share/nginx/html/style.css
```

As alterações feitas no host são refletidas imediatamente no container.

---

## Resumo

| Tipo           | Gerenciado por | Uso principal      |
| -------------- | -------------- | ------------------ |
| **Bind Mount** | Usuário        | Desenvolvimento    |
| **Volume**     | Docker         | Dados persistentes |
| **tmpfs**      | Memória RAM    | Dados temporários  |
