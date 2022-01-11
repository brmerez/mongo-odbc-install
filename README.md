#  Setup ODBC MongoDB - Passo a passo

## Pré-requisitos

- Gerenciador de ODBC (`ODBC Manager` no Mac e `Administrador de fonte de dados ODBC` no Windows)
- **Importante:** No Windows, utilize a versão 32-bit para conseguir usar uma conexão ODBC no Excel 
- [Drivers ODBC do Mongo](https://github.com/mongodb/mongo-bi-connector-odbc-driver/releases/)
- [MongoDB Connector for BI](https://www.mongodb.com/try/download/bi-connector)

-----

## Passo a Passo

### 1. Instalar os Drivers do Mongo
- Esse passo é bem auto-explicativo, é só seguir os passos da instalação.

### 2. Configurando o driver no ODBC Manager

- Abrindo o ODBC Manager, na aba `Drivers` podemos `Adicionar` um novo Driver

- Em `Caminho do Driver` clique em `Escolher` e navegue até a pasta onde forma instalados os drivers
    - Segundo a instalação padrão do MacOS, os drivers devem estar instalados na pasta `/Library/MongoDB/ODBC/{número da versão}/`
    - Nesta pasta haverão 2 arquivos: `libmdbodbca.so` e `libmdbodbcw.so`, cada uma é referente à versão ANSI e Unicode.

- Em seguida, precisaremos configurar a conexão na aba `DSN de Sistema` (ou `DSN de Usuário` no Windows, os passos são os mesmos)

- Clicando em `Adicionar`, selecione o Driver que acabamos de adicionar.
    - Podem haver 2 opções: `MongoDB ANSI ODBC` e `MongoDB Unicode ODBC`, qualquer uma das duas é válida.

- Escreva o nome no campo `Nome da Fonte de Dados`
    - Cada DSN contempla __uma__ base de dados, então é recomendável que coloque o nome da base de dados que pretendemos acessar.

(MacOS)
- Na seção com as colunas `Palavra-chave` e `Valor`, iremos adicionar respectivamente em cada coluna:
    - Palavra-chave:`SERVER` Valor: `127.0.0.1`.
    - Palavra-chave:`PORT` Valor: `3307`.
    - Palavra-chave:`DATABASE` Valor: [nome da base de dados do MongoDB que queremos acessar](ex: `cupons`)

(Windows)
- No campo `TCP/IP Server`, inserir `127.0.0.1`
- No campo `Port`, inserir `3307`
- No campo `Database`, inserir o nome da base de dados que queremos acessar (ex: `cupons`)
- Clique no botão `Test`, e uma confirmação deve aparecer com uma mensagem de sucesso.

### 3. Utilizando o Connector for BI (mongosqld)

- Uma vez configurado o ODBC Manager, toda vez que quisermos utilizar a conexão ODBC, precisamos rodar o programa `mongosqld`, encontrado na pasta onde baixamos o Connector for BI
- Para isso, precisaremos utilizar o terminal.

- (MacOS / Linux):
    - Precisaremos instalar uma biblioteca chamada `openssl@1.1`
    - O programa pode pedir uma senha durante o processo de instalação.
    - Para tanto, devemos instalar o homebrew, um gerenciador de pacotes para o Mac disponível no link https://brew.sh/index_pt-br
    - Após instalar o programa, devemos inserir no terminal:<br>
    `brew install openssl@1.1`
    - Depois disso, rodar o seguinte comando:<br>
    `echo 'export PATH="/usr/local/opt/openssl@1.1/bin:$PATH"' >> ~/.zshrc`
    
- Abrindo o terminal, precisaremos entrar na pasta onde está instalado o connector for BI

- Abra a pasta do Connector for BI, arraste a pasta `bin` para o terminal e aperte enter.

- Agora vamos rodar o seguinte comando no terminal:

    `./mongosqld --mongo-uri=mongodb://{endereço de IP do MongoDB}` no MacOS/Linux

    `mongosqld.exe --mongo-uri=mongodb://{endereço de IP do MongoDB}` no Windows

- Se tudo der certo, deverá aparecer no texto do terminal os nomes das bases do MongoDB, algo como `['cupons','prods', 'orders']`
- __**IMPORTANTE:**__ **Não feche a janela do terminal rodando o comando `mongosqld` enquanto usa a conexão ODBC**

