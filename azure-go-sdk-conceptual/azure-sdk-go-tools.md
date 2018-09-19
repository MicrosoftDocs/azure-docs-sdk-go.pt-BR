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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="cd96e-103">Ferramentas para desenvolvedores usando o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="cd96e-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="cd96e-104">Para escrever código Go de forma eficaz e fazer com que ele funcione perfeitamente com os serviços do Azure, aqui estão algumas ferramentas recomendadas.</span><span class="sxs-lookup"><span data-stu-id="cd96e-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="cd96e-105">CLI do Azure</span><span class="sxs-lookup"><span data-stu-id="cd96e-105">Azure CLI</span></span>

<span data-ttu-id="cd96e-106">A CLI do Azure fornece uma interface de linha de comando para criar e configurar os recursos do Azure em suas assinaturas.</span><span class="sxs-lookup"><span data-stu-id="cd96e-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="cd96e-107">A CLI pode ajudá-lo a começar a criar recursos do Azure comuns e compartilhados rapidamente, para que você possa se concentrar no uso de serviços mais complexos.</span><span class="sxs-lookup"><span data-stu-id="cd96e-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="cd96e-108">A CLI tem recursos de filtragem e de consulta para que você possa direcionar a saída diretamente para suas ferramentas favoritas de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="cd96e-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="cd96e-109">A CLI está disponível para instalação em seu sistema local, como uma imagem do Docker ou por meio do [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="cd96e-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cd96e-110">Instalar a CLI do Azure</span><span class="sxs-lookup"><span data-stu-id="cd96e-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="cd96e-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd96e-111">Visual Studio Code</span></span>

<span data-ttu-id="cd96e-112">Visual Studio Code é um editor leve que oferece suporte à linguagem Go.</span><span class="sxs-lookup"><span data-stu-id="cd96e-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="cd96e-113">Essa extensão oferece suporte para recursos como preenchimento automático, modelos `impl`, refatoração e depuração.</span><span class="sxs-lookup"><span data-stu-id="cd96e-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="cd96e-114">O Visual Studio Code também oferece suporte para acesso no editor ao controle do código-fonte e extensões para trabalhar com os serviços do Azure.</span><span class="sxs-lookup"><span data-stu-id="cd96e-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="cd96e-115">Instalar o Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd96e-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="cd96e-116">Obter a extensão Go do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd96e-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="cd96e-117">Obter a extensão das Ferramentas do Azure do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd96e-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="cd96e-118">CI/CD com o Projeto de DevOps do Azure</span><span class="sxs-lookup"><span data-stu-id="cd96e-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="cd96e-119">Os pipelines de projeto do Azure DevOps permitem que você configure um sistema de integração contínua para seus aplicativos Go.</span><span class="sxs-lookup"><span data-stu-id="cd96e-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="cd96e-120">Tudo o que você precisa é um repositório git, e você pode implantar e testar diretamente no Azure.</span><span class="sxs-lookup"><span data-stu-id="cd96e-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cd96e-121">Aprenda como criar um pipeline de CI/CD com os Projetos de DevOps do Azure</span><span class="sxs-lookup"><span data-stu-id="cd96e-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="cd96e-122">Gerenciamento de dependência com dep</span><span class="sxs-lookup"><span data-stu-id="cd96e-122">Dependency management with dep</span></span>

<span data-ttu-id="cd96e-123">O SDK do Azure para linguagem Go usa dep para gerenciamento de dependência.</span><span class="sxs-lookup"><span data-stu-id="cd96e-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="cd96e-124">O comando dep permite efetuar pull e requisitos do fornecedor para seu aplicativo Go, evitando conflitos de versão e garantindo que seu projeto funcione corretamente.</span><span class="sxs-lookup"><span data-stu-id="cd96e-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cd96e-125">Obter o gerente de dependência de dep</span><span class="sxs-lookup"><span data-stu-id="cd96e-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
