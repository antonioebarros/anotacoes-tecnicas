# Portas

## Mapeamento de portas

Por padrão, os containers executam em uma rede isolada, Isso significa que serviços executados dentro deles não podem ser acessados diretamente pelo computador hospedeiro (host).

Para expor um serviço, utilize a opção `-p` (`--publish`) no comando `docker run`.

Sintaxe:

```bash
docker run -p <porta_host>:<porta_container> <imagem>
```

Forma explícita:

```bash
docker container run -p <porta_host>:<porta_container> <imagem>
```

Exemplo:

```bash
docker run -p 8080:80 nginx
```

Nesse exemplo:

- `8080` → porta do host.
- `80` → porta utilizada pelo Nginx no container.

Ao acessar:

```text
http://localhost:8080
```

a requisição é encaminhada para a porta **80** do container.

> **Importante:** a porta do host e a porta do container não precisam ser iguais.

Exemplo:

```bash
docker run -p 3000:80 nginx
```

Nesse caso:

```text
Host (3000) ─────► Container (80)
```
