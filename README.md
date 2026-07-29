ICA Lab - Terraform com Docker

📖 Sobre o projeto

Este projeto demonstra uma implementação simples utilizando Terraform para automatizar o gerenciamento de recursos Docker.

O objetivo é apresentar os conceitos básicos de Infraestrutura como Código (IaC), utilizando o provider Docker para realizar o provisionamento de imagens e containers de forma declarativa.

🎯 Objetivo

O arquivo main.tf define toda a infraestrutura necessária para:

Configurar o provider Docker;
Baixar automaticamente a imagem oficial do Ubuntu 22.04;
Criar um container baseado nessa imagem;
Manter o container em execução continuamente.

Este exemplo serve como laboratório para estudos de Terraform e Docker.

📂 Estrutura do Projeto
.
├── main.tf
└── README.md

🛠 Tecnologias Utilizadas

Terraform
Docker
Provider Docker (kreuzwerker/docker)
Ubuntu 22.04

📄 Explicação do arquivo main.tf

1. Configuração do Terraform

terraform {
    required_providers{
        docker = {
            source = "kreuzwerker/docker"
            version = "~> 3.0"
        }
    }
}

Este bloco informa ao Terraform qual provider será utilizado.

Neste caso:

Provider: Docker
Origem: kreuzwerker/docker
Versão compatível: qualquer versão 3.x

O Terraform fará o download automaticamente do provider durante a execução do comando:

terraform init

2. Configuração do Provider

provider "docker" {}

Este bloco configura a comunicação entre o Terraform e o Docker instalado na máquina.

Como nenhuma configuração adicional foi definida, o provider utilizará a configuração padrão do Docker local.

3. Download da imagem Ubuntu

resource "docker_image" "ubuntu" {
    name = "ubuntu:22.04"
}

Este recurso possui a função de baixar a imagem oficial do Ubuntu na versão 22.04.

Caso a imagem já exista localmente, o Terraform reutilizará a imagem existente.

Após a execução será equivalente ao comando:

docker pull ubuntu:22.04

4. Criação do Container

resource "docker_container" "servidor" {
    name  = "ubuntu-server"
    image = docker_image.ubuntu.image_id
    command = ["sleep", "infinity"]
}

Este recurso cria um container Docker utilizando a imagem baixada anteriormente.

Nome do container
name = "ubuntu-server"

Define o nome do container.

Imagem utilizada
image = docker_image.ubuntu.image_id

O container utiliza a imagem criada pelo recurso:

docker_image.ubuntu

Essa referência cria automaticamente uma dependência entre os recursos, garantindo que a imagem seja baixada antes da criação do container.

Comando executado
command = ["sleep", "infinity"]

O Ubuntu normalmente encerra sua execução imediatamente caso nenhum processo permaneça ativo.

O comando:

sleep infinity

mantém o processo em execução indefinidamente, evitando que o container seja finalizado.

🔄 Fluxo de execução

Terraform Init
        │
        ▼
Download do Provider Docker
        │
        ▼
Download da imagem Ubuntu 22.04
        │
        ▼
Criação do container
        │
        ▼
Container permanece ativo

▶️ Como executar

Inicializar o projeto
terraform init
Visualizar o plano
terraform plan
Criar a infraestrutura
terraform apply

Confirme a execução digitando:
yes

Verificar o container
docker ps

Saída esperada:
ubuntu-server

Acessar o container:
docker exec -it ubuntu-server bash

Remover a infraestrutura:
terraform destroy

📌 Recursos criados

O projeto provisiona:

Recurso	Descrição

Docker Image	Ubuntu 22.04
Docker Container	ubuntu-server

🔗 Dependência entre recursos

O Terraform identifica automaticamente que:

docker_container.servidor
        │
        ▼
docker_image.ubuntu

Dessa forma, a imagem sempre será criada antes do container.

📚 Conceitos abordados

Este projeto demonstra os seguintes conceitos do Terraform:

Providers
Resources
Dependências automáticas
Infraestrutura como Código (IaC)
Gerenciamento declarativo de containers Docker
Referência entre recursos
Provisionamento automatizado

🚀 Possíveis melhorias

Como evolução deste laboratório, é possível adicionar:

Exposição de portas (ports)
Montagem de volumes (volumes)
Variáveis (variables.tf)
Outputs (outputs.tf)
Redes Docker (docker_network)
Múltiplos containers
Docker Compose com Terraform
Integração com imagens personalizadas

👨‍💻 Autor

Projeto desenvolvido por Rhoney Seabra para fins de estudo e prática de Terraform e Docker, demonstrando os fundamentos de Infraestrutura como Código (IaC)