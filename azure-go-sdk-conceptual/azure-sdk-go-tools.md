---
title: Ferramentas para desenvolvedores Go
description: Ferramentas para trabalhar com o SDK do Azure para linguagem Go e serviços do Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 25b46e3a1636c39e261ba11c6f8939d8721cc693
ms.sourcegitcommit: 79d216c6b0442d0f3b3660ff2a34dc8b2049390c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093151"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Ferramentas para desenvolvedores usando o SDK do Azure para linguagem Go

Para escrever código Go de forma eficaz e fazer com que ele funcione perfeitamente com os serviços do Azure, aqui estão algumas ferramentas recomendadas.

## <a name="azure-cli-20"></a>CLI do Azure 2.0

A CLI do Azure 2.0 fornece uma interface de linha de comando para criar e configurar os recursos do Azure em suas assinaturas. A CLI pode ajudá-lo a começar a criar recursos do Azure comuns e compartilhados rapidamente, para que você possa se concentrar no uso de serviços mais complexos. A CLI tem recursos de filtragem e de consulta para que você possa direcionar a saída diretamente para suas ferramentas favoritas de linha de comando. A CLI está disponível para instalação em seu sistema local, como uma imagem do Docker ou por meio do [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

O Visual Studio Code é um editor leve que tem suporte abrangente para a linguagem Go por meio de extensões. Essas extensões incluem suporte para recursos como preenchimento automático, modelos `impl`, refatoração e depuração. O Visual Studio Code também oferece várias extensões para ferramentas de desenvolvedor comuns, como controle de origem e até mesmo oferece extensões para interações diretas com os serviços do Azure. A Microsoft mantém uma meta-extensão oficial incluindo essas extensões do Azure, incluindo uma interface interativa para a CLI do Azure.

* [Instalar o Visual Studio Code](https://code.visualstudio.com/Download)
* [Obter a extensão Go do Visual Studio Code](https://code.visualstudio.com/docs/languages/go)
* [Obter a extensão de Ferramentas do Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a>Gerenciamento de dependência com dep

Há muitas maneiras de gerenciar suas dependências de pacote e fazer vendoring com Go, porque não há nenhuma solução oficial ainda. É a maneira recomendada para executar o gerenciamento com o gerenciador de dependência `dep`. O SDK do Azure para linguagem Go usa dep para seu vendoring e garante obter corretamente dependências para qualquer outro projeto usando o dep.

> [!div class="nextstepaction"]
> [Obter o gerente de dependência de dep](https://github.com/golang/dep)
