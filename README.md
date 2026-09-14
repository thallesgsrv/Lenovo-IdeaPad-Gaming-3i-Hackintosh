# Lenovo IdeaPad Gaming 3i — Hackintosh

![Platform](https://img.shields.io/badge/platform-Hackintosh-lightgrey)
![macOS](https://img.shields.io/badge/macOS-Ventura-blue)
![Bootloader](https://img.shields.io/badge/bootloader-OpenCore-informational)
![Status](https://img.shields.io/badge/status-experimental-orange)
![License](https://img.shields.io/badge/license-See%20LICENSE-lightgrey)

EFI e documentação para Hackintosh no **Lenovo IdeaPad Gaming 3i 15IMH05 (82CG)**, utilizando **OpenCore** e **macOS Ventura**.

> ⚠️ **Aviso:** este repositório foi criado com base na configuração e experiência de instalação deste equipamento específico. Hackintosh não é oficialmente suportado pela Apple e os resultados podem variar entre máquinas, mesmo com hardware semelhante. Use por sua conta e risco.

---

## Índice

- [Hardware](#hardware)
- [Estrutura do repositório](#estrutura-do-repositório)
- [EFI](#efi)
- [ACPI](#acpi)
- [Instalação](#instalação)
- [Compatibilidade](#compatibilidade)
- [Áudio](#áudio)
- [Wi-Fi e Bluetooth](#wi-fi-e-bluetooth)
- [Ethernet](#ethernet)
- [NVIDIA GTX 1650](#nvidia-gtx-1650)
- [Troubleshooting](#troubleshooting)
- [Atualizações](#atualizações)
- [Referências](#referências)
- [Informações do projeto](#informações-do-projeto)

---

## Hardware

| Componente            | Especificação                           |
| ---------------------- | ---------------------------------------- |
| **Modelo**              | Lenovo IdeaPad Gaming 3i 15IMH05 / 82CG |
| **CPU**                 | Intel Core i7-10750H                    |
| **GPU integrada**       | Intel UHD Graphics 630                  |
| **GPU dedicada**        | NVIDIA GeForce GTX 1650                 |
| **RAM**                 | 16 GB DDR4 2933 MHz                     |
| **Wi-Fi / Bluetooth**   | Intel Wi-Fi 6 AX201                     |
| **Ethernet**            | Realtek RTL8111H                        |
| **Áudio**               | Realtek ALC257                          |
| **Bootloader**          | OpenCore                                |
| **macOS**               | Ventura                                 |

### Sobre a GPU

A **Intel UHD Graphics 630** é utilizada como GPU principal no macOS.

A **NVIDIA GTX 1650** não possui suporte adequado nas versões modernas do macOS utilizadas neste projeto e, portanto, é desativada através de configuração ACPI. No Windows, a GTX 1650 permanece disponível normalmente.

---

## Estrutura do repositório

```text
.
├── EFI/
│   └── OC/
│       ├── ACPI/
│       ├── Drivers/
│       ├── Kexts/
│       ├── Resources/
│       ├── Tools/
│       ├── config.plist
│       └── OpenCore.efi
│
├── README.md
└── LICENSE
```

A pasta `EFI` contém os arquivos necessários para a inicialização do OpenCore.

---

## EFI

A EFI deste repositório utiliza componentes específicos, selecionados de acordo com o hardware presente nesta máquina.

### Kexts principais

| Categoria       | Kexts                                                                 |
| ---------------- | ---------------------------------------------------------------------- |
| Núcleo            | `Lilu`, `WhateverGreen`, `VirtualSMC`                                  |
| Sensores/SMC      | `SMCProcessor`, `SMCSuperIO`, `SMCBatteryManager`                      |
| Áudio             | `AppleALC`                                                              |
| Wi-Fi/Bluetooth   | `AirportItlwm`, `IntelBluetoothFirmware`, `BlueToolFixup`               |
| Rede cabeada      | `RealtekRTL8111`                                                        |
| Entrada/Trackpad  | `VoodooI2C`, `VoodooI2CHID`, `VoodooPS2Controller`                      |
| USB               | `USBMap`                                                                |
| Outros            | `CpuTscSync`, `BrightnessKeys`                                          |

---

## ACPI

A EFI utiliza SSDTs para corrigir e/ou adaptar componentes do firmware para funcionamento no macOS:

```text
SSDT-AWAC.aml
SSDT-DISABLE-NVIDIA.aml
SSDT-EC-USBX.aml
SSDT-GPRW.aml
SSDT-HPET.aml
SSDT-I2C.aml
SSDT-MCHC.aml
SSDT-PLUG.aml
SSDT-PNLF-CFL.aml
```

### NVIDIA

`SSDT-DISABLE-NVIDIA.aml` desativa a GTX 1650 durante a inicialização do macOS, permitindo que o sistema utilize a Intel UHD Graphics 630.

---

## Instalação

### 1. Preparação

Antes de começar, tenha em mãos:

- Pendrive de pelo menos 16 GB
- Computador com acesso à internet
- Backup dos arquivos importantes
- EFI deste repositório
- Imagem/instalador do macOS compatível

> **Importante:** este projeto foi instalado utilizando uma imagem do **Olarila**. Este README documenta o método utilizado neste equipamento e não afirma que a instalação foi feita com `macrecovery.py`.

### 2. BIOS

Antes da instalação, entre na BIOS/UEFI e verifique as configurações relacionadas ao boot:

- Boot em **UEFI**
- **Secure Boot** desabilitado
- Recursos de inicialização rápida desabilitados quando necessário
- Boot de dispositivos USB em modo UEFI habilitado

> As opções disponíveis podem variar conforme a versão da BIOS.

### 3. Preparação do pendrive

Prepare o pendrive com uma imagem compatível do macOS e copie a pasta `EFI` para a partição EFI do pendrive:

```text
EFI/
└── OC/
    ├── ACPI/
    ├── Drivers/
    ├── Kexts/
    ├── Resources/
    └── config.plist
```

### 4. Primeiro boot

1. Conecte o pendrive e ligue o notebook.
2. Abra o menu de boot da Lenovo.
3. Selecione o dispositivo USB em modo UEFI.
4. O OpenCore deverá aparecer.
5. Selecione o instalador do macOS.
6. Prossiga com a instalação normalmente.

> Durante a instalação, o computador pode reiniciar várias vezes. Após cada reinicialização, selecione novamente a entrada correspondente à instalação do macOS no OpenCore.

### 5. Após instalar

Copie a EFI para a partição EFI do SSD interno — não dependa do pendrive para inicializar o sistema:

```text
EFI/
└── OC/
    ├── ACPI/
    ├── Drivers/
    ├── Kexts/
    ├── Resources/
    └── config.plist
```

---

## Compatibilidade

| Componente             | Status                          |
| ------------------------ | --------------------------------- |
| Intel UHD 630             | ✅ Funcionando                    |
| NVIDIA GTX 1650           | 🚫 Desativada                     |
| Wi-Fi Intel AX201         | ✅ Funcionando                    |
| Bluetooth Intel AX201     | ✅ Funcionando                    |
| Ethernet RTL8111H         | ✅ Funcionando                    |
| Áudio Realtek ALC257      | ✅ Funcionando                    |
| Microfone                 | ✅ Funcionando                    |
| USB                       | ✅ Funcionando                    |
| Teclado                   | ✅ Funcionando                    |
| Trackpad                  | ⚠️ Pode apresentar limitações     |
| HDMI                      | ⚠️ Pode apresentar limitações     |
| Sleep                     | ⚠️ Pode apresentar limitações     |
| AirDrop                   | ⚠️ Limitado com hardware Intel    |

> Os estados acima representam o funcionamento observado/configurado neste projeto. Algumas funções podem depender da versão do macOS, BIOS, kexts e configuração utilizada.

---

## Áudio

O áudio é configurado através de `AppleALC.kext`, utilizando o codec **Realtek ALC257**.

Caso o áudio não funcione em uma instalação diferente, verifique o `layout-id` no `config.plist`.

---

## Wi-Fi e Bluetooth

O notebook utiliza **Intel Wi-Fi 6 AX201**, com suporte fornecido por kexts da comunidade:

```text
AirportItlwm.kext
IntelBluetoothFirmware.kext
BlueToolFixup.kext
```

> O funcionamento e os recursos disponíveis podem variar em relação aos Macs com hardware Apple nativo.

---

## Ethernet

A placa de rede cabeada **Realtek RTL8111H** é suportada por `RealtekRTL8111.kext`.

---

## NVIDIA GTX 1650

A GTX 1650 é desativada no macOS, pois GPUs NVIDIA desta geração não possuem suporte adequado nas versões modernas do macOS utilizadas neste projeto. A máquina utiliza a **Intel UHD Graphics 630** como GPU principal no macOS. No Windows, a GTX 1650 continua disponível normalmente.

---

## Troubleshooting

<details>
<summary><strong>O OpenCore não aparece</strong></summary>

Verifique:
- Se a pasta `EFI` está na partição EFI correta
- Se `BOOT/BOOTx64.efi` está presente
- Se o boot está configurado como UEFI
- Se o Secure Boot está desativado
- Se a BIOS permite boot pelo dispositivo utilizado

</details>

<details>
<summary><strong>O macOS trava durante o boot</strong></summary>

Verifique, em ordem:
1. `config.plist`
2. Kexts
3. SSDTs
4. Versão do OpenCore
5. Argumentos de boot (`boot-args`)

Para diagnosticar, pode ser necessário habilitar o modo verbose.

</details>

<details>
<summary><strong>A GTX 1650 aparece no sistema</strong></summary>

Verifique se `SSDT-DISABLE-NVIDIA.aml` está presente e habilitado no `config.plist`.

</details>

<details>
<summary><strong>Wi-Fi não funciona</strong></summary>

Verifique `AirportItlwm.kext` e confirme se a versão do kext corresponde à versão do macOS instalada.

</details>

<details>
<summary><strong>Bluetooth não funciona</strong></summary>

Verifique se `IntelBluetoothFirmware.kext` e `BlueToolFixup.kext` estão presentes.

</details>

<details>
<summary><strong>Áudio não funciona</strong></summary>

Verifique `AppleALC.kext` e o `layout-id` configurado no `config.plist`.

</details>

---

## Atualizações

Antes de atualizar o macOS ou substituir kexts:

1. Faça backup da EFI atual.
2. Verifique a compatibilidade da nova versão.
3. Atualize os kexts necessários.
4. Atualize o OpenCore quando necessário.
5. Teste primeiro utilizando um backup funcional.

> Não substitua todos os componentes da EFI indiscriminadamente.

---

## Referências

- [Dortania OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg)
- [Lilu](https://github.com/acidanthera/Lilu)
- [WhateverGreen](https://github.com/acidanthera/WhateverGreen)
- [AppleALC](https://github.com/acidanthera/AppleALC)
- [VirtualSMC](https://github.com/acidanthera/VirtualSMC)
- [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X)
- [OpenIntelWireless](https://github.com/OpenIntelWireless)

---

## Informações do projeto

| Item          | Valor                                    |
| -------------- | ------------------------------------------ |
| **Modelo**      | Lenovo IdeaPad Gaming 3i 15IMH05 / 82CG   |
| **CPU**         | Intel Core i7-10750H                      |
| **GPU**         | Intel UHD Graphics 630                    |
| **dGPU**        | NVIDIA GTX 1650                           |
| **RAM**         | 16 GB DDR4                                |
| **Wi-Fi**       | Intel AX201                               |
| **Ethernet**    | Realtek RTL8111H                          |
| **Áudio**       | Realtek ALC257                            |
| **Bootloader**  | OpenCore                                  |
| **macOS**       | Ventura                                   |

---

## Aviso legal

Este projeto é destinado a fins educacionais e experimentais. Hackintosh envolve modificações no processo de inicialização e utilização de componentes não suportados oficialmente pela Apple. Não há garantia de funcionamento em outros modelos ou configurações.

**Use por sua conta e risco.**