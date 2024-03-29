# Utilização prática de Docker e Microserviços

## Tópicos abordados

 1. Microserviços: Desmebrar aplicações em serviços independentes usando APIs, a vantagem é a modulariade e escalabilidade.

 2. Cluster: Conjunto de computadores trabalhando em conjunto. Cada computador é conhecido como um nó (node) e executam a mesma tarefa, controlado e programado pelo sistema.

 3. Swarm: Oferece funcionalidades de orquestração de contêiner, incluindo clustering nativo de hosts do Docker e agendamento de cargas de trabalho de contêiners. Um grupo de hosts do Docker formam um cluster "Swarm".

- Vantagens: Divisão da carga de trabalho, em caso de problemas de hardware outro nó assume automaticamente as cargas de trabalho, garantindo que o aplicativo seja escalável e resiliente.

## Etapas do processo

  Criar uma instância na nuvem pública geralmente envolve os seguintes passos básicos. Vou dar uma visão geral desses passos usando a AWS (Amazon Web Services) como exemplo, pois é uma das plataformas de nuvem pública mais populares:

 1. Crie uma conta na AWS: Acesse o site da AWS (<https://aws.amazon.com>) e crie uma conta. Você precisará fornecer informações de pagamento, mesmo que opte pelos serviços gratuitos.

 2. Acesse o Console de Gerenciamento da AWS: Após criar sua conta, faça login no Console de Gerenciamento da AWS usando suas credenciais.

 3. Escolha a Região: Na parte superior direita do Console de Gerenciamento, selecione a região em que deseja criar sua instância. As regiões podem variar em termos de recursos disponíveis e preços.

 4. Navegue até o Serviço EC2: No Console de Gerenciamento da AWS, procure pelo serviço EC2 (Elastic Compute Cloud) e clique nele para acessar o painel de controle do EC2.

 5. Crie uma Instância: No painel do EC2, clique em "Launch Instance" (Iniciar Instância) para iniciar o assistente de criação de instância.

 6. Escolha uma AMI (Amazon Machine Image) - um template que contém o software necessário para sua instância. Você pode escolher uma AMI fornecida pela AWS ou uma personalizada, se preferir.

 7. Selecione o tipo de instância, que determina os recursos computacionais (CPU, memória, armazenamento, etc.) disponíveis para sua instância.

 8. Configure os detalhes da instância, como número de instâncias, rede, segurança, etc.

 9. Revise as configurações e, quando estiver satisfeito, clique em "Launch" (Iniciar).

 10. Você será solicitado a criar ou selecionar um par de chaves para autenticação SSH. Isso permite que você se conecte à sua instância remotamente.

 11. Acesse sua Instância: Após lançar a instância, ela será provisionada e iniciada. Você pode acessá-la via SSH (para instâncias Linux) ou Remote Desktop (para instâncias Windows) usando o par de chaves criado durante o processo de lançamento.

 12. Gerencie sua Instância: Você pode gerenciar sua instância através do Console de Gerenciamento da AWS, onde pode parar, iniciar, reiniciar, redimensionar e até mesmo excluir a instância conforme necessário.

Lembre-se de que esses são os passos básicos para criar uma instância na AWS. Outras plataformas de nuvem pública, como Google Cloud Platform (GCP) e Microsoft Azure, têm processos semelhantes, mas com suas próprias interfaces e terminologias específicas.

## Criação de Imagens Docker

 1. Encontre a imagem que deseja utilizar no docker hub(<https://hub.docker.com/>) ou crie nativamente e instale as dependencias necessarias.

 2. Voce pode criar conteiners como: MySQL(<https://hub.docker.com/_/mysql>) ou Apache(<https://hub.docker.com/_/zookeeper>), conforme as necessidades do seu projeto.

 3. Após a criação do conteiner é importante fazer testes de estresse no container, as ferramentas variam dependendo do conteiner que precisa ser testado, algumas delas são: Apache JMeter, Gatling, Siege, Locust e loader.io.

Boas práticas de criação de imagens: Inclua informações sobre boas práticas ao criar imagens Docker, como usar camadas de forma eficiente, minimizar o tamanho da imagem, utilizar imagens base oficiais sempre que possível, e limpar arquivos temporários em camadas intermediárias.

## Iniciando um cluster no Swarm

O modo Swarm é um recurso avançado para gerenciar um cluster de daemons do Docker. <https://docs.docker.com/engine/swarm/> .Docker Swarm permite que você agrupe vários hosts Docker (geralmente chamados de nós) em um cluster para formar um único sistema de contêineres. Uma vez que você tenha configurado um cluster Swarm, você pode usar o Swarm para implantar, escalar, gerenciar e manter aplicativos distribuídos em contêineres em larga escala em um ambiente de produção.

Algumas das principais funcionalidades fornecidas pelo Docker Swarm incluem:

 1. Orquestração de Contêineres: Swarm facilita a implantação e gerenciamento de aplicativos distribuídos em contêineres. Você pode definir serviços que consistem em vários contêineres e especificar como eles devem ser implantados e escalados no cluster.

 2. Balanceamento de Carga: Swarm inclui um balanceador de carga integrado que distribui o tráfego entre os contêineres de um serviço, garantindo que as solicitações sejam distribuídas de forma eficiente e que os contêineres estejam distribuídos de maneira uniforme entre os nós do cluster.

 3. Escalabilidade Automática: Swarm oferece suporte à escalabilidade automática de serviços com base em métricas de desempenho, como utilização de CPU ou carga de rede. Isso permite que você dimensione dinamicamente os recursos do seu aplicativo conforme necessário para lidar com picos de tráfego ou demanda.

 4. Alta Disponibilidade: Swarm fornece recursos para garantir a alta disponibilidade de aplicativos em contêineres, incluindo a replicação de contêineres em vários nós do cluster e a detecção e recuperação automática de falhas.

Configuração de segurança: Além de criar o cluster, é importante abordar questões de segurança, como autenticação e autorização, para proteger o cluster contra acessos não autorizados.

## Criando um serviço no cluster

Nesta etapa podemos criar a quantidade de replicas dos serviços necessarios com docker que se divide entre os nós criados dos clusters.

```  bash
docker service create --replicas 3 --name meu-servico meu-imagem
```

## Replicando o volume

Podemos utilizar um servidor nfs, mas existem algumas resalvas:

 1. Overhead de Rede: NFS opera sobre a rede, o que pode introduzir algum overhead de rede, especialmente em ambientes distribuídos. Isso pode afetar o desempenho da aplicação, especialmente em cenários de alta carga.

 2. Pontos Únicos de Falha (SPOFs): Se o servidor NFS se tornar um ponto único de falha, pode afetar a disponibilidade e a confiabilidade do sistema como um todo. Para mitigar isso, você pode configurar o NFS com alta disponibilidade, mas isso adiciona complexidade ao seu ambiente.

 3. Escalabilidade Limitada: Embora o NFS seja escalável até certo ponto, ele pode não ser a melhor escolha em ambientes de contêineres altamente dinâmicos e em escala.

 4. Alternativas Modernas: Sistemas de armazenamento distribuído baseados em nuvem (por exemplo, Amazon EFS, Google Cloud Filestore) ou soluções de armazenamento distribuído nativas do Kubernetes (por exemplo, Kubernetes Persistent Volumes com CSI drivers), podem oferecer melhor escalabilidade, desempenho e integração com ambientes de contêineres.

### Algumas opções populares ao invés de nfs são

 1. Docker Volume Plugins: Docker Swarm oferece suporte a plugins de volume que permitem integrar sistemas de armazenamento externos diretamente ao Docker. Isso pode incluir sistemas de armazenamento distribuído como GlusterFS, Ceph, RexRay, entre outros. Esses plugins permitem que os contêineres em um cluster Docker Swarm acessem volumes compartilhados sem a necessidade de configurar um servidor NFS separado.

 2. Soluções de Armazenamento em Nuvem: Plataformas de nuvem como AWS, Google Cloud Platform e Microsoft Azure oferecem serviços de armazenamento em nuvem altamente disponíveis e escaláveis que podem ser facilmente integrados aos clusters Docker Swarm. Por exemplo, Amazon EFS (Elastic File System), Google Cloud Filestore e Azure Files oferecem armazenamento de arquivos distribuído que pode ser montado em vários contêineres em diferentes nós do cluster.

 3. Soluções de Armazenamento Distribuído do Kubernetes: Se você estiver usando Kubernetes como orquestrador de contêineres em vez de Docker Swarm, há várias opções nativas de armazenamento distribuído disponíveis, como Persistent Volumes (PVs) com Container Storage Interface (CSI) drivers. Essas soluções permitem que os volumes persistam além da vida útil dos contêineres e possam ser compartilhados entre contêineres em diferentes pods e nós do cluster Kubernetes.

## Criando um proxy com NGINX

Voce pode criar um arquivo nginx.conf

``` bash
http {
 upstream all {
  server 172.31.0.127:80;
  server 172.31.0.333:80;
 }
 server {
  listen 4500;
  location / {
   proxy_pass http://all;
  }
 }
}

events { }
```

Então criar um arquivo dockerfile para usar a imagem oficial do nginx e o arquivo de configuração criado. Observação: é interessante manter um conteiner separado. Isso mantém a separação de preocupações entre o servidor da Web e o proxy reverso, facilitando a manutenção e a escalabilidade do sistema.

O NGINX é uma das ferramentas mais populares para realizar o balanceamento de carga em ambientes de contêineres e microserviços devido à sua eficiência, flexibilidade e recursos avançados de configuração que pode ser utilizado.

## Estressando o cluster

### Kubernetes

- Kube-burner: Ferramenta de benchmarking do Kubernetes para avaliar a escalabilidade do cluster e o desempenho dos recursos.

- Kube-bench: Ferramenta de auditoria de segurança do Kubernetes para verificar a conformidade com os padrões de segurança recomendados.

- Litmus: Uma estrutura para execução de testes de resiliência em clusters Kubernetes, que inclui uma biblioteca de cenários de teste pré-construídos.

### Docker Swarm

- Docker Swarm Visualizer: Uma ferramenta de visualização que permite monitorar o estado do cluster Docker Swarm e acompanhar o escalonamento de serviços e a distribuição de contêineres.
- Swarm Mode Stress Test: Uma ferramenta que pode ser usada para criar uma carga de trabalho simulada em um cluster Docker Swarm, testando sua capacidade de dimensionamento e tolerância a falhas.

Também podem ser utilizadas ferramentas de teste de carga gerais como: Apache Jmeter, Gatling, Locust e loader.io. É importante evidenciar os testes pois eles previnem muitos problemas, não apenas os teste de carga e resiliencia, mas também os testes de segurança e as medidas preventivas necessarias.

## Contras no uso da nuvem privada

1. Desafios com a segurança da Tecnologia da informação (lógica e física) e medidas preventivas para garantir a continuidade nos negócios.
2. Custo com mão de obra especializada.
3. Custo de Hardware.
4. Custo de Energia elétrica.
5. Falta de Energia (uso de geradores).
6. Despesas inesperadas.
7. Planejamento das estratégias necessarias.
