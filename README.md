# Projeto de DevOps 

## :page_with_curl: Introdução

### Este projeto desenvolve uma pipeline completa de **infraestrutura e deploy contínuo utilizando práticas de DevOps**. A meta é provisionar o ambiente de desenvolvimento automaticamente, assim criar um cluster Kubernetes e realizar o deploy de uma **aplicação fullstack do Manual do Dev**, utilizando as ferramentas **GitOps** e **ArgoCD.**

**Tecnologias utilizadas: 
FrontEnd: HTML + CSS + JS
BackEnd: NodeJS + Express
Banco de Dados: MySQL
Devops: Docker, Kubernetes, Kind, ArgoCD, GitHub Actions, Packer, Vagrant, Ansible**
O projeto conecta o DevOps com foco no provisionamento, automação e conteinerização, aplicadas com deploy.
## :wrench: Escolha do Ambiente
### Tipo de ambiente: Máquina Virtual (VM) + Containers (Docker) + Kubernetes (Kind)


**Justificativa:**


Máquinas virtuais são utilizadas para simular infraestrutura com isolamento e reprodutibilidade
Packer, Vagrant e Ansible automatizam a criação de uma VM e instalam de forma automatizada o Docker, Kind e ArgoCD


Docker utiliza containers que simplificam o empacotamento e entrega contínua


Kind facilita a criação de clusters Kubernetes locais para desenvolvimento e testes de CI/CD
GitOps e ArgoCD sincroniza o cluster com o repositório Git para um deploy contínuo


### Descrição da máquina


Sistema Ubuntu Server, levantada com Packer + Vagrant


Incluí Docker, Kind, Kubectl, Nginx e ArgoCD para provisionamento de cluster Kubernetes


Recursos aproximados: 2 CPUs, 2048 Memória
## :bulb: Provisionamento 

### Ferramentas utilizadas:


Packer: Criação de imagem base com Docker, Kind e outras dependências


Vagrant: Criação da VM a partir da imagem realizada pelo Packer
Ansible: Instalação automatizada de ArgoCD, Docker e outros


Docker: Containerização do desenvolvimento frontend, backend e banco de dados
Docker Compose: Execução dos containers fullstack


**Alguns dos principais scripts criados:**
- **Vagrantfile** para provisionamento automatizado da VM


- **debian.json** e **packer.pkr.hcl** para Build da imagem com Packer


**Desafios e soluções:**


Gerenciamento de recursos da VM em máquinas locais → solução: ajustes dinâmicos no Vagrantfile.


Configuração entre containers e o Kind → solução: configuração de rede bridge e portas mapeadas no Kind.
### :rocket: Passo a passo:
**Iniciar packer**
```bash
packer init .
```
**Build da Imagem**
```bash
packer build debian.json
```

**Criar a VM**
```bash
vagrant up
```

## :floppy_disk: Cluster Kubernetes 

### Kind (Kubernetes in Docker)


**Configuração dos nós:**

1 control plane


2 nós de trabalho (workers)


**Testes realizados:**


Deploy de pods de teste e exposição via service


Validação de nós
```bash
kubectl get nodes
```

## :dart: GitOps com ArgoCD 

**Instalação do ArgoCD:**
- Instalado através do Ansible (**install_argocd.yml**)

- Deploy via YAML oficial de instalação no cluster Kind

- Configuração do serviço para acesso local via NodePort

- Login com usuário admin e senha gerada através do Ansible (**rotate_argo.yml**)

- Conectado ao GitHub para monitoramento


### Configuração do repositório Git:


**Repositório GitHub configurado com a estrutura:**

```bash
:file_folder: /packer-Devops-gabi (Packer, Vagrant)
:file_folder: /todolist-fullstack-backend (aplicação backend)
:file_folder: /frontend-fullstack (aplicação frontend)
:file_folder: /k8s (Deployment, Kustomization e Service)
```
**ArgoCD:** configurado para monitorar a pasta /k8s para deploy automatizado


**Deploy da aplicação:**


Backend: NodeJS + Express containerizado


Frontend: HTML + CSS + JS via container Nginx


Banco de dados: MySQL via container


Rede: Serviços expostos internamente via ClusterIP com frontend exposto via NodePort para acesso externo


**Imagem do ArgoCD:**



## :computer: Aplicação 

### Descrição:
Aplicação web de uma lista de tarefas, com cadastro e listagem de dados, demonstrando CRUD (criação, leitura, atualização e remoção).


**Backend:** NodeJS + Express (rotas CRUD)


**Frontend:** HTML, CSS, JavaScript (formulário e listagem)


**Database:** MySQL (armazenamento)
**Como acessar a aplicação no cluster:**


Acesso ao frontend pelo NodePort do serviço configurado no Kind


Backend e banco de dados acessíveis internamente via cluster


Adicionar URL local de acesso após subida: 191.52.55.65:4444
## :arrows_counterclockwise: Reprodução do Projeto
Para reproduzir o projeto clone os repositórios
```bash
FrontEnd
$ git clone https://github.com/GabrielaSchubert/frontend-fullstack.git
BackEnd
$ git clone https://github.com/GabrielaSchubert/todolist-fullstack-backend.git
Infraestrutura
$ git clone https://github.com/GabrielaSchubert/packerDevops-gabi.git
```
Após esse passo execute o **packer e vagrant** conforme os passos passados anteriormente, depois verifique o cluster, acesse o **ArgoCD**, conecte os repositórios **Git** e acesse a aplicação com o **IP da máquina e porta NodePort**.
## :heavy_check_mark: Conclusão 
**Lições aprendidas:**
-  Ampliamento das práticas de Devops modernas, como conteinerização e monitoramento


- Integração de ferramentas de provisionamento, Kubernetes e GitOps


- Aplicação do ArgoCD facilitando o deploy contínuo


**Dificuldades encontradas:**
- Compreender as práticas de Devops, como integração de Contêineres e Kind


- Curva de aprendizado de ferramentos como o Kubernetes e ArgoCD


**O que faria diferente:**
- Separar melhor o ambiente de trabalho, como pastas e arquivos


- Automatização completa do deploy da aplicação com configuração do ArgoCD com Ansible, integrando perfeitamente
## :heavy_exclamation_mark: Link para Repositório

```bash
FrontEnd
$ git clone https://github.com/GabrielaSchubert/frontend-fullstack.git
BackEnd
$ git clone https://github.com/GabrielaSchubert/todolist-fullstack-backend.git
Infraestrutura
$ git clone https://github.com/GabrielaSchubert/packerDevops-gabi.git
```
