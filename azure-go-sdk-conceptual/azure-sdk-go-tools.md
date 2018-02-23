---
title: Ferramentas para desenvolvedores Go
description: "Ferramentas para trabalhar com o SDK do Azure para linguagem Go e serviços do Azure"
keywords: "azure, go, golang, azure, visual studio, código do visual studio"
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 4753775e608b39c6da43d64fd08c1532e03d5810
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/15/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="055db-104">Ferramentas para desenvolvedores usando o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="055db-104">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="055db-105">Para escrever código Go de forma eficaz e fazer com que ele funcione perfeitamente com os serviços do Azure, aqui estão algumas ferramentas recomendadas.</span><span class="sxs-lookup"><span data-stu-id="055db-105">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="055db-106">CLI do Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="055db-106">Azure CLI 2.0</span></span>

<span data-ttu-id="055db-107">A CLI do Azure 2.0 fornece uma interface de linha de comando para criar e configurar os recursos do Azure em suas assinaturas.</span><span class="sxs-lookup"><span data-stu-id="055db-107">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="055db-108">A CLI pode ajudá-lo a começar a criar recursos do Azure comuns e compartilhados rapidamente, para que você possa se concentrar no uso de serviços mais complexos.</span><span class="sxs-lookup"><span data-stu-id="055db-108">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="055db-109">A CLI tem recursos de filtragem e de consulta para que você possa direcionar a saída diretamente para suas ferramentas favoritas de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="055db-109">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="055db-110">A CLI está disponível para instalação em seu sistema local, como uma imagem do Docker ou por meio do [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="055db-110">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="055db-111">Instalar a CLI 2.0 do Azure</span><span class="sxs-lookup"><span data-stu-id="055db-111">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="055db-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="055db-112">Visual Studio Code</span></span>

<span data-ttu-id="055db-113">O Visual Studio Code é um editor leve que tem suporte abrangente para a linguagem Go por meio de extensões.</span><span class="sxs-lookup"><span data-stu-id="055db-113">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="055db-114">Essas extensões incluem suporte para recursos como preenchimento automático, modelos `impl`, refatoração e depuração.</span><span class="sxs-lookup"><span data-stu-id="055db-114">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="055db-115">O Visual Studio Code também oferece várias extensões para ferramentas de desenvolvedor comuns, como controle de origem e até mesmo oferece extensões para interações diretas com os serviços do Azure.</span><span class="sxs-lookup"><span data-stu-id="055db-115">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="055db-116">A Microsoft mantém uma meta-extensão oficial incluindo essas extensões do Azure, incluindo uma interface interativa para a CLI do Azure.</span><span class="sxs-lookup"><span data-stu-id="055db-116">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="055db-117">Instalar o Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="055db-117">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="055db-118">Obter a extensão Go do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="055db-118">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="055db-119">Obter a extensão de Ferramentas do Azure</span><span class="sxs-lookup"><span data-stu-id="055db-119">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="055db-120">Gerenciamento de dependência com dep</span><span class="sxs-lookup"><span data-stu-id="055db-120">Dependency management with dep</span></span>

<span data-ttu-id="055db-121">Há muitas maneiras de gerenciar suas dependências de pacote e fazer vendoring com Go, porque não há nenhuma solução oficial ainda.</span><span class="sxs-lookup"><span data-stu-id="055db-121">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="055db-122">É a maneira recomendada para executar o gerenciamento com o gerenciador de dependência `dep`.</span><span class="sxs-lookup"><span data-stu-id="055db-122">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="055db-123">O SDK do Azure para linguagem Go usa dep para seu vendoring e garante obter corretamente dependências para qualquer outro projeto usando o dep.</span><span class="sxs-lookup"><span data-stu-id="055db-123">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="055db-124">Obter o gerente de dependência de dep</span><span class="sxs-lookup"><span data-stu-id="055db-124">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="055db-125">Telemetria com o Application Insights</span><span class="sxs-lookup"><span data-stu-id="055db-125">Telemetry with Application Insights</span></span>

<span data-ttu-id="055db-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) é um produto de análise que permite que você colete facilmente informações de telemetria de aplicativos e que se integra com o ecossistema do Azure, Visual Studio Team Services e GitHub.</span><span class="sxs-lookup"><span data-stu-id="055db-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="055db-127">Ele pode ser usado em muitos aplicativos, e a Microsoft oferece um SDK Go para trabalhar com o Application Insights.</span><span class="sxs-lookup"><span data-stu-id="055db-127">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="055db-128">Obtenha o Application Insights para o SDK Go</span><span class="sxs-lookup"><span data-stu-id="055db-128">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 
