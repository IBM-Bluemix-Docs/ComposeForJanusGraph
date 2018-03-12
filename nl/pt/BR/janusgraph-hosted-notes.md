---
copyright:
  years: '2015, 2017'
lastupdated: '2017-11-21'
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Notas hospedadas do JanusGraph

O {{site.data.keyword.composeForJanusGraph_full}} é um serviço hospedado, portanto, assegurar que as implementações são seguras difere em alguns casos da execução do JanusGraph em um servidor dedicado. Este tópico destaca essas diferenças e variações. A maioria dessas mudanças afeta como as consultas de linguagem Gremlin funcionam.

## Ambientes de simulação

Para assegurar que as consultas Gremlin não acessem a funcionalidade que poderia comprometer o sistema, todas as consultas são executadas em um ambiente de simulação. Isso significa que todas as chamadas de função e método devem ter uma assinatura que corresponda a uma das expressões regulares a seguir:

```
java\.util.*
java\.lang\.Object.*
java\.lang\.Class <T extends java\.lang\.Object>#getSimpleName\(
java\.lang\.Iterable <T extends java\.lang\.Object>#iterator\(\)
java\.lang\.Boolean(?!#getBoolean\().*
java\.lang\.Double.*
java\.lang\.Float.*
java\.lang\.Integer(?!#getInteger\().*
java\.lang\.Long(?!#getLong\().*
java\.lang\.Math.*
java\.lang\.String(?!#intern\(\)).*
java\.lang\.StringBuilder.*
java\.lang\.Object#equals\(
java\.lang\.Object#hashCode\(
java\.util\.ArrayList#equals\(

org\.codehaus\.groovy\.runtime\.DefaultGroovyMethods.*
org\.codehaus\.groovy\.runtime\.StringGroovyMethods.*
org\.apache\.commons\.configuration\.MapConfiguration.*
org\.apache\.tinkerpop\.gremlin\.structure(?!\.io|\.Graph#toString\(|\.Graph#variables\(|\.Graph#configuration\(|\.Graph#io\(\)|\.util\.GraphFactory.*).*
org\.apache\.tinkerpop\.gremlin\.process\.traversal.*
org\.apache\.tinkerpop\.gremlin\.util\.Gremlin#version\(\)
org\.janusgraph\.core(?!\.JanusGraphFactory.*).*
org\.janusgraph\.graphdb(?!\.tinkerpop\.JanusGraphBlueprintsGraph#toString\(|\.tinkerpop\.JanusGraphBlueprintsGraph#variables\(|\.tinkerpop\.JanusGraphBlueprintsGraph#configuration\(|\.tinkerpop\.JanusGraphBlueprintsGraph#io\().*
org\.janusgraph\.example\.GraphOfTheGodsFactory(?!#create\(|#main\().*
org\.apache\.tinkerpop\.gremlin\.groovy\.plugin\.dsl\.credential\.CredentialGraph.*

Script\d+#.+\(?.+\)
groovy\.lang\.Closure <V extends java\.lang\.Object>#call\(?.+\)

org\.janusgraph\.util\.stats\.MetricManager#getRegistry\(\)
org\.janusgraph\.util\.stats\.MetricManager#INSTANCE
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#getRegistry\(\)
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#INSTANCE
```

Tentar executar uma chamada de função ou método que não corresponda a nenhuma das entradas na lista de desbloqueio de funções resulta em um erro: 

```
[Static type checking] - Not authorized to call this method: ...
```

O ambiente de simulação também requer que todas as variáveis sejam declaradas estaticamente para ativar a verificação de segurança. Isso pode ser feito com o comando `def`.

## Sessões

Uma sessão permite que uma conexão com o {{site.data.keyword.composeForJanusGraph}} mantenha o estado entre as solicitações. A maioria das opções de conexão com o {{site.data.keyword.composeForJanusGraph}} não possui sessões associadas a elas. Isso significa que qualquer script Gremlin enviado por meio desses métodos de conexão precisa ser totalmente autocontido. Por exemplo, abrir qualquer gráfico com o qual o script precisa trabalhar, consultar nós apropriados e atravessar o gráfico por meio desses nós precisariam todos ser manipulados em uma solicitação. Essa restrição se aplica a ambas as solicitações de HTTP e conexões WebSockets. 

A exceção a isso é quando você faz a conexão por meio do console Gremlin usando `:remote connect`.

```
:remote connect tinkerpop.server conf/compose.yaml
```

Se você fizer a conexão usando o comando acima não haverá sessão e a conexão operará da mesma maneira que as solicitações de HTTP. Mas se você incluir `session` como argumento, o suporte de sessão agora será ativado.

```
:remote connect tinkerpop.server conf/compose.yaml session
```

Usando uma sessão, será possível definir uma variável em um comando e consultá-la em outro comando.
{: .tip}

## Transações

Todas as mudanças no banco de dados subjacente do JanusGraph são encapsuladas em uma transação. As transações permitem que quaisquer mudanças entre elas sejam confirmadas para torná-las permanentes ou recuperadas para assegurar que elas não. 

Ao fazer uma solicitação por meio de uma solicitação de HTTP ou sobre um WebSocket, uma transação é iniciada automaticamente quando você faz uma mudança que seria gravada no banco de dados. Essa transação também é confirmada automaticamente quando a solicitação está completa.

A exceção é quando você está se conectando usando o Console do Gremlin com sessão ativada. A sessão pode ser de longa duração, portanto, cabe ao usuário confirmar as mudanças chamando `graph.tx().commit()`, em que `graph` é o gráfico aberto que contém as mudanças. Isso também significa que é possível chamar `graph.tx().rollback()` para voltar para o estado do gráfico antes do início da transação. 

## Índices combinados

A versão atual do {{site.data.keyword.composeForJanusGraph}} não suporta índices combinados.
