## **Projeto IaC: Deploy de um Web Server na AWS com Terraform e Ansible** ##


Este repositório contém um projeto de Infraestrutura como Código (IaC) que automatiza o provisionamento de um servidor web na nuvem da AWS. O processo utiliza o Terraform para criar a infraestrutura e o Ansible para configurar o servidor e fazer o deploy de uma aplicação web simples.

---

**Tecnologias Utilizadas**

**Terraform**: Para provisionar a infraestrutura na AWS (EC2, Security Groups, Key Pair).

**AWS**: Provedor de nuvem onde a infraestrutura é criada.

**Ansible**: Para o gerenciamento de configuração do servidor (instalação do Nginx, Git) e o deploy da aplicação.

**Git**: Para o versionamento do código e clone do site de exemplo.

---

O fluxo de trabalho segue os seguintes passos:

O Terraform é executado para criar os recursos na AWS, incluindo uma instância EC2.

Após a criação, o IP público da instância EC2 é obtido.

O Ansible utiliza o IP e uma chave SSH para se conectar à instância.

O playbook do Ansible atualiza o servidor, instala o Nginx e clona um repositório com o site de exemplo, publicando-o.

---

**Pré-requisitos**

Antes de começar, certifique-se de que você tem as seguintes ferramentas instaladas e configuradas:

Terraform
AWS CLI
Uma conta na AWS com credenciais configuradas (via aws configure)
Ansible

---

**Como Executar**

Clone o repositório:

```git clone [URL_DO_SEU_REPOSITORIO]```
```cd [NOME_DA_PASTA]```

Configure seu IP público:

Abra o arquivo ```variables.tf``` e altere a variável ```meu_ip_publico``` para o seu endereço IP público no formato CIDR (ex: "SEU_IP/32"). Isso garantirá que apenas sua máquina possa acessar a instância via SSH.

Inicialize o Terraform:

Este comando prepara seu ambiente, baixando os plugins necessários.

```terraform init```

Crie a Infraestrutura:
Execute o apply para provisionar os recursos na AWS. Confirme a operação digitando yes.

```terraform apply```

Atualize o Inventário do Ansible:

Após a conclusão do apply, o Terraform exibirá o IP público da instância. Copie este IP e cole-o no arquivo inventory, substituindo o placeholder ip_da_instancia_ec2.

Ajuste as Permissões da Chave SSH:

```chmod 400 ec2-instance-key.pem```


Execute o Ansible para Configurar o Servidor:

```ansible-playbook -i inventory playbook.yml```

Acesse o Site:
Após a finalização do Ansible, acesse o IP público (http://SEU_IP_PUBLICO) no seu navegador para ver o site no ar!

---

Destrua a Infraestrutura:

IMPORTANTE: Quando terminar, não se esqueça de destruir todos os recursos para evitar cobranças na sua conta da AWS.


```terraform destroy```

---

Estrutura do Projeto

```
├── ansible.cfg             # Configurações do Ansible (desabilita host key checking)

├── ec2.tf                  # Define a criação da instância EC2

├── key_pair.tf             # Cria e gerencia o par de chaves SSH

├── outputs.tf              # Define as saídas do Terraform (IP público)

├── playbook.yml            # Playbook do Ansible para configurar o servidor

├── provider.tf             # Configura o provedor da AWS

├── security_group.tf       # Define as regras de firewall (Security Groups)

├── variables.tf            # Define as variáveis de entrada (seu IP)

└── inventory               # Arquivo de inventário do Ansible
```

**Créditos**

Este projeto foi desenvolvido como parte do desafio de IaC da Avanti DVP, com base no material e no passo a passo originais disponíveis em:
https://gitlab.com/avanti-dvp/iac-com-terraform-e-aws
