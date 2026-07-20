## Docker start

## `docker start`

Inicia um container existente que está parado.

Diferentemente do `docker run`, **não cria um novo container**, apenas inicia um container já existente.

Sintaxe:

```bash
docker start [OPÇÕES] CONTAINER
```

Exemplo:

```bash
docker start meuDebian
```

> **Importante:** o container é iniciado com o mesmo comando definido durante sua criação. O `docker start` não permite alterar esse comando.

---

## Exemplo

Criando um container:

```bash
docker run --name meuDebian debian bash --version
```

Após a execução, o container ficará no estado **Exited**.

Para executá-lo novamente:

```bash
docker start meuDebian
```

Nesse caso, o Docker executará novamente:

```bash
bash --version
```

Como esse comando termina imediatamente, o container voltará ao estado **Exited**.

---

## Forma explícita

Também é possível utilizar o comando na forma completa:

```bash
docker container start -ai meuDebian
```

### `-a` (`--attach`)

Conecta o terminal à saída padrão (**stdout**) e à saída de erros (**stderr**) do container.

### `-i` (`--interactive`)

Mantém a entrada padrão (**STDIN**) aberta, permitindo interação com o processo principal.

### `-ai`

Combinação de `-a` e `-i`.

É comum utilizar essa opção para retomar containers criados para executar um shell interativo.

Exemplo:

```bash
docker run --name meuDebian -it debian bash
```

Após sair do Bash com:

```bash
exit
```

é possível retornar ao mesmo shell:

```bash
docker container start -ai meuDebian
```

---

## Relação com outros comandos

| Comando        | Cria um container | Inicia um container existente |
| -------------- |:-----------------:|:-----------------------------:|
| `docker run`   | ✅                 | ✅                             |
| `docker start` | ❌                 | ✅                             |
