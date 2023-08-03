Projeto SIPp
Instalação via docker da imagem e execução de testes simples de REGISTROS e Chamadas.
Até o ultimo comit ainda não consegui gerar e atender as chamadas diretamente no SIPp, somente efetuar as chamadas.

Especificação:
Utilizado google GCP + Maquina Ubuntu 20.04

Instalaçao do ambiente
#Instalação do docker
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
#Download imagem do SIPp
docker pull ctaloi/sipp

#Pré-requisitos para iniciar cenário
Necessário arquivos xml e csv que constam nesse Projeto
Arquivo REGISTER.xml = Nome sugestivo, ele mota o pacote SIP para enviar REGISTER e responder em caso de 401 ou 100

Arquivo REGISTER.csv = Ele contem as configurações dos ramais SIP, usuario e senha.
cada linha desse arquivo será usada separadamente cada vez que o sistema "chamar" o arquivo REGISTER.xml
O arquivo xml utiliza o arquivo csv através de variáveis. [field0];[field1];[field2]...
Sendo que as variáveis são separadas por ponto e virgula no arquivo csv.

