---

Copyright:
  Years: 2017
lastupdated: "2017-09-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Conectando um aplicativo {{site.data.keyword.cloud_notm}}

Para conectar um aplicativo {{site.data.keyword.cloud}} a seu serviço, use as credenciais de serviço que são criadas quando o serviço é provisionado.

## Credenciais disponíveis

Campo de nome|Descrição
----------|-----------
`deployment_id`|Um identificador interno para o serviço conforme criado no Compose.
`name`|O nome da implementação do banco de dados.
`db_type`|O tipo de banco de dados que é oferecido pelo serviço; neste caso, `JanusGraph`.
`uri_cli`|Uma linha de comandos `CURL` que ilustra como enviar uma consulta do Gremlin com a autenticação baseada em token.
`uri_cli_1`|Uma linha de comandos `CURL` alternativa que se conecta à instância de banco de dados.
`maps`|não usado
`uri`|O URI `https` que é usado ao se conectar ao serviço, que inclui o nome do host do servidor e o número da porta à qual se conectar. Veja [Conectando um aplicativo externo](./connecting-external.html).
`uri_direct_1`|Um URI `https` alternativo para se conectar ao serviço.
`misc`|Um campo pai que contém os campos `session`, `websocket` e `gremlin_console_yaml`.
`misc.session`| Um URI `https` que um usa para recuperar um token de autenticação de 60 minutos que pode ser usado para interagir com o serviço. Veja [Conectando um aplicativo externo - autenticação do token](./connecting-external.html#token-authentication).
`misc.websocket`|Os URIs `wss` que são usados ao se conectar ao serviço usando Websockets. Veja [Conectando um aplicativo externo - Websockets](./connecting-external.html#websockets).
`misc.gremlin_console_yaml`|Informações do YAML que são usadas para configurar o console Gremlin para se conectar ao serviço. Veja [Conectando um aplicativo externo - Gremlin Console](./connecting-external.html#gremlin-console).
{: caption="Tabela 1. Credenciais do Compose for JanusGraph" caption-side="top"}

## Usando as credenciais

Seu aplicativo precisa ser conectado ao seu serviço para usar as credenciais de serviço. As etapas a seguir mostram como é possível fazer isso, usando Node.js como um exemplo.

### Conectando o aplicativo ao serviço

Se você já tem um aplicativo {{site.data.keyword.cloud_notm}}, é possível conectá-lo a um serviço de sua conta do {{site.data.keyword.cloud_notm}}:

1. Efetue login em sua conta do {{site.data.keyword.cloud_notm}}
2. No seu painel, selecione o aplicativo que você deseja conectar ao seu serviço.
3. No painel de conexões da página _Visão geral_ do aplicativo, clique em **Conectar novo** ou **Conectar existente** para se conectar a um serviço {{site.data.keyword.cloud_notm}} novo ou existente.

  - Se você clicou em **Conectar existente**, uma lista de serviços disponíveis é mostrada. Selecione o serviço ao qual você deseja conectar seu app e clique em **Conectar**.
  - Se você clicou em **Conectar novo**, siga as etapas para provisionar uma nova instância de serviço. Quando você selecionar um serviço do catálogo para provisionar, o campo _Conectar a _ será preenchido automaticamente com o nome do seu aplicativo.

Se você tiver um aplicativo que não foi transferido por upload para o {{site.data.keyword.cloud_notm}}, será possível ligar o aplicativo ao serviço antes de carregá-lo no {{site.data.keyword.cloud_notm}}: 

1. Efetue login em sua conta do {{site.data.keyword.cloud_notm}}
2. Faça download e instale a ferramenta [Cloud Foundry CLI](https://github.com/cloudfoundry/cli)
3. Conecte-se ao {{site.data.keyword.cloud_notm}} na ferramenta de linha de comandos e siga os prompts para efetuar login.

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

4. Abra o arquivo `manifest.yml` do seu aplicativo.

  - Mude o valor `host` para algo exclusivo. O host que você escolher determinará o subdomínio da URL do seu aplicativo: `<host>.mybluemix.net`.
  - Mude o valor `name`. O valor que você escolher será o nome do app como aparece em seu painel do {{site.data.keyword.cloud_notm}}.

5. Atualize o valor `services` em `manifest.yml` para corresponder ao nome de seu serviço. O `manifest.yml` agora será semelhante a este:

  ```
  ---
  applications:
  - name:    compose-janusgraph-sample-service
    host:    compose-janusgraph-sample-service
    memory:  256M
    services:
      - compose-janusgraph-sample-app
  ```

### Acessando e usando as credenciais

Seu aplicativo precisa analisar as variáveis de ambiente do Cloud Foundry. No Node.js, é possível fazer isso com o pacote [cfenv](https://www.npmjs.com/package/cfenv).

1. Instale o pacote com `--save` para atualizar o `package.json`:

  ```
  npm install --save cfenv
  ```

2. Extraia as credenciais do {{site.data.keyword.composeForJanusGraph_full}} das variáveis de ambiente do Cloud Foundry.

  ```javascript
  const cfenv = require('cfenv');
  const appenv = cfenv.getAppEnv();

  // Within the application environment (appenv) there's a services object
  const services = appenv.services;

  // The services object is a map named by service so extract the one for JanusGraph
  let janusgraph_services = services["compose-for-janusgraph"];

  // We now take the first bound JanusGraph service and extract its credentials object
  let credentials = janusgraph_services[0].credentials;
  ```

  Agora é possível usar as credenciais de serviço. Por exemplo, para obter um URI de sessão que é possível usar para obter um token de autenticação:

  ```javascript
  let jgurl = credentials.misc.session[0];
  ```

3. Envie por push o app para o {{site.data.keyword.cloud_notm}}. Quando você enviar por push o app, ele será automaticamente ligado ao serviço.

  ```
  $ cf push
  ```

Para obter mais informações sobre o uso de aplicativos com serviços do {{site.data.keyword.cloud_notm}}, veja [Incluindo um serviço em seu app](https://console.bluemix.net/docs/services/reqnsi.html#add_service).
