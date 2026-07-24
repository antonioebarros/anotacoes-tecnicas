# Volumes

Volumes são a forma recomendada de armazenar dados persistentes em containers. Eles continuam existindo mesmo após o container ser removido.

---

## Criar um volume

```bash
docker volume create <nome-do-volume>
```

---

## Listar volumes

```bash
docker volume ls
```

---

## Utilizar um volume

### Sintaxe completa (`--mount`)

```bash
docker run -it \
  --mount source=<nome-do-volume>,target=<diretorio-no-container> \
  <imagem>
```

### Sintaxe abreviada (`-v`)

```bash
docker run -it \
  -v <nome-do-volume>:<diretorio-no-container> \
  <imagem>
```

> **Observação:** `--mount` é a sintaxe mais explícita e recomendada pela documentação do Docker. A opção `-v` é mais curta e muito utilizada no dia a dia.

---

## Inspecionar um volume

```bash
docker volume inspect <nome-do-volume>
```

Esse comando mostra informações como:

- Nome do volume
- Driver utilizado
- Ponto de montagem (`Mountpoint`)
- Labels
- Escopo (`Scope`)

---

## Criação automática

Não é obrigatório criar um volume antes de utilizá-lo.

Se o volume informado em `source=` (ou em `-v`) não existir, o Docker o criará automaticamente.

Exemplo:

```bash
docker run -it \
  --mount source=meu-volume,target=/dados \
  ubuntu
```

ou

```bash
docker run -it \
  -v meu-volume:/dados \
  ubuntu
```

Se `meu-volume` ainda não existir, ele será criado automaticamente antes da inicialização do container.