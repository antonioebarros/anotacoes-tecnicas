# Instalar driver nvidia no debian 13(trixie)

### 1. Inicio

Durante a instalação, habilitar os seguintes repositórios:

- `main`

- `contrib`

- `non-free`

- `non-free-firmware`

é possivel habilitar posteriormente editando o `sources.list`.

---

## 2. Verificar o `sources.list`

No Debian 13 (Trixie), o arquivo deve conter algo semelhante a isto:

```bash
sudo nano /etc/apt/sources.list
```

```text
deb http://deb.debian.org/debian trixie main contrib non-free non-free-firmware
deb http://deb.debian.org/debian-security trixie-security main contrib non-free non-free-firmware
deb http://deb.debian.org/debian trixie-updates main contrib non-free non-free-firmware
```

Atualizar a lista de pacotes:

```bash
sudo apt update
```

---

## 3. Atualizar completamente o sistema

Antes de instalar o driver da NVIDIA, atualizar todos os pacotes:

```bash
sudo apt full-upgrade
sudo reboot
```

Isso evita instalar o driver sobre um sistema parcialmente atualizado.

---

## 4. Instalar os pacotes essenciais

```bash
sudo apt install build-essential dkms
```

Esses pacotes serão utilizados para compilar automaticamente o módulo da NVIDIA sempre que um novo kernel for instalado.

---

## 5. Instalar os meta-pacotes do kernel

**Este é um dos passos mais importantes.**

```bash
sudo apt install linux-image-amd64 linux-headers-amd64
```

Os meta-pacotes garantem que futuras atualizações instalem automaticamente o novo kernel e seus respectivos headers, permitindo que o DKMS recompile o driver da NVIDIA.

---

## 6. Instalar o driver NVIDIA

```bash
sudo apt install nvidia-driver
```

O Debian instalará automaticamente todas as dependências necessárias, incluindo o suporte ao DKMS.

---

## 7. Reiniciar o computador

```bash
sudo reboot
```

---

## 8. Verifiquar se o driver foi carregado

Conferindo se tudo está funcionando corretamente:

```bash
nvidia-smi
```

```bash
lsmod | grep nvidia
```

```bash
glxinfo | grep renderer
```

Caso o comando `glxinfo` não exista:

```bash
sudo apt install mesa-utils
```

---

# ⚠️ Observação importante sobre o DKMS

Após a instalação é uma boa ideia verificar se o DKMS registrou corretamente o módulo da NVIDIA.

```bash
dkms status
```

O resultado esperado é algo semelhante a:

```text
nvidia/xxx.xx, 6.x.x-amd64, x86_64: installed
```

### observação: se aparecer **"dkms: comando não encontrado"**:

erro pode ser causado porque mesmo com o pacote `dkms` instalado o executavavel pode estar em `/usr/sbin` que não faz parte da variavel PATH do usuario.

testando com o comando:

```bash
/usr/sbin/dkms status
```

se o comando funcionar normalmente confirma que o DKMS está instalado e que o módulo da NVIDIA foi compilado corretamente para o kernel em uso.

Para evitar digitar o caminho completo todas as vezes, adicionar `/usr/sbin` ao `PATH`:

```bash
echo 'export PATH=$PATH:/usr/sbin' >> ~/.bashrc
source ~/.bashrc
```

---

Também conferir a configuração utilizada pelo `sudo`, *mas muito cuidado ao mexer aqui*:

```bash
sudo visudo
```

Na linha `Defaults secure_path`, `/usr/sbin` deve estár presente.

Abrir o `visudo` apenas para verificar a configuração.

Se `/usr/sbin` já estiver na linha `Defaults secure_path`, não alterar nada.

> **Por que isso é importante?**
> 
> O DKMS é responsável por recompilar automaticamente o módulo proprietário da NVIDIA sempre que um novo kernel é instalado.
> 
> Se ele não estiver funcionando corretamente o sistema pode inicializar sem o driver de vídeo após uma atualização, resultando em baixa resolução falha ao carregar o ambiente gráfico ou até mesmo uma tela preta.
> 
> Por isso, vale a pena confirmar que o DKMS está funcionando antes da próxima atualização do kernel.

---

## 10. Após cada atualização de kernel

Antes de reiniciar o computador, executar:

```bash
dkms status
```

Se o módulo da NVIDIA aparecer compilado para o novo kernel, está tudo certo.

Também é possível conferir se os headers da nova versão foram instalados:

```bash
dpkg -l | grep linux-headers
```

isso reduz bastante as chances de ter problemas após atualizar o kernel.