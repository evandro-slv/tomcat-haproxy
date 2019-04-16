# Criando um proxy/load balancer com Tomcat + HAProxy

Veremos como usar o HAProxy como load balancer para direcionar requisições à dois servidores executando o Tomcat, usaremos também o docker para facilitar a execução de contêineres isolados para as aplicações, mas o mesmo pode ser feito em um ambiente comum e compartilhado.

Podemos então executar o comando que sobe nossos contêineres ao mesmo tempo:

    docker-compose up

> O comando pode ser executado com a flag -d para rodar em segundo plano.

Assim que todos processos forem executados, navegamos até a url http://localhost/ para ver nosso serviço e em http://localhost:1234 para ver os stats do nosso proxy:
Podemos simular um deploy de versão alterando o arquivo haproxy.cfg :

    backend web-backend
      balance roundrobin
      server tomcat1 tomcat1:8080 check disabled  # atualizando serviço
      server tomcat2 tomcat2:8080 check

E podemos atualizar o proxy com o seguinte comando:

    docker kill -s HUP haproxy_haproxy_1

Este comando envia um sinal HUP para a máquina que está executando o haproxy (neste caso, `haproxy_haproxy_1`), que irá recarregar as configurações em tempo real sem downtime algum.

Para mais informações, ver o artigo no medium: https://medium.com/tango-labs/criando-um-proxy-load-balancer-com-tomcat-haproxy-1eb353839e77
