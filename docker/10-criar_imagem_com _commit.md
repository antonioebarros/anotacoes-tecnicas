# Criar uma imagem com `commit`

Normalmente, uma imagem é criada a partir de outra, como `ubuntu`, `debian` ou `alpine`.

Uma imagem é formada por **camadas (layers)**. Cada camada representa uma alteração no sistema de arquivos, como a instalação de um programa, a criação de um arquivo ou uma mudança de configuração. Essas camadas formam a "receita" da imagem.



O commit Salva o estado atual de um container como uma nova imagem.

## Exemplo

Criando um container baseado na imagem Debian:

```bash
docker run -it --name debian-base debian
```

Dentro do container, instalando o `vim`:

```bash
apt update
apt install vim
```

Depois de finalizar as alterações, criando uma nova imagem a partir desse container:

```bash
docker commit debian-base debian-vim
```

Agora existe uma nova imagem chamada `debian-vim`.

Todos os containers criados a partir dessa imagem já terão o `vim` instalado.

```bash
docker run -it debian-vim
```

## Como funciona

```text
Imagem Debian
      │
      ▼
Container
      │
Instalação do vim
      │
      ▼
docker commit
      │
      ▼
Imagem debian-vim
```

> **Observação:** `docker commit` (ou `podman commit`) cria uma **nova camada** contendo as alterações feitas no container. A imagem original (`debian`) permanece inalterada.

---

> **Boas práticas:** `commit` é muito útil para aprendizado, testes e para salvar rapidamente o estado de um container. Em ambientes de desenvolvimento e produção o mais comum é criar um **Dockerfile** (ou **Containerfile**), pois ele documenta todas as etapas de construção da imagem e permite reproduzi-la facilmente em qualquer máquina.
