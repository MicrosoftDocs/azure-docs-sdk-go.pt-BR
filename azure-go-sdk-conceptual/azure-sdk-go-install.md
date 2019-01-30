---
title: Instale o SDK do Azure para linguagem Go
description: Como instalar, fornecer e configurar o SDK do Azure para linguagem Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 2799e3a6c637036eeaf7b20adf8aa55a8a4ab400
ms.sourcegitcommit: 4db332f5e43a5b43032ff9017805d5fd5a650d86
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55145525"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="52dee-103">Instale o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="52dee-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="52dee-104">Bem-vindo ao SDK do Azure para linguagem Go!</span><span class="sxs-lookup"><span data-stu-id="52dee-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="52dee-105">O SDK permite gerenciar e interagir com os serviços do Azure em seus aplicativos Go.</span><span class="sxs-lookup"><span data-stu-id="52dee-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="52dee-106">Obtenha o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="52dee-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="52dee-107">Alguns serviços do Azure têm seu próprio SDK do Go e não estão incluídos no pacote base de SDK do Azure para linguagem Go.</span><span class="sxs-lookup"><span data-stu-id="52dee-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="52dee-108">A tabela a seguir lista os serviços com seus próprios SDKs e os nomes de pacote.</span><span class="sxs-lookup"><span data-stu-id="52dee-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="52dee-109">Esses pacotes são considerados em versão prévia.</span><span class="sxs-lookup"><span data-stu-id="52dee-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="52dee-110">Serviço</span><span class="sxs-lookup"><span data-stu-id="52dee-110">Service</span></span> | <span data-ttu-id="52dee-111">Pacote</span><span class="sxs-lookup"><span data-stu-id="52dee-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="52dee-112">Armazenamento de Blobs</span><span class="sxs-lookup"><span data-stu-id="52dee-112">Blob Storage</span></span> | [<span data-ttu-id="52dee-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="52dee-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="52dee-114">Armazenamento de Arquivos</span><span class="sxs-lookup"><span data-stu-id="52dee-114">File Storage</span></span> | [<span data-ttu-id="52dee-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="52dee-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="52dee-116">Fila de Armazenamento</span><span class="sxs-lookup"><span data-stu-id="52dee-116">Storage Queue</span></span> | [<span data-ttu-id="52dee-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="52dee-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="52dee-118">Hub de evento</span><span class="sxs-lookup"><span data-stu-id="52dee-118">Event Hub</span></span> | [<span data-ttu-id="52dee-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="52dee-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="52dee-120">Barramento de Serviço</span><span class="sxs-lookup"><span data-stu-id="52dee-120">Service Bus</span></span> | [<span data-ttu-id="52dee-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="52dee-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="52dee-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="52dee-122">Application Insights</span></span> | [<span data-ttu-id="52dee-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="52dee-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="52dee-124">Fornecimento do SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="52dee-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="52dee-125">O SDK do Azure para linguagem Go pode usar a funcionalidade de vendoring pelo [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="52dee-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="52dee-126">Por motivos de estabilidade, é recomendado usar vendoring.</span><span class="sxs-lookup"><span data-stu-id="52dee-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="52dee-127">Para usar `dep` em seu próprio projeto, adicione `github.com/Azure/azure-sdk-for-go` a uma seção `[[constraint]]` do seu `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="52dee-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="52dee-128">Por exemplo, para o vendor na versão `14.0.0`, adicione a seguinte entrada:</span><span class="sxs-lookup"><span data-stu-id="52dee-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="52dee-129">Incluir o SDK do Azure para linguagem Go em seu projeto</span><span class="sxs-lookup"><span data-stu-id="52dee-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="52dee-130">Para usar os serviços do Azure a partir de seu código Go, importe todos os serviços com os quais você interage e os módulos `autorest` necessários.</span><span class="sxs-lookup"><span data-stu-id="52dee-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="52dee-131">Você obterá uma lista completa dos módulos disponíveis do GoDoc para os [serviços disponíveis](https://godoc.org/github.com/Azure/azure-sdk-for-go) e [pacotes AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="52dee-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="52dee-132">Os pacotes mais comuns do `go-autorest` de que você precisa são:</span><span class="sxs-lookup"><span data-stu-id="52dee-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="52dee-133">Pacote</span><span class="sxs-lookup"><span data-stu-id="52dee-133">Package</span></span> | <span data-ttu-id="52dee-134">DESCRIÇÃO</span><span class="sxs-lookup"><span data-stu-id="52dee-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="52dee-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="52dee-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="52dee-136">Objetos para lidar com a autenticação de cliente de serviço</span><span class="sxs-lookup"><span data-stu-id="52dee-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="52dee-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="52dee-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="52dee-138">Constantes para interações com os serviços do Azure</span><span class="sxs-lookup"><span data-stu-id="52dee-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="52dee-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="52dee-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="52dee-140">Mecanismos de autenticação para acessar os serviços do Azure</span><span class="sxs-lookup"><span data-stu-id="52dee-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="52dee-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="52dee-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="52dee-142">Digite os auxiliares de asserção para trabalhar com as estruturas de dados do SDK do Azure</span><span class="sxs-lookup"><span data-stu-id="52dee-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="52dee-143">Os pacotes da linguagem Go e os serviços do Azure possuem controle de versão independente.</span><span class="sxs-lookup"><span data-stu-id="52dee-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="52dee-144">As versões de serviço fazem parte do caminho de importação do módulo, sob o módulo `services`.</span><span class="sxs-lookup"><span data-stu-id="52dee-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="52dee-145">O caminho completo para o módulo é o nome do serviço, seguido da versão no formato `YYYY-MM-DD`, seguido do nome do serviço novamente.</span><span class="sxs-lookup"><span data-stu-id="52dee-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="52dee-146">Por exemplo, para importar a versão `2017-03-30` do serviço de computação:</span><span class="sxs-lookup"><span data-stu-id="52dee-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="52dee-147">É recomendável que você use a versão mais recente de um serviço ao iniciar o desenvolvimento e mantenha-os consistentes.</span><span class="sxs-lookup"><span data-stu-id="52dee-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="52dee-148">Os requisitos de serviço podem ser alterados entre as versões o que poderia interromper o seu código, mesmo se não existem atualizações do SDK da linguagem Go durante esse período.</span><span class="sxs-lookup"><span data-stu-id="52dee-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="52dee-149">Se você precisar de um instantâneo coletivo dos serviços, você também pode selecionar uma versão de perfil.</span><span class="sxs-lookup"><span data-stu-id="52dee-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="52dee-150">Agora, o único perfil bloqueado é a versão `2017-03-09`, que pode não ter os recursos mais recentes dos serviços.</span><span class="sxs-lookup"><span data-stu-id="52dee-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="52dee-151">Os perfis estão localizados no módulo `profiles`, com a versão no formato `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="52dee-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="52dee-152">Os serviços são agrupados em sua versão de perfil.</span><span class="sxs-lookup"><span data-stu-id="52dee-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="52dee-153">Por exemplo, para importar o módulo de gerenciamento de recursos do Azure a partir do perfil `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="52dee-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="52dee-154">Os perfis `preview` e `latest` também estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="52dee-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="52dee-155">Porém, não é recomendado usá-los.</span><span class="sxs-lookup"><span data-stu-id="52dee-155">Using them is not recommended.</span></span> <span data-ttu-id="52dee-156">Esses perfis são versões progressivas, e o comportamento do serviço poderá mudar a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="52dee-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52dee-157">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="52dee-157">Next steps</span></span>

<span data-ttu-id="52dee-158">Para começar a usar o SDK do Azure para linguagem Go, experimente um início rápido.</span><span class="sxs-lookup"><span data-stu-id="52dee-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="52dee-159">Implantar uma máquina virtual no Azure a partir de um modelo</span><span class="sxs-lookup"><span data-stu-id="52dee-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="52dee-160">Transferir objetos para o Armazenamento de Blobs do Azure usando o SDK de Blobs do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="52dee-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="52dee-161">Conectar-se ao Banco de Dados do Azure para PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="52dee-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="52dee-162">Se você quiser começar com outros serviços no SDK Go imediatamente, dê uma olhada em alguns códigos de exemplo disponíveis.</span><span class="sxs-lookup"><span data-stu-id="52dee-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="52dee-163">Autenticar com os serviços do Azure</span><span class="sxs-lookup"><span data-stu-id="52dee-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [<span data-ttu-id="52dee-164">Implantar novas máquinas virtuais com autenticação SSH</span><span class="sxs-lookup"><span data-stu-id="52dee-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="52dee-165">Implantar uma imagem de contêiner nas Instâncias de Contêiner do Azure</span><span class="sxs-lookup"><span data-stu-id="52dee-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="52dee-166">Criar um cluster no Serviço Kubernetes do Azure</span><span class="sxs-lookup"><span data-stu-id="52dee-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute)
* [<span data-ttu-id="52dee-167">Trabalhar com serviços de armazenamento do Azure</span><span class="sxs-lookup"><span data-stu-id="52dee-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="52dee-168">Todos os exemplos do SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="52dee-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
