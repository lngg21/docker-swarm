 Docker Swarm 
===================

## O que é o docker Swarm?

O docker swarm é um orquestrador de containerês nativo do Docker. Enquanto o Docker Engine permite executar containerês em uma única máquina , o swarm permite transformar um grupo de máquina em um único cluster visual consolidado.


## O que foi feito na atividade:
No inicio da atividade, começamos usando o Docker Compose classico, onde eu gerencia os containeres individualmente em uma única maquina(Docker Daemon). Depois que aplicamos o Docker Swarm, nós transformamos essa única máquina em um cluster unificado

### Os 4 Pilares da Mudança:
#### Manager vs. Workers:
###### Manager: É o "cérebro". Ele monitora a saúde das aplicações e decide em qual máquina cada contêiner roda.  Workers: São as máquinas de trabalho que apenas executam os contêineres ordenados pelo Manager. (Em ambiente de teste/local, o seu próprio PC atua como Manager e Worker ao mesmo tempo).

#### Rede Bridge vs. Rede Overlay:
###### No Compose usamos driver: bridge, que funciona somente dentro de uma mesma máquina física. 
######  No Swarm usamos driver: overlay. A rede Overlay cria uma malha de rede virtual (túnel VXLAN) por cima das placas de rede físicas. Um contêiner na Máquina A consegue falar com um contêiner na Máquina B usando apenas o nome do serviço (DNS interno), com tráfego criptografado e seguro. 
#### Contêineres vs. Serviços e Réplicas:
###### No Compose você roda um contêiner por definição.No Swarm você define um Service com réplicas (ex: 3 réplicas de product, 2 de api-gateway). Se uma réplica travar ou morrer, o Swarm percebe imediatamente e recria uma nova em milissegundos para manter o número desejado ativo.  

#### Ingress Load Balancing (Routing Mesh):
###### O Swarm traz balanceamento de carga nativo. Quando uma requisição chega na porta 8080 de qualquer nó do cluster, a malha de roteamento interna distribui as requisições em modelo Round-Robin entre as réplicas ativas do serviço.


--------------------
## Logs
#### docker stack services aponti

aqui é o comando para ver os serviços rodando e as réplicas ativas.
"APONTI" é o Manager


ID             NAME                 MODE         REPLICAS   IMAGE                 PORTS
ulo9gj75z4nm   aponti_api-gateway   replicated   2/2        aponti-gateway:v1     *:8080->8080/tcp
z726ue16xztn   aponti_database      replicated   0/1        postgres:15-alpine    
y992qxao3xai   aponti_inventory     replicated   0/3        aponti-inventory:v1   
lb2lpvomuiy1   aponti_order         replicated   0/3        aponti-order:v1       
aokgg1t07qgv   aponti_product       replicated   0/3        aponti-product:v1  