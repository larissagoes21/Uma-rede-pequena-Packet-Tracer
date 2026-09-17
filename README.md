# Uma-rede-pequena-Packet-Tracer
Prática de configuração de uma rede pequena utilizando o Cisco Packet Tracer(em andamento).

## As tecnologias e ferramentas utilizadas foram:
Packet Tracer
13 pc 
3 laptop
5 switch 
2 roteadores
1 servidor
Cabos diretos
Cabos cruzado

## Sua topologia

A rede é composta por pcs, laptops, switchs, roteadores, servidor e cabos. A topologia foi dividida em 5 setores:

- TI/suporte;
- Financeiro;
- administrativo;
- RH;
- Atendimento;

A rede foi dividida em diferentes sub-redes para permitir a organização e a comunicação entre os setores.

## Configuração da rede: 

o servidor foi configurado para distribuir endereços IPs de forma automática através do DHCP, incluindo informações como:  
`IPs`, `Gateway padrão` e `Máscara de sub-rede`.

Foram utilizados dois roteadores para separar os domínios de broadcast.

Os switches e os roteadores foram configurados através do `CLI`, com `endereços IP` para se comunicarem dentro da rede como: `IPs e Máscara de sub-rede`.

Os roteadores foram configurados com senhas para impedir que usuários não autorizados tenham acesso.

## Montagem da rede

Primeiro, foram adicionados 13 pcs, 3 laptop, 5 switch, 2 roteador e 1 servidor.

Conectei cada Pc ao switch da sua rede utilizando cabos diretos. Depois, conectei os switchs aos roteadores e os roteadores ao switch da rede `TI/Suporte`, que também possui conexão com o `servidor DHCP`.

Configurei os roteadores com um `hostname` e uma `senha`.

- Roteador1
Hostname: cisco senha: cisco

- Roteador2
 Hostname: cisco2 senha: cisco2
 
Nos roteadores as portas ligadas ao switch do TI estavam desligadas, fui em CLI > digitei o comando nome da porta e depois dei “No shutdown” > Deu conectividade.
Despois nos roteadores configurei endereços IPs para o roteador1 e para roteador2 conectado ao switch do TI 
Roteador1 IP: 192.168.0.1 Máscara de sub-rede: 255.255.255.0
Roteador2 IP: 192.168.0.2 Máscara de sub-rede: 255.255.255.0
Agora os roteadores conseguem se comunicar com a rede TI

Configuração DHCP:

No servidor, fui em desktop > ip configuration para configurar um IP ao meu servidor DHCP
DHCP IP: 192.168.0.10 Máscara de sub-rede: 255.255.255.0
Gateway:192.168.0.1
Depois do IP configuration > services > DHCP e deixei a configuração do DHCP ligada.
O DHCP está configurado para atribui endereços ips ao TI partir de: 192.168.0.3 a 192.168.0.20 
depois add > save para salvar a pool do TI
Fiz o mesmo para as outras redes:
Atendimento: 192.168.1.3 a 192.168.1.20 
RH: 192.168.2.3 a 192.168.2.20
Administrativo: 192.168.3.3 a 192.168.3.20 
Financeiro: 192.168.4.3 a 192.168.4.20 
Após configurar as pool para cada rede poderem receber seus endereços IPs.
Configurando Vlans: 
Depois, configurei uma VLAN para cada rede e suas respectivas interfaces VLAN
enable >  configure terminal > interface vlan número da vlan >ip address endereço IP > ip helper-address 192.168.0.10 > no shutdown
Configurei cada vlan com um ip helper-address para encaminhar as solicitações DHCP das redes diferentes da rede do servidor para o servidor DHCP.
Depois de fazer com que cada vlan tenha um endereço IP, acessei o atendimento e fiz testes para saber se estava funcionando, após concluir o funcionamento fiz o mesmo com os outros 2 pcs na rede atendimento. 
E repeti o processo no RH, mas os IPs que do dhcp que estavam aparecendo são do atendimento.
Para corrigir esse erro antes fiz o comando show vlan brief para saber qual era o problema na interface Vlan
A interface vlan do RH estava dando na porta vlan1 e isso fez que a rede RH recebesse IPs da rede atendimento
 Para corrigir esse erro usei comandos como: switchport mode access e switchport access vlan 2 para voltar para portas certas

Como esperado o RH começou a receber os endereços certos: 192.168.2.0/24

Repeti o mesmo processo de configuração nas demais VLANs.
Ao configurar o administrativo 192.168.3.1 no pc 6 mas apresentou uma falha.
 fui investigar isso configurei temporiamente um ip estático no pc 6: 192.168.3.4 Depois utilizei um PDU para acompanhar a comunicação entre o PC e o servidor DHCP no modo Simulation.
 A solicitação chegava ao servidor mas quando voltava. o pacote chegava ao Roteador 1, que não possuía uma rota para a rede 192.168.3.0/24. Dessa forma, o pacote não conseguia retornar corretamente ao PC.
para resolver esse problema eu adicionei uma rota para o endereço ip com o comando ip route 192.168.3.0 255.255.255.0 192.168.0.2
também adicionei rotas do roteador1 para o roteador2 e vice-versa e das redes de cada roteador 
Após as correções, o DHCP passou a funcionar corretamente na rede Administrativa.
Testes:
Realizei testes de conectividade utilizando o comando ping entre PCs e seus respectivos gateways.
Testei a comunicação entre PC e servidor DHCP.
Depois, realizei testes de comunicação entre dispositivos de redes diferentes.
Um dos testes foi realizado entre um PC do Financeiro e um PC do Atendimento. O teste de ping foi concluído com sucesso, demonstrando que os dispositivos de redes diferentes conseguiam se comunicar através dos roteadores.
Testei o caminho do PDU do Administrativo até o servidor e identificou o problema de rota. 

Também utilizei o Simulation Mode do Packet Tracer para acompanhar o caminho dos pacotes durante os testes.
Resultado:
Após as configurações e correções, os PCs conseguiram obter automaticamente seus endereços IP através do servidor DHCP e se comunicar com dispositivos de outras redes.
Como resultado, consegui montar e configurar uma pequena rede composta por 13 PCs, 3 laptops, 5 switches, 2 roteadores e 1 servidor DHCP, colocando em prática conceitos de endereçamento IP, DHCP, VLANs, gateways, roteamento, comunicação entre redes e troubleshooting.

