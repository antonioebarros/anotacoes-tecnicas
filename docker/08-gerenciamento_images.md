# Gerenciamento de imagens

## Listar imagens

```bash
docker image ls
```

Lista as imagens armazenadas localmente.

> **Alias:**
> 
> ```bash
> docker images
> ```

---

## Baixar uma imagem

```bash
docker image pull <imagem>
```

Baixa uma imagem de um registro (registry), como o Docker Hub.

Exemplo:

```bash
docker image pull nginx
```

> **Nota:** normalmente esse comando não precisa ser executado manualmente. Se a imagem não existir localmente, o `docker run` fará o download automaticamente.

---

## Remover uma imagem

```bash
docker image rm <imagem>
```

Remove uma imagem do armazenamento local.

A imagem pode ser identificada por:

- nome (`nginx`)
- nome e tag (`nginx:latest`)
- ID da imagem

Exemplos:

```bash
docker image rm nginx
```

```bash
docker image rm nginx:latest
```

> **Importante:** uma imagem só pode ser removida se não houver containers utilizando-a (inclusive parados).

Caso exista algum container associado, remova-o primeiro:

```bash
docker container rm <container>
```

---

## Remover imagens sem uso

```bash
docker image prune
```

Remove imagens sem uso (*dangling images*).

Para remover **todas** as imagens não utilizadas por nenhum container:

```bash
docker image prune -a
```

---

## Inspecionar uma imagem

```bash
docker image inspect <imagem>
```

Exibe informações detalhadas da imagem em formato JSON.

---

## Criar uma tag

```bash
docker image tag <imagem> <nova_tag>
```

Cria uma nova tag para uma imagem existente.

Exemplo:

```bash
docker image tag nginx meu-nginx:v1
```

---

## Construir uma imagem

```bash
docker image build <caminho>
```

Constrói uma imagem a partir de um `Dockerfile`.

Exemplo:

```bash
docker image build -t meu-app:v1 .
```

---

## Enviar uma imagem

```bash
docker image push <imagem>
```

Envia uma imagem para um registro (registry), como o Docker Hub.

Exemplo:

```bash
docker image push meu-usuario/meu-app:v1
```
