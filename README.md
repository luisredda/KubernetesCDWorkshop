# Kubernetes Continuous Deliver Workshop Labs

Bem vindos ao workshop de CD em Kubernetes com Harness! Para uma melhor experiência, recomendamos o atendimento dos seguintes pré requisitos:

- Criar uma conta Free trial em https://harness.io/try-continuous-delivery-as-a-service-for-free/ . após o término do Trial você continuará utilizando nossa edição community sem data de expiração.

- Conceitos báicos sobre Kubernetes e Linux

- Acesso a um cluster Kubernetes, podendo ser Minikube. Recomendamos pelo menos 16GB de memória com pelo menos 8GB disponível para o K8s, caso você não possua esses recursos, a Google Cloud oferece até US$ 300 de créditos por 1 ano, o que permite a utilização do GKE: https://cloud.google.com/free


## Lab 1 - Login em app.harness.io e se familiarize com o ambiente.


1. Abra seu browser em https://app.harness.io.

2. Explore a interface. 

3. Visitaremos o menu "Setup" com mais frequência. Clique lá e explore os diferentes conectores e opções de configuração.

4. Pergunte ao instrutor se tiver alguma dúvida sobre alguma configuração ou conceito específico!

## Lab 2 - Instalar o Harness Delegate

O Delegate é o componente que faz a abstração das tecnologias, conexão nas entidades (reposítórios, clusters, etc) e orquestração das atividades relativas a deployments, verificação, governança, etc. No caso do Kubernetes, ele é instalado como um POD em seu cluster. Com isso, não precisamos nos preocupar com o permissionamento dentro do Cluster, já que podemos abstrair e assumir as credenciais de execução.

Para instalar o Delegate, vá até "Setup" - "Harness Delegates". Baixe o Delegate como Kubernetes YAML e aplique em seu cluster. Atenção: A config padrão vem com alocação de 8Gi de memória no Pod, caso seu ambiente seja restrito, diminua para 4Gi. Após a aplicação do YAML em seu cluster você verá seu Delegate disponível na interface.

## Lab 3 - Setup de um deployment básico de NGINX no Kubernetes

Após a instalação do Delegate, é necessário criar um Cloud Provider. Clique em "Setup" - "Cloud Providers" e crie um Cloud Provider do tipo "Kubernetes Cluster". Selecione a opção "Inherit from selected Delegate", teste e salve a configuração.

![Cloud Provider](/images/CloudProvider.png)

**Agora iremos criar uma nova application na Harness.**

1. Clique no menu Setup na parte superior direita.

2. Clique no botão "Add Application" e preencha as informações do sua nova aplicação. 

![Application Setup](/images/application.jpg)

3. Clique em Submit e o Harness criará sua nova aplicação. Agora podemos configurar as outras partes da implantação.

4. Clique em Services para adicionar seu primeiro Serviço em sua aplicação. Clique em Add Service. 
Preencha o nome do seu serviço (Ex: Nginx) e configure o Deployment Type para Kubernetes.

![Add Service](/images/add_service.jpg)

Clique em Submit. Você verá a tela de overview do seu novo serviço.

5.  Na Visão geral do serviço, clique em Add Artifact Source e selecione Docker Registry. Para Source Server, selecione Harness Docker Hub. Essa é uma exemplo de conexão com a configuração do hub.docker.com público da Harness. Para o nome da imagem do Docker, coloque library/nginx. Isso nos permitirá escolher nossa versão do nginx quando a implantação for realizada.

![Artifact Source](/images/artifact_source.jpg)

Clique em submit quando pronto.  Não iremos alterar os Manifests YAML nesse momento, por ser um deployment simples.

6. Clique no nome da aplicação do lado superior esquerdo para voltar para a configuração de Aplicações. Em sua aplicação clique em environments, onde iremos configurar a representação dos ambientes onde o deployment será realizado.

7. Clique em Add Environment e coloque o nome do seu ambiente, e configure o ambiente para non-production. 

![Environment](/images/environment.jpg)

Clique em Submit.

8. Adicione uma Infrastructure Definition. Selecione Kubernetes Cluster para seu cloud provider, e finalmente preencha o namespace desejado.

![Infrastructure Definition](/images/infra_def.jpg)

Clique em submit. 

9. Volte em sua aplicação, clique em Workflows e adicione um novo workflow.

10. Selecione Rolling Deployment, seu ambiente, serviço e definição de infraestrutura.

![Workflow](/images/workflow.jpg)

Clique em Submit.

11. Agora você já está pronto para o deployment! Clique em Deployment no lado superior direito, o que exibirá a tela para iniciar um deplouyment.

12. Como escolhemos "library/nginx" como nosso artefato, agora precisamos selecionar a versão de nginx que desejamos para o deployment.

![Start New Deployment](/images/start_new.jpg)

13. Você pode acompanhar todo o status do deployment em tempo real! Inclusive a exeucução de cada comando no Kubernetes.

![Live Deployment View](/images/deployment_view.jpg)

It really can be that easy to upgrade a service!

## Lab 3 - Cleanup 

1. Caso você queira deletar o deployment anterior e deixar seu cluster limpo, crie um novo workflow com outro nome

![Cleanup Workflow](/images/clean_wf.jpg)

Clique submit.

3. Por padrão, o Harness coloca uma etapa de Rollout Deployment, mas não vamos precisar disso. Passe o mouse sobre o estágio Rollout Deployment e clique no X que aparece ao lado da etapa para excluí-lo.

![Remove Step](/images/remove_step.jpg)

4. Adicione um novo step e clique em "Delete"

![Delete Step](/images/delete_step.jpg)

Clique em Next.

5. Aqui configuraremos o namespace a ser deletado. Nesse caso pode ser hardcoded a nivel de testes mas voce pode usar uma variável para excluir qualquer nome que esteja associado ao deployment de forma dinamica.

![Configure Delete](/images/configure_delete.jpg)

Clique em Submit.

6. Execute o deplyment.

![Final Delete](/images/final_delete.jpg)

7. Parabéns!!!
