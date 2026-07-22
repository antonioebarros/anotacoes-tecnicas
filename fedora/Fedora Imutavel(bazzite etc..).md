# Fedora Imutável (Bazzite, Aurora, Bluefin...)

## Filosofia

> O sistema operacional é apenas a base. Ele deve permanecer pequeno, previsível, fácil de atualizar e o mais imutável possível. Tudo o que puder deve ficar isolado do sistema.

---

## Sistema (rpm-ostree)

Instale apenas componentes que realmente precisam se integrar ao sistema operacional.

Exemplos:

- Drivers (NVIDIA, AMD, etc.)
- Firmware
- Docker ou Podman (quando necessário no host)
- Visual Studio Code
- Ferramentas de virtualização (quando precisarem rodar no host)

> **Regra:** se o software precisa interagir diretamente com o kernel, drivers ou integração profunda com o desktop, faz sentido instalá-lo como pacote *layered*.

---

## Flatpak

Aplicações gráficas utilizadas no dia a dia.

Exemplos:

- LibreWolf ou Firefox
- KeePassXC
- Discord
- Spotify
- GIMP
- OBS Studio
- VLC

> **Regra:** sempre que existir um Flatpak oficial ou bem mantido, essa deve ser a primeira opção.

---

## Distrobox

Ambientes completos para desenvolvimento.

Exemplos:

- Python
- Go
- Node.js
- Java
- Rust
- GCC/Clang
- SDKs
- Ferramentas de compilação

Cada Distrobox funciona como um ambiente independente e pode ser recriado a qualquer momento.

---

## Dev Containers

Ambientes específicos para cada projeto.

```text
Projeto API
├── Python 3.13
├── PostgreSQL
└── Redis

Projeto Site
├── Node.js 24
└── Nginx
```

Cada projeto define suas próprias dependências, garantindo isolamento e reprodutibilidade.

---

## Projetos

Os arquivos dos projetos permanecem fora dos containers.

```text
~/Projetos
```

Dessa forma, é possível recriar um Distrobox ou um Dev Container sem perder o código-fonte.

---

## Backups

Fazer backup apenas do que realmente importa:

- Projetos
- Banco de dados do KeePassXC
- Documentos
- Configurações importantes (`.config`, `.ssh`, `.gitconfig`, etc.)

O sistema operacional pode ser reinstalado a qualquer momento.

---

## Resumo

```text
Host (rpm-ostree)
        │
        ├── Drivers
        ├── Firmware
        ├── VS Code
        └── Podman

        ↓

Flatpak
        └── Aplicações gráficas

        ↓

Distrobox
        └── Ambiente de desenvolvimento

        ↓

Dev Containers
        └── Ambiente de cada projeto

        ↓

Projetos
        └── Código-fonte

        ↓

Backups
        └── Apenas os dados importantes
```
