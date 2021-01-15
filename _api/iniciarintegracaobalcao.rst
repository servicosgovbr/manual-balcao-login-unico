Iniciando a Integração da API do Balcão
=======================================

Utilização
----------

Para a utilização da API de cadastro remoto de contas no Acesso gov.br será necessário que a aplicação cliente adquira inicialmente um access token. O access token pode ser utilizado por uma hora.

A utilização da API de cadastro remoto de contas no Acesso gov.br depende dos seguintes passos:

1. Obter o *ticket de acesso* por meio da requisição POST para o endereço https://sso.staging.acesso.gov.br/token tendo passando as seguintes informações:

Parâmetros do Header para requisição Post https://sso.staging.acesso.gov.br/token

=================  ======================================================================
**Variavél**  	   **Descrição**
-----------------  ----------------------------------------------------------------------
**Content-Type**   Tipo do conteúdo da requisição que está sendo enviada. O padrão será **application/x-www-form-urlencoded**
**Authorization**  Palavra **Basic** e informação codificada em *Base64*, no seguinte formato: CLIENT_ID:CLIENT_SECRET (senha de acesso do serviço consumidor)(utilizar `codificador para Base64`_ para gerar codificação). A palavra Basic deve está antes da informação. 
=================  ======================================================================
	
Exemplo de *header*:

.. code-block:: console

	Content-Type:application/x-www-form-urlencoded
	Authorization: Basic											
	ZWM0MzE4ZDYtZjc5Ny00ZDY1LWI0ZjctMzlhMzNiZjRkNTQ0OkFJSDRoaXBfTUJYcVJkWEVQSVJkWkdBX2dRdjdWRWZqYlRFT2NWMHlFQll4aE1iYUJzS0xwSzRzdUVkSU5FcS1kNzlyYWpaZ3I0SGJuVUM2WlRXV1lJOA==

Parâmetros da Query para requisição Post https://sso.staging.acesso.gov.br/token
	
==========================  ======================================================================
**Variavél**  	            **Descrição**
--------------------------  ----------------------------------------------------------------------
**grant_type**              Especifica para o provedor o tipo de autorização. Neste caso será **client_credentials**
**client-id**               Identificação da aplicação para acessar API
**client-secret**           Senha para acessar API
**scope**                   Escopo especiﬁco para API de pré-cadastro
==========================  ======================================================================

Exemplo de *query*

.. code-block:: console

	https://sso.staging.acesso.gov.br/token?grant_type=client_credentials&client-id=minha-aplicacacao&client-secret=123456&scope=sistema_govbr_preregistro_balcao_XXX	

O serviço retornará, em caso de sucesso, no formato JSON, as informações conforme exemplo:

.. code-block:: JSON

	{ 
		"access_token": "(Token de acesso a API de pré-cadastro.)", 
		"token_type": "(O tipo do token gerado. Padrão: Bearer)", 
		"expires_in": "(Tempo de vida do token em segundos.)",
		"scope": "(Escopo liberado para API de pré-cadastro)"
	} 

2. De posse das informações do json anterior, a aplicação consumidora está habilitada para acessar API de Pré-Cadastro por meio da requisição PUT para o endereço https://api.staging.acesso.gov.br/pre-registro/v2/contas/**cpf** passando as seguintes informações: 

Parâmetros do Header para requisição PUT https://api.staging.acesso.gov.br/pre-registro/v2/contas/**cpf**

=================  ======================================================================
**Variavél**  	   **Descrição**
-----------------  ----------------------------------------------------------------------
**Content-Type**   Tipo do conteúdo da requisição que está sendo enviada. O padrão será **application/json**
**Authorization**  Palavra **Bearer** e o *ACCESS_TOKEN* da requisição POST do https://sso.staging.acesso.gov.br/token 
=================  ======================================================================
	
Exemplo de *header*:

.. code-block:: console

	Content-Type:application/json
	Authorization: Bearer											
	ZWM0MzE4ZDYtZjc5Ny00ZDY1LWI0ZjctMzlhMzNiZjRkNTQ0OkFJSDRoaXBfTUJYcVJkWEVQSVJkWkdBX2dRdjdWRWZqYlRFT2NWMHlFQll4aE1iYUJzS0xwSzRzdUVkSU5FcS1kNzlyYWpaZ3I0SGJuVUM2WlRXV1lJOA==

Parâmetros da Query para requisição PUT https://api.staging.acesso.gov.br/pre-registro/v2/contas/**cpf**
	
==========================  ======================================================================
**Variavél**  	            **Descrição**
--------------------------  ----------------------------------------------------------------------
**cpf**                     CPF do usuário. Padrão do formato: **apenas números**
==========================  ======================================================================

Corpo JSON para requisição com os atributos:

.. code-block:: JSON

	{
		"nome":"Nome do cidadão. Não poderá ter caracteres especiais. Item obrigatório", 
		"telefone":"Telefone do cidadão. Deverá ter apenas números e quantidade 11. Item opcional",
		"email":"Endereço de e-mail do cidadão. Respeitar sintaxe padrão de email. Item opcional",
		"senha":"Senha do cidadão. Deverá está em texto claro. Item obrigatório",
		"codigosConfiabilidades": ["Identificação do nome do selo a ser representado pelo balcão. Informação será passada pela SGD. Item obrigatório"],
		"dadosAtendimento":
		{
			"protocoloAtendimento":"Identificação de atendimento gerado no balcão do órgão para cadastro do cidadão. Item obrigatório",
			"nomeAtendente":"Nome do operador responsável pelo cadastro do cidadão em balcão",
			"numeroDocumentoAtendente":"Número do documento fornecido pelo operador responsável pelo cadastro da conta em balcão",
			"tipoDocumentoAtendente":"Tipo de documento fornecido pelo operador responsável pelo cadastro da conta em balcão",
			"dataHoraAtendimento":"Data e Hora do atendimento para cadastro da conta em balcão. Formato yyyy-MMdd'T'HH:mm:ss.SSS"
		}
	}

Exemplo de *query*

.. code-block:: console

	https://api.staging.acesso.gov.br/pre-registro/v2/contas/12345678980	

Resultados Esperados ou Erros 
-----------------------------

Os acessos aos serviços do Login Único ocorrem por meio de chamadas de URLs e as respostas são códigos presentes conforme padrão do protocolo http por meio do retorno JSON, conforme exemplo:

.. code-block:: JSON

  {
	"codigo": "(Código HTTP do erro)",
	"descricao": "(Descrição detalhada do erro ocorrido. )"
  }

.. _`codificador para Base64`: https://www.base64decode.org/
.. _`Exemplos de Integração`: exemplointegracao.html