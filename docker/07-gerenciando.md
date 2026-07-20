# Gerenciamento de containers

O Docker oferece diferentes formas de executar alguns comandos por compatibilidade com versões antigas e por possuir aliases.

Exemplos equivalentes:

```bash
docker ps

docker container ls

docker container ps

docker container list
```

Todos listam os **containers em execução**.

> **Nota:** Nesta anotação será utilizada a sintaxe organizada do Docker:
> 
> ```text
> docker <recurso> <ação>
> ```
> 
> Exemplo:
> 
> ```bash
> docker container ls
> ```

---

## Listando containers

```bash
docker container ls
```

Lista apenas os containers em execução.

```bash
docker container ls -a
```

Lista todos os containers, incluindo os que estão parados.

---

## Controle de containers

### Iniciar

```bash
docker container start <container>
```

Inicia um container parado.

### Parar

```bash
docker container stop <container>
```

Encerra um container em execução.

### Reiniciar

```bash
docker container restart <container>
```

Reinicia um container.

### Remover

```bash
docker container rm <container>
```

Remove um container parado.

### Remover todos os containers parados

```bash
docker container prune
```

Remove todos os containers que estão no estado **Exited**.

---

## Inspeção e diagnóstico

### Logs

```bash
docker container logs <container>
```

Exibe a saída padrão (**stdout**) e a saída de erros (**stderr**) do container.

### Inspecionar

```bash
docker container inspect <container>
```

Exibe informações detalhadas do container em formato JSON, como:

- configuração;
- estado;
- rede;
- volumes;
- variáveis de ambiente;
- entre outras.

---

## Executando comandos

```bash
docker container exec <container> <comando>
```

Executa um comando dentro de um container em execução.

Exemplo:

```bash
docker container exec nginx ls /
```

Lista os arquivos da raiz do container.

### Terminal interativo

É comum utilizar `-it` para abrir um shell dentro do container.

Imagens baseadas em Debian ou Ubuntu:

```bash
docker container exec -it nginx bash
```

Imagens menores, como Alpine:

```bash
docker container exec -it alpine sh
```

Isso permite executar comandos manualmente no container.
