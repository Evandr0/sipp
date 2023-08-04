# Projeto SIPp

Instalação via docker da imagem e execução de testes simples de REGISTROS e Chamadas.
Até o ultimo comit ainda não consegui gerar e atender as chamadas diretamente no SIPp, somente efetuar as chamadas.

Especificação:
Utilizado google GCP + Maquina Ubuntu 20.04

Instalaçao do ambiente
## Instalação do docker e outrs libs
- sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
- sudo apt-get install build-essential libncurses5-dev libopenssl-dev libpcap-dev libnet1.1-dev lksctp-tools libgsl-dev
- sudo apt install build-essential
- sudo apt install libncurses5-dev

## Download imagem do SIPp
- docker pull ctaloi/sipp

## Pré-requisitos para iniciar cenário
- Necessário arquivos xml e csv que constam nesse Projeto

- Arquivo REGISTER.xml = Nome sugestivo, ele monta o pacote SIP para enviar REGISTER e responder em caso de 401 ou 100

- Arquivo REGISTER.csv = Ele contem as configurações dos ramais SIP, usuario e senha.
	cada linha desse arquivo será usada separadamente cada vez que o sistema "chamar" o arquivo REGISTER.xml
	O arquivo xml utiliza o arquivo csv através de variáveis. [field0];[field1];[field2]...
	Sendo que as variáveis são separadas por ponto e virgula no arquivo csv.

- Colocar arquivos xml e csv na pasta /root/sipp-docker/scenarios/

## Execurando o comando para REGISTRAR os ramais
- docker run --name sipp_register --rm -d -v /root/sipp-docker/scenarios:/sipp ctaloi/sipp -sf REGISTER.xml -inf REGISTER.csv -m 31 -l 1 -r 10 -nd 10.158.0.4:5060

Entendendo o comando acima

- docker run : executar container
- --name NOME : Nome que dará ao container
- --rm : Automaticamente remove o container após finalização
- -d : Execução do container em background
- -v : Mapeamento de volume
- ctaloi/sipp : nome da imagem que será executada

Depois do nome da imagem os comandos são referentes ao SIPp

- -sf ARQUIVO.xml : Carrega um arquivo de cenário XML alternativo.
- -inf ARQUIVO.csv : Injete valores de um arquivo CSV externo durante as chamadas nos cenários.
- -m VALOR : Interrompa o teste e saia quando as todas as 'chamadas' forem processadas
- -l VALOR : Defina o número máximo de chamadas simultâneas. Uma vez atingido este limite, o tráfego é reduzido até que o número de chamadas abertas diminua. Padrão: (3 * call_duration (s) * taxa).
- -r VALOR : Defina a quantidade de "chamadas" por segundos
- -nd : Sem padrão. Desative todos os comportamentos padrão do SIPp, que são os seguintes:
                      - No tempo limite de retransmissão UDP, aborte a chamada enviando um BYE ou um CANCEL
                      - No tempo limite de recebimento sem atributo ontimeout, interrompa a chamada enviando um BYE ou um cancelamento
                      - Em BYE inesperado, envie um 200 OK e feche a chamada
                      - Em CANCEL inesperado, envie um 200 OK e feche a chamada
                      - Em PING inesperado envie um 200 OK e continue a chamada
                      - Em qualquer outra mensagem inesperada, interrompa a chamada enviando um BYE ou um CANCELAR

- o útimo valor é referente ao servidorsip:portasip



## Testes chamadas com audio
- Teste basico onde o ramal 2000 liga para o 2031. É necessário ação humana para atender a chamada no ramal 2031
Baixar o arquivo g711a.pcap para a pasta /root/sipp-docker/scenarios
docker run --name sipp_call --rm -d -v /root/sipp-docker/scenarios:/sipp ctaloi/sipp 10.158.0.4:5060 -sf CALL2.xml -inf CALL.csv -m 1 -l 2 -r 2 -nd
