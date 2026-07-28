# TMPFS

`tmpfs` é um sistema de arquivos temporário armazenado em memória (RAM). Ele é útil para dados que não precisam ser persistidos, como arquivos temporários, caches ou informações sensíveis.

Quando um container é encerrado (ou removido), todo o conteúdo armazenado em um `tmpfs` é perdido.

## Sintaxe

Usando `--tmpfs`:

```bash
docker run -it --tmpfs /diretorio-no-container <imagem>
```

Usando `--mount`:

```bash
docker run -it \
  --mount type=tmpfs,destination=/diretorio-no-container \
  <imagem>
```

> **Observação:** A opção `--mount` é a sintaxe mais moderna e flexível, sendo recomendada para configurações mais complexas.

## Exemplo

```bash
docker run -it --tmpfs /minha-app ubuntu
```

Nesse exemplo, o Docker monta um sistema de arquivos `tmpfs` em `/minha-app` dentro do container.

Tudo o que for gravado nesse diretório será armazenado apenas na memória RAM do host. Assim que o container for encerrado, todos os arquivos desse diretório serão apagados.

Se um novo container for criado com o mesmo comando, um novo `tmpfs` vazio será montado em `/minha-app`.

## Características

- Armazenado na memória RAM.
- Não persiste após o encerramento do container.
- Existe apenas dentro do ciclo de vida do container.
- Ideal para arquivos temporários, cache e dados sensíveis.
- Não pode ser compartilhado entre containers como um volume.

## Comparação

| Tipo | Persiste após o container? | Local de armazenamento |
|------|-----------------------------|------------------------|
| Volume | ✅ Sim | Disco |
| Bind Mount | ✅ Sim | Diretório do host |
| tmpfs | ❌ Não | Memória RAM |