---
title: Ferramentas para desenvolvedores usando o SDK do Azure para linguagem Go
description: Ferramentas para trabalhar com o SDK do Azure para linguagem Go e serviços do Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059196"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Ferramentas para desenvolvedores usando o SDK do Azure para linguagem Go

Para escrever código Go de forma eficaz e fazer com que ele funcione perfeitamente com os serviços do Azure, aqui estão algumas ferramentas recomendadas.

## <a name="azure-cli"></a>CLI do Azure

A CLI do Azure fornece uma interface de linha de comando para criar e configurar os recursos do Azure em suas assinaturas. A CLI pode ajudá-lo a começar a criar recursos do Azure comuns e compartilhados rapidamente, para que você possa se concentrar no uso de serviços mais complexos. A CLI tem recursos de filtragem e de consulta para que você possa direcionar a saída diretamente para suas ferramentas favoritas de linha de comando. A CLI está disponível para instalação em seu sistema local, como uma imagem do Docker ou por meio do [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Instalar a CLI do Azure](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code é um editor leve que oferece suporte à linguagem Go. Essa extensão oferece suporte para recursos como preenchimento automático, modelos `impl`, refatoração e depuração. O Visual Studio Code também oferece suporte para acesso no editor ao controle do código-fonte e extensões para trabalhar com os serviços do Azure.

* [Instalar o Visual Studio Code](https://code.visualstudio.com/Download)
* [Obter a extensão Go do Visual Studio Code](https://code.visualstudio.com/docs/languages/go)
* [Obter a extensão das Ferramentas do Azure do Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD com o Projeto de DevOps do Azure

Os pipelines de projeto do Azure DevOps permitem que você configure um sistema de integração contínua para seus aplicativos Go. Tudo o que você precisa é um repositório git, e você pode implantar e testar diretamente no Azure.

> [!div class="nextstepaction"]
> [Aprenda como criar um pipeline de CI/CD com os Projetos de DevOps do Azure](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Gerenciamento de dependência com dep

O SDK do Azure para linguagem Go usa dep para gerenciamento de dependência. O comando dep permite efetuar pull e requisitos do fornecedor para seu aplicativo Go, evitando conflitos de versão e garantindo que seu projeto funcione corretamente.

> [!div class="nextstepaction"]
> [Obter o gerente de dependência de dep](https://github.com/golang/dep)
