---
title: Ferramentas para desenvolvedores Go
description: Ferramentas para trabalhar com o SDK do Azure para linguagem Go e serviços do Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: dfa3912ac13e6f6d52d607f9dcc150f3a5b57602
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039498"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="1d33f-103">Ferramentas para desenvolvedores usando o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="1d33f-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="1d33f-104">Para escrever código Go de forma eficaz e fazer com que ele funcione perfeitamente com os serviços do Azure, aqui estão algumas ferramentas recomendadas.</span><span class="sxs-lookup"><span data-stu-id="1d33f-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="1d33f-105">CLI do Azure</span><span class="sxs-lookup"><span data-stu-id="1d33f-105">Azure CLI</span></span>

<span data-ttu-id="1d33f-106">A CLI do Azure fornece uma interface de linha de comando para criar e configurar os recursos do Azure em suas assinaturas.</span><span class="sxs-lookup"><span data-stu-id="1d33f-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="1d33f-107">A CLI pode ajudá-lo a começar a criar recursos do Azure comuns e compartilhados rapidamente, para que você possa se concentrar no uso de serviços mais complexos.</span><span class="sxs-lookup"><span data-stu-id="1d33f-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="1d33f-108">A CLI tem recursos de filtragem e de consulta para que você possa direcionar a saída diretamente para suas ferramentas favoritas de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="1d33f-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="1d33f-109">A CLI está disponível para instalação em seu sistema local, como uma imagem do Docker ou por meio do [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="1d33f-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1d33f-110">Instalar a CLI do Azure</span><span class="sxs-lookup"><span data-stu-id="1d33f-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="1d33f-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1d33f-111">Visual Studio Code</span></span>

<span data-ttu-id="1d33f-112">O Visual Studio Code é um editor leve que tem suporte abrangente para a linguagem Go por meio de extensões.</span><span class="sxs-lookup"><span data-stu-id="1d33f-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="1d33f-113">Essas extensões incluem suporte para recursos como preenchimento automático, modelos `impl`, refatoração e depuração.</span><span class="sxs-lookup"><span data-stu-id="1d33f-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="1d33f-114">O Visual Studio Code também oferece várias extensões para ferramentas de desenvolvedor comuns, como controle de origem e até mesmo oferece extensões para interações diretas com os serviços do Azure.</span><span class="sxs-lookup"><span data-stu-id="1d33f-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="1d33f-115">A Microsoft mantém uma meta-extensão oficial incluindo essas extensões do Azure, incluindo uma interface interativa para a CLI do Azure.</span><span class="sxs-lookup"><span data-stu-id="1d33f-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="1d33f-116">Instalar o Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1d33f-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="1d33f-117">Obter a extensão Go do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1d33f-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="1d33f-118">Obter a extensão de Ferramentas do Azure</span><span class="sxs-lookup"><span data-stu-id="1d33f-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="1d33f-119">CI/CD com o Projeto de DevOps do Azure</span><span class="sxs-lookup"><span data-stu-id="1d33f-119">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="1d33f-120">Com o pipeline de Projeto de DevOps do Azure, você pode configurar uma compilação contínua e implantar em seus aplicativos Go.</span><span class="sxs-lookup"><span data-stu-id="1d33f-120">With the Azure DevOps Project pipeline, you can set up a continuous build and deploy for your Go applications.</span></span> <span data-ttu-id="1d33f-121">Tudo o que você precisa é de um repositório git disponível para poder configurar para implantar e testar diretamente em seus recursos do Azure.</span><span class="sxs-lookup"><span data-stu-id="1d33f-121">All you need is an available git repo, and you can set up to deploy and test directly on your Azure resources.</span></span> <span data-ttu-id="1d33f-122">O pipeline de configuração é fácil de criar e gerenciar e, uma vez que ela é provisionada diretamente no Azure, você pode controlá-lo da mesma maneira que você processa seus outros recursos do Azure.</span><span class="sxs-lookup"><span data-stu-id="1d33f-122">The configuration pipeline is easy to create and manage, and since it's provisioned directly on Azure, you can control it in the same way that you handle your other Azure resources.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1d33f-123">Aprenda como criar um pipeline de CI/CD com os Projetos de DevOps do Azure</span><span class="sxs-lookup"><span data-stu-id="1d33f-123">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="1d33f-124">Gerenciamento de dependência com dep</span><span class="sxs-lookup"><span data-stu-id="1d33f-124">Dependency management with dep</span></span>

<span data-ttu-id="1d33f-125">Há muitas maneiras de gerenciar suas dependências de pacote e fazer vendoring com Go, porque não há nenhuma solução oficial ainda.</span><span class="sxs-lookup"><span data-stu-id="1d33f-125">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="1d33f-126">É a maneira recomendada para executar o gerenciamento com o gerenciador de dependência `dep`.</span><span class="sxs-lookup"><span data-stu-id="1d33f-126">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="1d33f-127">O SDK do Azure para linguagem Go usa dep para seu vendoring e garante obter corretamente dependências para qualquer outro projeto usando o dep.</span><span class="sxs-lookup"><span data-stu-id="1d33f-127">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1d33f-128">Obter o gerente de dependência de dep</span><span class="sxs-lookup"><span data-stu-id="1d33f-128">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
