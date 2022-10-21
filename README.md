# AWS-Jenkins-Pipeline

## Configurando um pipeline de CI/CD integrando o Jenkins com AWS CodeBuild e AWS CodeDeploy 
 
O servidor de automação de código aberto Jenkins é usado para implantar artefatos do AWS CodeBuild com o AWS CodeDeploy, criando um pipeline de CI/CD funcional.
Quando implementado corretamente, o pipeline de CI/CD é acionado por alterações de código enviadas ao repositório do GitHub, alimentadas automaticamente no CodeBuild e, em seguida, a saída é implantada no CodeDeploy.
  
 
O pipeline em funcionamento cria um serviço de compilação totalmente gerenciado que compila seu código-fonte. Em seguida, ele produz artefatos de código que podem ser usados pelo CodeDeploy para implantar em seu ambiente de produção automaticamente.


![image](https://user-images.githubusercontent.com/48589838/89983289-e5fc2900-dc94-11ea-9258-685375cad1dd.png)



### Passo à passo 

Criando recursos para construir a infraestrutura, incluindo o servidor Jenkins, o projeto CodeBuild e o aplicativo CodeDeploy.

Acessando e desbloqueando o servidor Jenkins.

Criando um projeto e configurando o plugin CodeDeploy Jenkins.

Testando todo o pipeline de CI/CD.

### Crie os recursos
como iniciar um modelo do AWS CloudFormation, uma ferramenta que cria os seguintes recursos:

```
- Bucket do Amazon S3 — armazena os arquivos do repositório do GitHub e o arquivo do aplicativo 
de artefato do CodeBuild que o CodeDeploy usa;
- Política de bucket do IAM S3 — permite que o servidor Jenkins acesse o bucket do S3;
- JenkinsRole — uma função do IAM e um perfil de instância para a instância do Amazon EC2 para 
uso como um servidor Jenkins. Essa função permite que o Jenkins na instância do EC2 acesse o bucket do S3 
para gravar arquivos e acessar para criar implantações do CodeDeploy;
- Aplicativo CodeDeploy e grupo de implantação CodeDeploy;
- Função de serviço do CodeDeploy — uma função do IAM para permitir que o CodeDeploy leia as tags aplicadas às 
instâncias ou os nomes de grupo do EC2 Auto Scaling associados às instâncias;
- CodeDeployRole — uma função do IAM e um perfil de instância para as instâncias EC2 do CodeDeploy. Essa função tem 
permissões para gravar arquivos no bucket do S3 criado por este modelo e para criar implantações no CodeDeploy;
- CodeBuildRole — uma função do IAM a ser usada pelo CodeBuild para acessar o bucket do S3 e criar os projetos de compilação;
- Servidor Jenkins — Uma instância do EC2 executando o Jenkins;
- Projeto CodeBuild — é configurado com o bucket do S3 e o artefato do S3;
- Grupo de Auto Scaling — contém instâncias do EC2 que executam o Apache e o agente do CodeDeploy com um Elastic Load Balancer;
- Configurações de inicialização do Auto Scaling — Para uso pelo grupo do Auto Scaling;
- Grupos de segurança — para o servidor Jenkins, o balanceador de carga e a instância do CodeDeploy EC2.
```

![image](https://user-images.githubusercontent.com/48589838/89985330-87d14500-dc98-11ea-9964-c1211d0c8a03.png)

![image](https://user-images.githubusercontent.com/48589838/89985319-83a52780-dc98-11ea-8442-3e8e7eb3e403.png)


### Acesse e desbloqueie seu servidor Jenkins

Copie o valor JenkinsServerDNSName da guia Saídas da pilha do CloudFormation e cole-o em seu navegador.

Para desbloquear o servidor Jenkins, faça SSH para o servidor usando o endereço IP e o par de chaves, seguindo as instruções de Desbloqueando o Jenkins.

Use o usuário root para Cat o arquivo de log (/var/log/jenkins/jenkins.log) e copie a senha alfanumérica gerada automaticamente (entre os dois conjuntos de asteriscos). Em seguida, use a senha para desbloquear seu servidor Jenkins, conforme mostrado nas capturas de tela a seguir.

![image](https://user-images.githubusercontent.com/48589838/89985442-ba7b3d80-dc98-11ea-9cb4-9014339ba6e3.png)

![image](https://user-images.githubusercontent.com/48589838/89985456-be0ec480-dc98-11ea-9f0a-32333a15e9ce.png)

![image](https://user-images.githubusercontent.com/48589838/89985477-c666ff80-dc98-11ea-8313-dcdec60d39f8.png)

![image](https://user-images.githubusercontent.com/48589838/89985469-c23ae200-dc98-11ea-9243-9c8994fa4f28.png)


### Crie um projeto e configure o plugin CodeDeploy Jenkins

![image](https://user-images.githubusercontent.com/48589838/89985612-fadabb80-dc98-11ea-84cf-c2add128ffc0.png)
![image](https://user-images.githubusercontent.com/48589838/89985621-ff06d900-dc98-11ea-9fee-f80963c8291f.png)
![image](https://user-images.githubusercontent.com/48589838/89985634-05955080-dc99-11ea-9187-db635bdeca9a.png)
![image](https://user-images.githubusercontent.com/48589838/89985688-15149980-dc99-11ea-8810-8e7a43c1e4ff.png)
![image](https://user-images.githubusercontent.com/48589838/89985702-1c3ba780-dc99-11ea-90c3-220b906d91a7.png)
![image](https://user-images.githubusercontent.com/48589838/89985709-1fcf2e80-dc99-11ea-8caf-4962b2721915.png)
![image](https://user-images.githubusercontent.com/48589838/89985726-25c50f80-dc99-11ea-9955-68b7897cb6db.png)
![image](https://user-images.githubusercontent.com/48589838/89985715-22ca1f00-dc99-11ea-9fe5-4a1b0c79e65c.png)
![image](https://user-images.githubusercontent.com/48589838/89985694-180f8a00-dc99-11ea-8a3c-fa211b9ea87e.png)
![image](https://user-images.githubusercontent.com/48589838/89985744-28c00000-dc99-11ea-8e62-e3d18baa5152.png)
![image](https://user-images.githubusercontent.com/48589838/89985756-2c538700-dc99-11ea-9318-a0cb7a6aed0a.png)
![image](https://user-images.githubusercontent.com/48589838/89985781-31b0d180-dc99-11ea-969e-407595b211ad.png)
![image](https://user-images.githubusercontent.com/48589838/89985795-35dcef00-dc99-11ea-816f-2ce6a2bacece.png)
![image](https://user-images.githubusercontent.com/48589838/89985806-38d7df80-dc99-11ea-8cd8-b003ccac1c45.png)
![image](https://user-images.githubusercontent.com/48589838/89985848-45f4ce80-dc99-11ea-9a47-c8256c083864.png)
![image](https://user-images.githubusercontent.com/48589838/89985864-4a20ec00-dc99-11ea-8dbf-fcecdedec7e6.png)
![image](https://user-images.githubusercontent.com/48589838/89985875-4db47300-dc99-11ea-8288-fb7e30a5cb11.png)


### Testando todo o pipeline de CI/CD

Para testar toda a solução, coloque um aplicativo em seu repositório GitHub.

A captura de tela a seguir mostra uma árvore do aplicativo contendo os arquivos de origem do aplicativo, incluindo arquivos de texto e binários, executáveis e pacotes:

![image](https://user-images.githubusercontent.com/48589838/89986084-a71ca200-dc99-11ea-9021-097d82084171.png)

Neste exemplo, os arquivos do aplicativo são o diretório de modelos, o arquivo test_app.py e o arquivo web.py.

O arquivo appspec.yml é o principal arquivo de especificação do aplicativo que informa ao CodeDeploy como implantar seu aplicativo. Jenkins usa o arquivo AppSpec para gerenciar cada implantação como uma série de “hooks” de eventos de ciclo de vida, conforme definido no arquivo. Para obter informações sobre como criar um arquivo AppSpec bem formado, consulte AWS CodeDeploy AppSpec File Reference.

O arquivo buildspec.yml é uma coleção de comandos de compilação e configurações relacionadas, no formato YAML, que o CodeBuild usa para executar uma compilação. Você pode incluir uma especificação de compilação como parte do código-fonte ou pode definir uma especificação de compilação ao criar um projeto de compilação. Para obter mais informações, consulte Como o AWS CodeBuild funciona.

A pasta de scripts contém os scripts que você gostaria de executar durante a execução do CodeDeploy LifecycleHooks em relação aos requisitos do aplicativo. Para obter mais informações, consulte Planejar uma revisão para AWS CodeDeploy.

Para testar esta solução, execute as seguintes etapas:

Unzip the application files and send them to your GitHub repository, run the following git commands from the path where you placed your sample application:

$ git add -A

$ git commit -m 'Your first application'

$ git push
On the Jenkins server dashboard, wait for two minutes until the previously set project trigger starts working. After the trigger starts working, you should see a new build taking place.

![image](https://user-images.githubusercontent.com/48589838/89986214-d92e0400-dc99-11ea-84cb-9ff3e830a1b8.png)

In the Jenkins server Console Output page, check the build events and review the steps performed by each Jenkins plugin. You can also review the CodeDeploy deployment in detail, as shown in the following screenshot:

![image](https://user-images.githubusercontent.com/48589838/89986227-dd5a2180-dc99-11ea-95a5-15938ac49df1.png)

On completion, Jenkins should report that you have successfully deployed a web application. You can also use your ELBDNSName value to confirm that the deployed application is running successfully.

![image](https://user-images.githubusercontent.com/48589838/89986033-9409d200-dc99-11ea-883c-37f6a469e02c.png)
