# Docker run

## `docker run`

Cria e inicia um novo container a partir de uma imagem.

Se a imagem não existir localmente, o Docker faz o download automaticamente do registry configurado (por padrão, o Docker Hub).

Sintaxe:

```bash
docker run [OPÇÕES] IMAGEM [COMANDO]
```

Exemplo:

```bash
docker run debian bash --version
```

> **Importante:** `docker run` sempre cria um **novo** container.

Internamente ele equivale a:

```text
docker run
    ├── docker create
    └── docker start
```

Por isso, executar `docker run` novamente não reinicia um container existente, e sim cria outro.

---

## Nomeando um container

```bash
docker run --name meuDebian -it debian bash
```

### `--name`

Define um nome para o container.

Sem essa opção, o Docker gera um nome aleatório, por exemplo:

```text
happy_turing
eager_morse
```

Isso facilita o gerenciamento:

```bash
docker start meuDebian
docker stop meuDebian
docker rm meuDebian
```

---

## Flags comuns

### `-i` (`--interactive`)

Mantém a entrada padrão (STDIN) aberta, permitindo enviar comandos para o processo em execução.

### `-t` (`--tty`)

Cria um pseudo-terminal (TTY), oferecendo uma experiência semelhante à de um terminal Linux, com prompt, edição de linha e cores.

### `-it`

Combinação de `-i` e `-t`.

É a forma mais comum de iniciar um container para uso interativo.

Exemplo:

```bash
docker run -it debian bash
```
