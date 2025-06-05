Esse script Bash faz uma varredura na rede local para listar os dispositivos conectados, exibindo:

    IP

    Nome do dispositivo (quando possível)

É útil, por exemplo, para quem quer saber quem está usando a sua rede Wi-Fi ou simplesmente mapear os dispositivos conectados em uma rede interna.
🔍 Etapas explicadas:
1. Identificação da Distribuição Linux

distro=`cat /etc/*-release | grep PRETTY_NAME | cut -d "\"" -f2`

Exibe qual distro você está usando (por exemplo: Debian, Kali, etc.).
2. Verificação de Conexões Ativas

qtdConexoes=`ifconfig -a | grep broadcast -c`

Verifica quantas interfaces de rede com suporte a broadcast (geralmente as conectadas) existem.
3. Captura de Interfaces e seus IPs

interfaces=( `...` )
inets=( `...` )

Pega o nome das interfaces (tipo eth0, wlan0, etc.) e os respectivos IPs.

Se tiver mais de uma interface conectada, o script te pergunta qual interface você quer usar.
4. Obtém o Gateway e o Prefixo da Rede

gateway=${lin[1]}
prefixo="${octetos[0]}.${octetos[1]}.${octetos[2]}"

Com isso, ele define o intervalo de IPs a escanear. Por exemplo, se o gateway for 192.168.1.1, ele faz um scan de 192.168.1.0 a 192.168.1.255.
5. Escaneia IPs Ativos

sudo nmap -sP -n -T5 --exclude "$gateway" "$prefixo.0-255"

Usa o nmap para encontrar dispositivos ativos (respostas ao ping).
6. Obtém Nome dos Dispositivos

nmblookup -A "$ip"

Tenta identificar o nome NetBIOS (Windows) do dispositivo.

Se não conseguir, tenta pegar o fabricante da placa de rede pelo MAC address usando o nmap.
7. Exibe os Dados

echo -e " IP\t\t->\tDispositivo"

Mostra uma lista no terminal com todos os IPs conectados e seus respectivos nomes.
