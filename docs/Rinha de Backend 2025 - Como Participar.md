Transcrição


0:00
Eu tô gravando esse vídeo aqui que é para tentar explicar de uma forma mais fácil como que é o desafio da rinha de
0:06
back end terceira edição 2025, que eu tô chamando de payment processor.
0:13
É, você vai precisar desenvolver, você precisa desenvolver um backend que vai
0:18
receber requisições de pagamentos, de solicitação de pagamentos e
0:23
eventualmente você precisa eh repassar essas solicitações
0:29
para um payment processor, que é quem realmente vai fazer o processamento do pagamento. Então, é como se você fosse
0:36
um proxy, é como você vai intermediar essas requisições, tá? E esse payment
0:42
processor, ele te cobra uma taxa, tá bom? É, vamos supor aqui que você receba
0:47
uma requisição de pagamento de R$ 100 e se a taxa do payment processor for de
0:54
5%, você fica com R$ 95 e o payment processor te cobre um dinheiro aí, um te
1:02
come uma taxa de 5% de R$ 5, no caso de
1:07
de R$ 100 de pagamento. Eh, essa é a ideia básica. Só que agora a
1:14
gente tem alguns porénens aqui. Eh, o payment processor durante o teste,
1:20
ele vai sofrer instabilidades, ele vai ficar indisponível, ele vai ficar com tempos alto, com tempo com tempos muito
1:28
altos de resposta, muito altos, muito baixos, vai variar. E para isso a gente vai ter um payment
1:36
processor de contingência, que a gente chama de fallback. Vamos chamar esse
1:41
aqui de de default, que é o principal, que é o mais barato, tá? E o fallback
1:48
ele vai te cobrar uma taxa maior do que a default, tá? Então, na teoria, você
1:55
deveria preferir sempre conseguir intermediar os as requisições de
2:01
pagamento pro default, porque você vai pagar menos taxa e pagando menos taxa, você vai ter mais
2:08
pontuação na rinha de back end, tá? No resultado final. Eh,
2:16
e como eu disse aqui, ele vai ficar o o PM de processor vai sofrer instabilidades, ele vai ficar com tempos
2:21
altos de resposta e ele pode também ficar indisponível. E o FBEC também pode acontecer isso, tá bom? Então, você vai
2:28
precisar desenvolver uma estratégia de como que você vai lidar com essas
2:34
esporag com essas eh instabilidades, com essa eh indisponibilidade que pode
2:40
acontecer com qualquer um. desses payment processors, inclusive ao mesmo tempo, tá? Os dois podem ficar ao
2:48
mesmo tempo indisponíveis durante o teste. Colocar aqui eh bem menos. Os dois os
2:57
dois tem eh os dois permit processors t exatamente a mesma PI, tá bom? É, é literalmente o mesmo contêiner, a mesma
3:03
coisa. A única diferença eh são as taxas, tá? O default sempre,
3:09
sempre é uma taxa fixa, sempre vai ter a taxa melhor do que o fall. Melhor
3:15
significa menor, tá? Você precisa, o a pontuação da rinha é basicamente, acho
3:20
que eu já disse isso, eh conseguir fazer o máximo de processamento de pagamentos para ter o
3:26
maior lucro possível, tá? Eh, que mais aqui?
3:32
Uma outra coisa, esses os dois, tudo que eu disser que um
3:38
que um payment processor tem de API, o outro também vai ter, tá? Porque eles são idênticos, como eu disse, a única
3:43
coisa que muda é a taxa. Você eles vão disponibilizar uma API de
3:50
health, então você vai conseguir verificar como que tá o status do end
3:57
point de pag de de post payments, tá?
4:03
Então eles, os dois vão disponibilizar um, acho que é payments.
4:08
Eh, eu esqueci o nome. Pera aí, deixa eu deixa eu consultar aqui como é que é,
4:15
qual que é o nome que eu esqueci.
4:23
Health check. Eu tô buscando aqui, tá?
4:30
Tá. É, esse aqui achei. Opa.
4:40
Os dois vão disponibilizar esse health check aqui que e a resposta da chamada
4:47
para esse end point aqui é assim: o serviço está falhando? Sim ou não, buleano? E qual que é o tempo mínimo de
4:55
tempo de ou qual que é o tempo mínimo de resposta? Eu literalmente coloquei um
5:00
tradi, alguma coisa assim, tá? Então eu coloco ali eh 100 msundos, então ele vai
5:06
responder o tempo normal dele de processamento mais esse tread slip aí de
5:12
100 ms pode ser 100 ms pode ser cinco, pode ser 5.000, pode ser qualquer coisa, tá?
5:20
Eh, porém, porém esse end point aqui ele tem uma limitação os pros dois, tá? De
5:27
novo, tudo que eu tudo que eu disser que um tem, o outro também tem. Igual esse esse end point aqui, ele tem uma
5:34
limitação que você só pode chamar uma vez a cada 5 segundos. Então você
5:42
não pode ficar igual louco chamando ele aqui para ver qual que o serviço que tá disponível e e tal, porque você vai
5:50
tomar um erro 429, que é too many requests, tá? Então, de
5:56
novo, a cada 5 segundos, você só pode chamar uma vez, tá bom?
6:04
Muito bem. Beleza. Eh, uma outra coisa aqui que vai acontecer durante o teste,
6:11
o seu back end. Deixa eu, como é que eu faço aqui? Deixa eu tirar aqui. Vai.
6:19
Hum. Deixa eu pegar isso aqui. O seu back endilizar
6:29
um end point de
6:35
de resumo dos pagamentos. Eu vou explicar aqui. Payment
6:40
summary. O que que é esse end point aqui? Você
6:47
precisa responder quantos pagamentos você fez
6:54
e para cada para cada payment processor. Então você vai falar assim: "Ah, pro
6:59
payment processor default eu fiz 200 pagamentos e o total monet de de o total
7:08
de valor é, sei lá, R$ 10.000. E proback eu fiz 20 pagamentos e o e o valor
7:17
monetário total é de X, tá? Então você vai precisar fazer isso.
7:23
E esses aqui, esses dois serviços aqui, deixa eu copiar aqui.
7:30
Opa, não, pelo melhor aqui.
7:37
Eles também vão disponibilizar isso aqui, esse um end point praticamente igual a esse,
7:44
tá? Você não vai chamar esses end points durante o teste. Isso aí não não você
7:50
não não vai poder fazer isso, tá? Quem vai fazer isso aqui é o script de teste,
7:57
tanto é que tem um um prefixo diferente aqui de admin
8:04
e o teste. E ele vai responder basicamente a mesma coisa, né? Então, se
8:10
você disser que tem 10 eh que o default você fez 10 pagamentos com X, o teste
8:17
vai chamar também esse aqui e vai esperar que tenha 10 pagamentos de X. Então é uma verificação de consistência
8:23
para pros dois, tá bom? Pros dois. E para e você e nessas chamadas,
8:31
se os valores não baterem, você vai ser
8:36
penalizado, tá bom? Então você sempre precisa manter consistente o que foi enviado para cá de pagamento com com o
8:44
seu armazenamento local aqui. Aí você vai ver como é que você vai fazer. Geralmente acho que a maioria das
8:50
pessoas vai colocar num banco de dados. Se você quiser colocar na memória, quiser escrever no arquivo, você faz o que você quiser, tá? Então vai existir
8:58
essa essa verificação de tempos em tempos, tá? Eh, a rinha já começou, algumas
9:06
pessoas já estão enviando e tem algumas pessoas falando: "Pô, mas eh eventualmente aqui momentaneamente os
9:13
valores não estão batendo, mas no final bate". Não interessa. Você sempre tem que manter os tentar, né? Manter a os
9:22
pagamentos consistentes. É como se fosse uma verificação, uma auditoria do Banco Central, né? Tá? Então tem essa questão
9:30
também. Ah, que mais? Deixa eu pensar aqui. Hum,
9:35
eu acho que em termos de desafio de APIs é isso.
9:42
E deixa eu pensar se eu tô esquecendo alguma coisa. É, é isso, é isso, é isso.
9:47
Em termos de desafio, é isso. Eh, agora, em termos de de entrega de arquitetura e
9:55
esse tipo de coisa, como nas outras linhas, a gente vai usar o Docker, né? A
10:02
gente usa o Docker aqui. Então você vai desenvolver a sua solução aqui, seu back
10:08
end. E quando eu digo back endo, tá bom? É sua API, seu banco de dados, sua mensageria ou qualquer coisa que você
10:15
queira aqui. Eh, load balance, seja qualquer coisa, você vai desenvolver o
10:21
Docker, você vai declarar todos os seus serviços num arquivo Docker com PS,
10:27
tá? E os os dois payment processor vão estar num outro docker file aqui, tá? Num
10:33
outro num outro numa outra containerização que você não vai precisar fazer nada, tá bom? Você só
10:38
precisa baixar o repositório da rinha e subir o docker compose dos payment
10:44
processor aqui. [Aplausos] Payment processors
10:51
e o seu backend aqui. Seu backend. Eh, uma coisa importante para você
10:58
conseguir, né, fazer essa chamada aqui do seu backend pros payment processor,
11:03
você precisa usar, declarar, eh, fazer uma referência pra rede que tá declarada
11:12
aqui no payment processo, tá? Deixa eu pegar um exemplo aqui.
11:18
A gente já teve algumas submissões, então vou pegar um exemplo aqui.
11:25
Deixa eu fechar as coisas aqui para ficar limpinha a tela
11:32
aqui, ó. Tá? Eh, esse aqui é uma submissão mesmo,
11:40
real, inclusive foi a primeira submissão. Então, é o seguinte, ó. Deixa eu pegar
11:48
um outro arquivo aqui.
11:54
Esse é o Docker Compose do payment processor, tá? Isso aqui tá pronto, você não precisa fazer nada, você só precisa
12:00
eh baixar e subir e subir isso aqui, tá bom? esse conteiner,
12:06
ela declara uma rede aqui chamada de payment processor. É dessa forma que
12:11
você vai se conectar aos ao as APIs de do payment processors,
12:18
tá? Eh, isso aqui é o docker compos do payment processor e esse aqui é o docker
12:23
composed submissão. É um exemplo, né? Então você vai declarar a sua rede aqui, né? Precisa ser no modo bridge, tá bom?
12:31
o host não não é permitido. E aí você vai declarar e incluir aqui uma
12:36
referência essa rede payment processor que é externa. Então é importante que você sempre suba, pelo menos a primeira
12:43
vez o docker compose do payment processor para que ele crie essa rede aqui e não
12:48
dê problema, porque se você subir primeiro o seu docker compose e declarado com uma rede externa que não
12:54
existe, vai dar um erro, tá? E aí um exemplo de como você vai
13:00
eh fazer uma referência para essa rede payment processor no seu serviço.
13:06
Um exemplo tá aqui, ó. Por exemplo, a submissão aqui, ela tem uma API
13:12
e essa API ela ela usa as redes e back end, que é uma rede local aqui, né?
13:18
Vamos chamar assim. e ela faz e ela tem aqui também dec eh incl incluída essa
13:26
rede payment processor, tá? Eh, é bom que já tem um exemplo aqui e o endereço
13:32
e o endereço para você chamar as APIs, eh, os endereços, né, o fall, o default,
13:40
são esses aqui, tá? HTTP, payment processor default. Por que esse
13:46
nome aqui, payment processor default? Porque é o nome do contêiner aqui, ó.
13:52
Cadê? Cadê o nome? É o nome do do host name, desculpa. Pment processor default,
13:58
tá? Então o DNS do Docker vai resolver esse nome enviando para esse serviço
14:05
aqui, tá? E é na porta, os dois serviços são na porta 8080. A porta 8080 é
14:12
disponível dentro do docker, tá bom? Se você quiser acessar os payment processor
14:19
da sua máquina host, eles estão disponíveis. O default, cadê?
14:25
[Música] O default é na porta 8001, então é local
14:32
host porta 8001 e o fall é de é local
14:38
host2. [Música] Beleza? né? Então, se você quiser dar uma explorada ali nessas
14:44
nas APIs, ver a cara ali delas e tal, você consegue também chamar fazer
14:50
chamadas da máquina host, certo?
14:56
Bom, é isso. Deixa eu ver que mais. Hum. Para você submeter, tá? Você fez o seu
15:03
backend, agora você quer participar da rinha. Como é que você faz? Você precisa
15:09
abrir um PR. Deixa eu trazer aqui a janela.
15:14
Per aí, deixa eu. Opa,
15:22
ixe, me enrolei aqui. Pera aí. Eh, esse aqui é o repositório da rinha.
15:28
Rinha de back end 2005, né, na minha conta Franc. O que que você vai fazer
15:33
aqui? É bom que já tem uns uns PRs aqui. Você vai abrir um PR
15:39
adicionando um diretório aqui com as com seu nome, com a sua identificação, tá?
15:47
Eh, com os seguintes artefatos assim aqui, tá? Coloca um readm, explica um
15:55
pouquinho, fala alguma coisa assim pras pras pessoas, porque as pessoas às vezes ficam curiosas e querem ver e tal. Dá
16:01
uma explicada. Ah, eu usei isso aqui, usei aquilo ali. Você quiser colocar alguma, ó, ó que legal aqui, ó. Vai que
16:07
a gente foi desenvolvimento em Gol em Go, processando pagamentos de forma síncrona com redis, como fila e
16:14
armazenamento. O Endinex faz balanceamento entre as instâncias, piriri, pororó. E aí coloca também, é
16:20
legal colocar o o link pro seu repositório público, Git, tá? Então é
16:26
importante que você para participar da R você precisa ter um repositório Git público com o código fonte da sua
16:32
solução, certo? Então coloca um ID, me explica bonitinho.
16:38
Eh, coloca também um arquivo info, que isso aqui se tiver, porque quando tem muita submissão fica difícil, porque as
16:45
pessoas querem ver, ah, quantas submissões eh foram feitas com GO, quantas com
16:51
Node, o que que usou Redis, tal. E aí para foi uma experiência de rinhas
16:56
passadas pra gente conseguir coletar essas informações de forma automática
17:02
mais fácil, aí eu eu peço aqui para que você coloque um arquivo info.jon
17:07
com esses campos aqui, tá? load balancer, storages, né, que é
17:13
praticamente banco de dados, né, as legs que você usou, eh, o link pro seu
17:19
repositório, a sua, as suas redes sociais pra gente conseguir te achar e o seu nome, tá? É simples.
17:27
Eh, e você precisa colocar um Docker Compose e Amel na raiz desse na raiz
17:34
desse diretório, tá bom? E e aí obviamente você vai colocar tudo que precisa, todo artefato que precisa
17:42
para pro para que o seu docker composo funcione, né? Aqui no caso ele tá usando next. Aí ele colocou a se você, se a
17:48
gente abrir aqui, ó, se a gente aqui o endnex, ele tá mapeando aqui, tá vendo?
17:53
O arquivinho innex config, ele tá colocando lá para dentro do contêinern
17:59
que que é onde vai mapear aqui as as APIs, esse tipo de coisa, tá? E aí tem
18:05
gente que coloca script de banco, alguma configuração, coloca o que apenas o que
18:12
precisar para rodar o teste, tá? Eh, tiveram algumas pessoas desavisadas que
18:18
incluíram o código fonte na submissão, tá? Não coloca o código fonte, imagina, eh, não precisa, só coloca aqui na
18:25
submissão o que precisa, o o mínimo necessário para
18:32
para rodar aqui os testes, tá bom? E aí você abre um PR e aí, eh, quer ver, ó,
18:38
por exemplo, tem um uma tem um PR aberto aqui, ó. Vamos dar uma olhada aqui, ó.
18:45
Ó, ele incluiu aqui participantes. Eh, David Alegrin 1, traço GO, ó,
18:52
certinho. Readm Docker Compose info, endo com o que a gente acabou de ver, tá?
19:01
Um outro ponto, um outro ponto, a gente usa os contêiners aqui e as linhas, uma
19:07
coisa essencial das linhas é a limitação de recursos, tá? Eh, como é que a gente
19:13
limita recurso? A gente limita, a gente vai limitar CPU e memória, tá bom? Então
19:20
esse docker composer aqui, ele a soma de todos os recursos precisa dar no máximo
19:27
vula, é um um uma CPU e meia, uma unidade e meia de CPU e 350 m de
19:36
memória. Como que essa distribuição aqui? Você faz do jeito que você quiser, só que a
19:43
soma de todas de todas as e os recursos disponíveis não pode ultrapassar isso
19:49
aqui, tá bom? Vamos pegar um exemplo aqui. Como é que você faz isso?
19:55
Você vai aqui, ó, na na diretiva do do seu serviço, você coloca aqui deploy
20:02
resources limits, CPUs e memory, tá bom? Coloca em Megabyte, por favor, tá bom?
20:09
Eh, então se a gente somar tudo aqui, ó, aqui tem 0,55
20:15
+ 0,25, você vai somando aqui. A soma do CPU não
20:21
pode passar uma unidade e meia de CPU, tá bom? Você distribui como você quiser.
20:27
E a mesma coisa pra memória. A soma das memórias aqui da das restrições de memória não pode não pode ultrapassar
20:33
350 M, certo? para facilitar a nossa vida aqui. A
20:38
minha vida, na verdade, não usa serviço replicado. Deixa eu achar aqui como,
20:45
deixa eu achar um exemplo do que não fazer.
20:54
Comp aqui, ó.
21:03
Hum. Cadê?
21:08
Ó, esse modo aqui, ó, no deploy, você não usa esse modo replicado aqui, tá bom? Porque senão você vai colocar a
21:14
limitação aqui e aí se você puser, por exemplo, réplica do é tudo isso que você
21:20
pôs vezes do, entendeu? Então, declara, declara, se você tem duas APIs, declara
21:27
uma e declara outra. Não coloca réplica dois aqui, tá bom? porque vai complicar muito minha vida aqui para para
21:33
verificar o uso dos recursos. Beleza? É só para é só para facilitar minha vida
21:39
mesmo. Bom, não usa esse modo replicado, beleza?
21:45
Ah, deixa eu ver que mais aqui.
21:52
Ã, a data de submissão, a data final, a data limite
21:59
é dia 17. Deixa eu ver se é isso mesmo, se eu não tô viajando aqui. É dia 17 de
22:05
agosto, que é um domingo. E você pode enviar até às 11:59
22:12
segundos, tá bom? Então, até domingo, dia 17 de agosto, você pode enviar à noite ali, tá? Eu pretendo divulgar os
22:19
resultados no dia 20 de agosto, certo?
22:25
Deixa eu ver se tem mais alguma coisa aqui que eu preciso passar de informação.
22:38
H, aguenta aí, aguenta aí. Ah, tá. Instruções para rodar o teste.
22:47
Eu não vou explicar aqui porque eu vou ter que mostrar muita coisa visual aqui.
22:52
Eu queria manter isso aqui muito simples, mas se você for no repositório da rinha, você vai ver que tem duas duas
22:59
instruções de como rodar o teste. Deixa eu lembrar o nome da pessoa que
23:04
colaborou, porque vocês colocam uns mic aqui que é difícil. É um uma pessoa foi muito gentil e fez
23:12
um mini guia de deixando bem mastigado como rodar o teste. É o Leonardo, tá?
23:17
Leonardo segue fold aqui. Ele fez um um guia, um mini guia de setup para você
23:24
rodar o teste e também tem um mini guia que eu fiz aqui. E aí vocês lê um, lê
23:29
outro, se não entendeu, eh, pergunta também ali, posta nas redes sociais que
23:35
eu a as se eu não conseguir responder, muitas vezes algumas pessoas respondem, a gente vai se ajudando aqui, tá bom?
23:41
Eh, diferentemente das outras linhas, eu não tô usando o Getling, eu tô usando o K6. o K6, né, da Grafana.
23:50
Muita gente pediu, parece que é uma é um é um é mais simples. Eu nunca fiz nada com K6. Eu tenho, apesar de ter
23:58
familiaridade com a sintaxe JavaScript, eu tô muito enferrujado com JavaScript, então eu posso ter feito muita coisa
24:04
esquisita nesse teste. Se você manja de de JavaScript, de K6, encontrar alguma
24:11
coisa esquisita no teste, abre um PR, comenta ali, ajuda, porque a rinha, uma,
24:17
uma das coisas mais legais da rinha é a colaboração. Beleza? Deixa eu ver que mais aqui se eu tô
24:25
como enviar eh os end points. Eu já expliquei certinho.
24:32
Bom, acho que é isso. Eh, um detalhe aqui
24:38
que se você tá, deixa eu explicar uma a pontuação. Eu não expliquei a pontuação. Acho que eu não expliquei direito, né, a
24:43
pontuação. Acho que eu só passei por cima. Deixa eu até mostrar a tela aqui.
24:54
Ah, deixa eu ver aqui. Pontuação, tá aqui, tá? O que que qual que é a
25:01
pontuação da rinha, né? É você o eh é é o máximo de lucro, é o lucro que você
25:07
teve. É basicamente isso, tá? Então, por seu lucro, você vai querer,
25:13
né, teoricamente tentar fazer o máximo de processamentos com o default, com o
25:19
payment processor default, tá? Eh, só que também você vai querer fazer rápido isso,
25:26
porque se você se você se você conseguiu
25:32
entender implicitamente aqui, você não precisa processar de forma síncrona os pagamentos. Você, a
25:40
sua PI pode simplesmente receber uma requisição e você pode enfileirar para você responder mais rápido eh na API,
25:48
tá? Só que se se você tiver muitos pagamentos infilerados,
25:54
a hora que o teste terminar, ele vai ele vai fazer uma requisição
26:01
pro summary, que vai dizer quanto que você processou, tá? E ele vai
26:07
interromper, ele não vai esperar que toda sua fila seja esvaziada, tá? Ele
26:12
vai simplesmente interromper. A hora que acabou o teste, ele vai fazer a contagem, tá? Então, se você tiver
26:18
200.000 pagamentos enfilerados, esses 200.000 vão pro saco, você não vai, não
26:23
vão ser contabilizados. Então você vai querer fazer, você vai querer descarregar, vamos chamar assim, os os
26:30
pagamentos o mais rápido possível, né? Você pode de novo, né? Você pode fazer o
26:35
processamento assíncrono, só que tem tente enviar o mais rápido
26:40
possível pros pros payment processors, beleza?
26:46
Porque é o lucro, é a unidade básica, assim, a a pontuação básica, é o
26:52
dinheiro, tá? É o quanto de dinheiro que você conseguiu ter, né? O quanto de lucro você conseguiu ter. Então, quanto
26:59
mais requisições você conseguir enviar pro pro default, que tem a taxa menor, é
27:04
melhor, tá? E o mais rápido possível, tá? Então você vai você vai ter que fazer esse esse balanço aí, tá? Eh, aí
27:13
tem uma outra coisa aqui, lembra da questão da verificação do summary? Se
27:20
você tiver inconsistências durante o teste ou final do teste, você vai tomar
27:25
uma multa de 35% sobre o valor do lucro, tá bom? Então, é
27:31
uma uma multa salgada aqui, tá? Então é importante que você tenha que a sua peha
27:37
bastante consistência, senão você vai perder muita pontuação. E como a galera gosta de sangue, gosta
27:43
de performance na rinha, eu coloquei também um critério aqui de performance, que é o P99. E o P99 é basicamente a
27:52
gente, né, eh, o percentil 99 aqui a gente pega os o 1% de requisições a os
28:00
1% de requisições piores e mede o tempo de resposta ali, né?
28:07
Então, se você conseguir fazer com que seu P99
28:13
seja 10 msundos ou menos, você vai ganhar um bônus, tá bom? Um bônus
28:21
de 2% até, deixa eu ver aqui, 20%. Tá?
28:27
Então, a fórmula aqui, eu não vou não vou repassar a fórmula aqui, mas não vou vou repassar sim. Então, a fórmula é
28:33
essa aqui, né? os 11% porque é abaixo de 10 ms aqui. Isso aqui é os 10 msos, tá?
28:38
Isso aqui é milissundo. Então, se você tiver, por exemplo, aqui um P99
28:44
de, vamos falar aqui, 5 msegundos, você vai receber 12% de bônus, tá? Que é
28:52
isso aqui, né? Vezes os 2% aqui, tá? E bom, se se você tiver um P99 acima de
29:00
de 11, você não vai ser penalizado, tá bom? Isso aqui, o P99 é só ganho. Você não perde se você tiver um P99 alto.
29:09
H, é, é, é bom. É, basicamente essa, esse é o critério de pontuação que tinha
29:14
bastante gente perguntando, ah, mas como é que é? Eh, uma outra coisa, o script que tá
29:20
disponível aqui na rinha, o script de teste do que tá no repositório, não vai
29:27
ser o script final, porque senão fica muito fácil, né? porque ele mostra todos os intervalos,
29:34
quando eh com quantos segundos o default vai começar a ficar instável, com quantos segundos o ele vai começar a dar
29:43
erro. Aí não tem graça, né? Aí você faz uma uma configuração de tempo ali e vai
29:49
eh perde a graça. Então saiba que o script final não é o que tá aqui, tá? eh
29:55
é parecido o suficiente para você desenvolver uma estratégia dinâmica, mas ele não, mas você precisa,
30:04
eh você vai precisar desenvolver uma estratégia sabendo que é surpresa as
30:09
instabilidades e indisponibilidades. Ah, que mais aqui? Eu acho que é isso,
30:17
tá? Eh, o payment processor, ele tá containerizado aqui, né? com uns artefatos aqui. Ah, uma outra coisa
30:23
aqui, se você usa MAC, né, com o processador ARM 64, eh, usa essa imagem aqui, tá? Usa essa
30:30
imagem para pro P pro pro Pment Processo. E também se você usa MAC, na hora de submeter,
30:38
a rinha builda a imagem para Linux AMD64. Tem as instruções aqui no no
30:45
repositório da Rinha, como de como fazer isso. Beleza? Eh, deixa eu ver. Ah, e o código fonte
30:51
aqui do payment processor tem um link, tem um link em alguma instrução aqui. Acho que aqui.
30:58
Ah, não sei. Bom, dá uma fuçada aqui se você se inter, se você quiser olhar o código fonte do payment processor. É,
31:05
ele tá aqui. Ele tá aqui, ó. É só entrar aqui, dar uma olhada. Se
31:10
você encontrar alguma inconsistência, abre um PR também, me ajuda, beleza? Me
31:16
ajuda. Ah, e eu acho que é isso. Eu acho que é isso. E qualquer coisa eu faço um vídeo
31:22
complementar, vou vou tentando responder nas redes sociais. Eh, eu sou uma pessoa só. Ele tem o que aonde tem mais
31:31
informações sobre a rinha é no Twitter, né? No X. Eu não consigo ficar postando no LinkedIn, no Blue Sky. Eu tô
31:40
escolhendo o Twitter porque é onde tem mais gente que tá seguindo a Rinha ali, é onde tem mais eh acho que a maioria
31:47
das pessoas tão tão seguindo pelo pelo Twitter. E LinkedIn é mais complicado,
31:52
né? O formato do LinkedIn é ficar postando e não é mini blog, né? Imagina,
31:57
não dá para ficar postando muita coisa ali. Tem algumas pessoas que disseram que iam fazer um canal Discord ali. Eu
32:05
achei muito legal, mas eu tô muito ferrado de tempo com algumas coisas e não tô conseguindo dar vazão assim para
32:12
para todo mundo que tá entrando em contato, dando sugestão, querendo fazer parceria, esse tipo de coisa, tá? Mas eh
32:20
é isso. Eh, me ajuda, contribui, se diverte. E o importante é a gente se
32:26
divertir e aprender e ficar eh e dar umas risadas aqui com a Rinha. Valeu,
32:32
gente.

