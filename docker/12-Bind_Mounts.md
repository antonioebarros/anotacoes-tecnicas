# Persistência de Dados - Bind Mounts

Containers são efêmeros: quando um container é removido, seus dados internos também são removidos.

Para manter os dados, o Docker oferece mecanismos de persistência, como:

- **Bind Mounts** (usa um diretório do host).
- **Volumes** (gerenciados pelo Docker).

## Bind Mounts

Um **Bind Mount** conecta um diretório do **host** a um diretório do **container**.

Tudo que for alterado em um lado aparece imediatamente no outro.

## Sintaxe com `-v`

```bash
docker run -it \
  -v /diretorio/no/host:/diretorio/no/container \
  ubuntu:22.04
```

Exemplo:

```bash
docker run -it \
  -v ~/projeto:/app \
  ubuntu:22.04
```

Nesse caso:

- `~/projeto` → diretório do host.
- `/app` → diretório dentro do container.

---

## Sintaxe com `--mount`

```bash
docker run -it \
  --mount type=bind,source=/diretorio/no/host,target=/diretorio/no/container \
  ubuntu:22.04
```

Exemplo:

```bash
docker run -it \
  --mount type=bind,source=$HOME/projeto,target=/app \
  ubuntu:22.04
```

### Diferenças entre `-v` e `--mount`

| `-v` | `--mount` |
|------|-----------|
| Sintaxe mais curta. | Sintaxe mais explícita e legível. |
| Cria automaticamente o diretório do host se ele não existir. | O diretório do host **deve existir**, caso contrário o comando falha. |
| Mais usado em exemplos rápidos. | Recomendado para configurações mais complexas. |

---

## Quando usar Bind Mount?

Ideal durante o desenvolvimento, pois o código pode ser editado no host enquanto o container enxerga as alterações imediatamente.

Exemplo:

```bash
Host
└── projeto/
    ├── app.py
    └── requirements.txt

          │
          │ Bind Mount
          ▼

Container
└── /app
    ├── app.py
    └── requirements.txt
```

As alterações feitas em qualquer um dos lados são refletidas instantaneamente no outro.

> **Diferença entre Docker e Podman**
>
> No Docker, a opção `-v` normalmente cria automaticamente o diretório do host caso ele não exista.
>
> No Podman, o diretório do host deve existir previamente. Caso contrário, o comando falha com um erro semelhante a:
>
> ```text
> Error: statfs /caminho: no such file or directory
> ```
>
> Para evitar esse erro, crie o diretório antes de montar:
>
> ```bash
> mkdir -p ~/projeto
> ```

> **SELinux (Fedora, RHEL, Bazzite...)**
>
> Em sistemas com **SELinux** habilitado, um Bind Mount pode gerar erro de **Permission denied**, mesmo que o diretório exista.
>
> Nesses casos, adicione um rótulo (`label`) ao mount:
>
> ```bash
> podman run -it -v ~/projeto:/app:Z ubuntu:24.04
> ```
>
> ou
>
> ```bash
> podman run -it -v ~/projeto:/app:z ubuntu:24.04
> ```
>
> - `:Z` → uso **exclusivo** por um único container.
> - `:z` → uso **compartilhado** entre vários containers.
>
> Em geral:
>
> - `:Z` é a opção mais comum e segura.
> - `:z` deve ser usada apenas quando vários containers precisarem acessar o mesmo diretório.
