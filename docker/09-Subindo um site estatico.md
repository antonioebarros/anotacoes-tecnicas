# Subindo um site estático

A imagem `dockersamples/static-site` está hospedada no Docker Hub sob o namespace `dockersamples`.

Em imagens que não pertencem à biblioteca oficial (*Official Images*), a referência segue o formato:

```text
<usuário>/<imagem>
```

Exemplo:

```text
dockersamples/static-site
```

---

## Deixando o Docker escolher a porta

```bash
docker run -d -P dockersamples/static-site
```

A opção `-P` (`--publish-all`) publica automaticamente todas as portas expostas (`EXPOSE`) pela imagem em portas aleatórias do host.

Para descobrir qual porta foi escolhida:

```bash
docker container port <container>
```

ou

```bash
docker port <container>
```

Depois, basta acessar a porta informada no navegador.

---

## Definindo a porta manualmente

```bash
docker run -d -p 5000:80 dockersamples/static-site
```

Nesse exemplo:

- `5000` → porta do host.
- `80` → porta utilizada pela aplicação no container.

A aplicação ficará disponível em:

```text
http://localhost:5000
```
