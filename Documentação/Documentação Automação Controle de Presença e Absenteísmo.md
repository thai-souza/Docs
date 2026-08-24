# Introdução
Com o objetivo de otimizar o processo de consolidação das informações e aprimorar a organização dos dados relacionados ao controle de presença e absenteísmo da operação, foram desenvolvidas duas automações utilizando as ferramentas **Microsoft Forms, Microsoft Power Automate e Microsoft Excel**.

A primeira automação é responsável por **armazenar automaticamente as respostas enviadas por meio do formulário em uma tabela do Excel**, garantindo que as informações sejam centralizadas e estruturadas para posterior consulta e análise.

A segunda automação tem como objetivo **realizar o controle diário dos gestores que responderam ao formulário**, identificando os envios realizados no dia e atualizando uma planilha de acompanhamento com o nome do gestor e seu respectivo status, classificando-o como **"Enviado"** ou **"Pendente"**, conforme o prazo estabelecido para o envio.

Dessa forma, as automações reduzem a necessidade de atividades manuais, aumentam a confiabilidade das informações e facilitam o acompanhamento diário do preenchimento do formulário.

 ---
# Automação 1 - Adição de Respostas do Formulário em uma Tabela

## Objetivo
A primeira automação tem como finalidade **registrar automaticamente, em uma tabela do Microsoft Excel, as informações preenchidas pelos usuários no formulário "Controle de Presença e Absenteísmo"**.

Sempre que uma nova resposta é enviada, o fluxo é acionado, coleta os detalhes da resposta e insere as respectivas informações na tabela **"RespostasForms"**.

## Funcionamento da Automação
O fluxo é composto pelas seguintes etapas:

1. **Quando uma nova resposta é enviada**  
    Gatilho responsável por iniciar a execução da automação sempre que uma nova resposta é registrada no formulário **"Controle de Presença e Absenteísmo"**.
2. **Obter os detalhes da resposta**  
    Recupera todas as informações preenchidas pelo usuário na resposta correspondente ao formulário.
3. **Adicionar uma linha em uma tabela**  
    Insere as informações obtidas na etapa anterior na tabela **"RespostasForms"**, relacionando cada resposta à sua respectiva coluna.

![[Documentação Automação Controle de Presença e Absenteísmo-1787051752767.webp]]

## Configuração - Passo a Passo
### Configuração no Microsoft Excel
Primeiramente, deve ser criada a estrutura que receberá as respostas do formulário.

1. Criar uma nova pasta de trabalho em branco no Microsoft Excel.
2. Na primeira linha da planilha, inserir os seguintes cabeçalhos, sendo cada informação posicionada em uma coluna:
    
    **Email, Data, Nome Completo, Gestor Responsável, Setor, Tipo de Expedição, Turno, Assistente, Ajudante, Auxiliar, Analista, Aprendiz, Empilhador, Paleteiro, Inspetor, Supervisor, Ausência, Atestado, Afastado, Folga e Férias.**
    
3. Selecionar o intervalo que contém os cabeçalhos e formatá-lo como **Tabela**.
4. Na janela de configuração, selecionar a opção **"Minha tabela tem cabeçalhos"**.
5. Definir o nome da tabela como **"RespostasForms"**.

Essa tabela será utilizada pelo Power Automate como destino para o armazenamento das respostas recebidas pelo formulário.
   
## Configuração no Microsoft Power Automate
### 1. Criar o fluxo
1. Acessar o Microsoft Power Automate.
2. Selecionar a opção **"Criar"**.
3. Escolher **"Fluxo da nuvem automatizado"**.
4. Inserir o nome do fluxo.
5. Selecionar o gatilho **"Quando uma nova resposta é enviada"**, da conexão do Microsoft Forms.

### 2. Configurar o gatilho "Quando uma nova resposta é enviada"
No gatilho, configurar o seguinte parâmetro:

- **ID de formulário:** selecionar o formulário utilizado para a coleta das informações, neste caso, **"Controle de Presença e Absenteísmo"**.

Esse gatilho será responsável por iniciar o fluxo automaticamente sempre que uma nova resposta for enviada.

### 3. Adicionar a ação "Obter os detalhes da resposta"
Adicionar uma nova ação denominada **"Obter os detalhes da resposta"**.

Configurar os seguintes parâmetros:

- **ID de formulário:** selecionar o formulário **"Controle de Presença e Absenteísmo"**.
- **ID da resposta:** selecionar o conteúdo dinâmico **"ID da resposta"**, disponibilizado pela etapa anterior.

O conteúdo dinâmico pode ser selecionado por meio do menu de informações da ação anterior, representado pelo símbolo de raio (**⚡**).

Essa etapa é responsável por recuperar os dados preenchidos pelo usuário na resposta enviada.

### 4. Adicionar a ação "Adicionar uma linha em uma tabela"
Adicionar uma nova ação denominada **"Adicionar uma linha em uma tabela"**.

Configurar os seguintes parâmetros:

- **Localização:** SharePoint Site – Sala PCP – Online/A.B.S;
- **Biblioteca de Documentos:** Data Lakehouse;
- **Arquivo:** `/Compartilhados/Controle de Presença/Automação/RespostasForms.xlsx`;
- **Tabela:** RespostasForms.

Após selecionar a tabela, serão disponibilizadas as respectivas colunas para preenchimento.

Nos **Parâmetros Avançados**, configurar cada coluna da tabela para receber o campo correspondente à resposta obtida na etapa **"Obter os detalhes da resposta"**.

Realizar o mapeamento entre os campos do formulário e as colunas da tabela, contemplando:

- Email;
- Data;
- Nome Completo;
- Gestor Responsável;
- Setor;
- Tipo de Expedição;
- Turno;
- Assistente;
- Ajudante;
- Auxiliar;
- Analista;
- Aprendiz;
- Empilhador;
- Paleteiro;
- Inspetor;
- Supervisor;
- Ausência;
- Atestado;
- Afastado;
- Folga;
- Férias.

Cada campo deve ser preenchido utilizando o respectivo **conteúdo dinâmico** disponibilizado pela ação **"Obter os detalhes da resposta"**.

Após a configuração, sempre que um usuário enviar uma nova resposta pelo formulário, o Power Automate executará o fluxo automaticamente e adicionará uma nova linha na tabela **"RespostasForms"**, mantendo os dados centralizados e organizados no arquivo Excel.

---
# Automação 2 – Controle Diário do Status de Envio do Formulário pelos Gestores
## Objetivo
A segunda automação tem como finalidade **realizar o controle diário do envio do formulário pelos gestores**, identificando quais gestores realizaram o preenchimento no dia corrente e quais ainda não efetuaram o envio.

Para isso, a automação utiliza três tabelas principais:

- **Gestores:** contém a relação de gestores que devem realizar o preenchimento diário do formulário;
- **RespostasForms:** armazena todas as respostas recebidas por meio do formulário;
- **Controle:** registra diariamente o nome de cada gestor e seu respectivo status de envio, classificando-o como **"Enviado"** ou **"Pendente"**.

A cada execução, o fluxo inicialmente remove os registros existentes na tabela **"Controle"** e, em seguida, realiza uma nova verificação com base nas respostas registradas no dia atual. Dessa forma, a tabela mantém apenas o status mais recente de cada gestor.

## Funcionamento da Automação
O fluxo é executado de forma programada em horários predefinidos e segue as seguintes etapas:

1. **Recorrência**  
    Gatilho responsável por iniciar automaticamente o fluxo nos horários configurados.
2. **Listar linhas presentes – Tabela Controle**  
    Recupera os registros atualmente existentes na tabela **"Controle"**.
3. **Aplicar a cada – Registros da Tabela Controle**  
    Percorre cada registro existente na tabela **"Controle"**, permitindo que os dados do controle anterior sejam removidos.
4. **Excluir uma linha**  
    Exclui, individualmente, cada registro existente na tabela **"Controle"**. Essa etapa tem como objetivo limpar os dados do controle anterior para que a tabela seja atualizada somente com as informações referentes ao dia atual.
5. **Listar linhas presentes – Tabela RespostasForms**  
    Recupera todos os registros armazenados na tabela **"RespostasForms"**.
6. **Matriz do filtro – Data do Dia**  
    Filtra os registros da tabela **"RespostasForms"**, mantendo somente as respostas cuja data corresponde ao dia atual.
7. **Listar linhas presentes – Tabela Gestores**  
    Recupera a relação de gestores cadastrados na tabela **"Gestores"**.
8. **Aplicar a cada – Gestores**  
    Percorre cada gestor cadastrado e verifica se existe uma resposta associada a ele na lista de respostas realizadas no dia.
9. **Matriz do filtro – Gestor**  
    Compara o gestor que está sendo processado com o campo **"Gestor Responsável"** presente nas respostas filtradas do dia.
10. **Matriz do filtro – Controle**  
    Verifica se já existe um registro do gestor na tabela **"Controle"**.
11. **Condição – Verificação do envio**  
    Analisa a quantidade de registros encontrados para o gestor:
    - **Verdadeiro:** caso exista pelo menos um registro, o gestor é considerado **"Enviado"** e uma nova linha é adicionada à tabela **"Controle"**;
    - **Falso:** caso não exista nenhum registro, o gestor é considerado **"Pendente"** e uma nova linha é adicionada à tabela **"Controle"**.

Ao final da execução, a tabela **"Controle"** apresentará todos os gestores cadastrados e o respectivo status de envio referente ao dia atual.

![[Documentação Automação Controle de Presença e Absenteísmo-1787577255109.webp]]

## Configuração – Passo a Passo
### Configuração no Microsoft Excel
Para o funcionamento da automação, são utilizadas duas pastas de trabalho: **"CadastroGestores"** e **"ControleDiario"**.

#### A. Cadastro dos gestores
Criar uma pasta de trabalho denominada **"CadastroGestores"**. Essa planilha será utilizada como base para identificar todos os gestores que devem realizar diariamente o envio do formulário.

A tabela servirá como referência para comparação com os registros armazenados na tabela **"RespostasForms"**.

Configurar a planilha da seguinte forma:

1. Criar uma nova pasta de trabalho denominada **"CadastroGestores"**.
2. Criar uma tabela com uma única coluna denominada **"Gestor"**.
3. Inserir nessa coluna o nome de todos os gestores que devem realizar o preenchimento do formulário.
4. Definir o nome da tabela como **"Gestores"**.

> **Importante:** a relação de gestores cadastrada nessa tabela será utilizada como base para determinar quais pessoas devem apresentar o status **"Enviado"** ou **"Pendente"** no controle diário.

#### B. Controle diário
Criar uma nova pasta de trabalho denominada **"ControleDiario"**. Essa planilha será responsável por armazenar o resultado da verificação diária realizada pelo Power Automate.

Configurar a planilha da seguinte forma:

1. Criar uma nova pasta de trabalho denominada **"ControleDiario"**.
2. Criar uma tabela com as seguintes colunas:
    - **Gestor**
    - **Status**
3. Definir o nome da tabela como **"Controle"**.
4. Não é necessário inserir previamente os nomes dos gestores nessa tabela, pois os registros serão adicionados automaticamente pela automação.

### Configuração no Microsoft Power Automate
#### 1. Criar o fluxo
No Microsoft Power Automate:

1. Selecionar **"Criar"**.
2. Selecionar **"Fluxo da nuvem agendado"**.
3. Inserir o nome do fluxo.
4. Definir a data de início da execução.
5. Definir o horário de início.
6. Configurar a frequência de repetição do fluxo.

A automação será executada automaticamente nos horários definidos no gatilho **"Recorrência"**.

#### 2. Configurar o gatilho "Recorrência"
No gatilho **"Recorrência"**, configurar os seguintes parâmetros:

- **Intervalo:** 1;
- **Frequência:** Dia;
- **Fuso horário:** `(UTC-03:00) Brasília`;
- **Nestas horas:** 8, 16;
- **Nestes minutos:** 5, 30.

> **Observação:** na definição dos horários, os valores separados por vírgula representam diferentes horários de execução. Com essa configuração, o fluxo será executado diariamente às **08:05, 08:30, 16:05 e 16:30**.

#### 3. Listar os registros da tabela "Controle"
Adicionar a ação **"Listar linhas presentes em uma tabela"** para recuperar os registros atualmente existentes na tabela de controle.

Configurar os seguintes parâmetros:

- **Localização:** SharePoint Site – Sala PCP – Online/A.B.S;
- **Biblioteca de Documentos:** Data Lakehouse;
- **Arquivo:** `/Compartilhados/Controle de Presença/Automação/ControleDiario.xlsx`;
- **Tabela:** Controle.

Essa etapa permite identificar os registros gerados pela execução anterior da automação.

#### 4. Percorrer os registros existentes
Adicionar a ação **"Aplicar a cada"**.

No campo **"Selecionar uma saída das etapas anteriores"**, utilizar o conteúdo dinâmico **body/value** proveniente da ação **"Listar linhas presentes – Tabela Controle"**.

Essa ação será responsável por percorrer individualmente cada registro existente na tabela **"Controle"**.

#### 5. Excluir os registros anteriores
Dentro da ação **"Aplicar a cada"**, adicionar a ação **"Excluir uma linha"**.

Configurar os parâmetros:

- **Localização:** SharePoint Site – Sala PCP – Online/A.B.S;
- **Biblioteca de Documentos:** Data Lakehouse;
- **Arquivo:** `/Compartilhados/Controle de Presença/Automação/ControleDiario.xlsx`;
- **Tabela:** Controle;
- **Coluna de Chave:** Gestor;
- **Valor da Chave:** utilizar a expressão:

`item()?['Gestor']`

A expressão retorna o nome do gestor referente ao registro que está sendo processado pelo loop.

Essa etapa é responsável por **limpar os registros da execução anterior**, permitindo que a tabela seja preenchida novamente com os resultados atualizados.

#### 6. Listar os registros da tabela "RespostasForms"
Após a exclusão dos registros anteriores, adicionar uma nova ação **"Listar linhas presentes em uma tabela"**.

Configurar:

- **Localização:** SharePoint Site – Sala PCP – Online/A.B.S;
- **Biblioteca de Documentos:** Data Lakehouse;
- **Arquivo:** `/Compartilhados/Controle de Presença/Automação/RespostasForms.xlsx`;
- **Tabela:** RespostasForms.

Essa ação recupera todas as respostas armazenadas no arquivo **"RespostasForms.xlsx"**.

#### 7. Filtrar as respostas do dia atual
Adicionar a ação **"Matriz do filtro"** para selecionar somente as respostas referentes ao dia atual.

Configurar:

- **From:** utilizar **body/value** da ação **"Listar linhas presentes – Tabela RespostasForms"**;
- **Filter Query:** selecionar o modo avançado e configurar a comparação entre a data da resposta e a data atual.

Como o campo **"Data"** está armazenado no Excel em formato numérico de data serial, é necessário convertê-lo para uma data no formato `yyyy-MM-dd` antes da comparação.

**Data da resposta:**

```
formatDateTime(
    addDays(
        '1899-12-30',
        int(
            float(
                item()?['Data']
            )
        )
    ),
    'yyyy-MM-dd'
)
```

**Data atual:**

```
formatDateTime(
    convertTimeZone(
        utcNow(),
        'UTC',
        'E. South America Standard Time'
    ),
    'yyyy-MM-dd'
)
```

A condição do filtro deve verificar se a **data da resposta é igual à data atual**.

Dessa forma, somente as respostas realizadas no dia da execução permanecerão no resultado do filtro.

#### 8. Listar os gestores cadastrados
Adicionar uma nova ação **"Listar linhas presentes em uma tabela"** para recuperar os gestores que devem realizar o preenchimento diário.

Configurar:

- **Localização:** SharePoint Site – Sala PCP – Online/A.B.S;
- **Biblioteca de Documentos:** Data Lakehouse;
- **Arquivo:** `/Compartilhados/Controle de Presença/Automação/CadastroGestores.xlsx`;
- **Tabela:** Gestores.

Essa etapa fornece a lista de gestores que será utilizada como base para a verificação dos envios.

#### 9. Percorrer a lista de gestores
Adicionar uma ação **"Aplicar a cada"**.

No campo **"Selecionar uma saída das etapas anteriores"**, utilizar o conteúdo **body/value** da ação **"Listar linhas presentes – Tabela Gestores"**.

A partir desse ponto, a automação processará cada gestor individualmente.

#### 10. Filtrar as respostas pelo gestor
Dentro da ação **"Aplicar a cada"**, adicionar uma nova ação **"Matriz do filtro"**.

Essa etapa tem como objetivo verificar se o gestor que está sendo processado possui alguma resposta registrada no conjunto de respostas realizadas no dia atual.

Configurar:

- **From:** utilizar o **body** da ação **"Matriz do filtro – Data do Dia"**;
- **Filter Query:** selecionar o modo avançado e comparar os seguintes valores:

**Valor da resposta:**
`item()?['Gestor Responsável']`

**Valor do gestor atual:**
`items('Aplicar_a_cada')?['Gestor']`

A condição deve verificar se o campo **"Gestor Responsável"** da resposta é igual ao nome do gestor que está sendo processado no loop.

#### 11. Listar os registros da tabela "Controle"
Adicionar uma nova ação **"Listar linhas presentes em uma tabela"**.

Configurar:

- **Localização:** SharePoint Site – Sala PCP – Online/A.B.S;
- **Biblioteca de Documentos:** Data Lakehouse;
- **Arquivo:** `/Compartilhados/Controle de Presença/Automação/ControleDiario.xlsx`;
- **Tabela:** Controle.

Essa etapa permite consultar os registros atualmente existentes na tabela de controle.

#### 12. Filtrar o gestor na tabela "Controle"
Adicionar uma nova ação **"Matriz do filtro"**.

Configurar:

- **From:** utilizar o conteúdo **body/value** da ação **"Listar linhas presentes – Tabela Controle 2"**;
- **Filter Query:** comparar:

**Gestor registrado na tabela:**
`item()?['Gestor']`

**Gestor atualmente processado:**
`items('Aplicar_a_cada')?['Gestor Responsável']`

Essa etapa verifica se o gestor já possui um registro na tabela **"Controle"**.

#### 13. Verificar o status de envio
Adicionar a ação **"Condição"** para determinar se o gestor realizou o envio do formulário.

Configurar a expressão para verificar se a quantidade de registros encontrados no filtro é maior que zero:

`length(body('Matriz_do_filtro_1'))` **é maior que** `0`

##### Se o resultado for verdadeiro
Caso a quantidade de registros seja maior que zero, significa que foi encontrada pelo menos uma resposta do gestor no dia atual.

Nesse cenário:

- Adicionar uma linha na tabela **"Controle"**;
- Preencher a coluna **"Gestor"** com o nome do gestor;
- Preencher a coluna **"Status"** com **"Enviado"**.

##### Se o resultado for falso
Caso a quantidade de registros seja igual a zero, significa que não foi encontrada nenhuma resposta do gestor no dia atual.

Nesse cenário:

- Adicionar uma linha na tabela **"Controle"**;
- Preencher a coluna **"Gestor"** com o nome do gestor;
- Preencher a coluna **"Status"** com **"Pendente"**.

---
### Resultado da Automação

Ao final da execução, a tabela **"Controle"** apresentará uma visão atualizada do status de envio de todos os gestores cadastrados.

O resultado será estruturado da seguinte forma:

|Gestor|Status|
|---|---|
|Gestor 1|Enviado|
|Gestor 2|Pendente|
|Gestor 3|Enviado|

A cada nova execução, os registros anteriores são removidos e o controle é reconstruído com base nas respostas registradas no dia atual.

Dessa forma, a automação permite realizar o **acompanhamento diário e automático dos envios**, facilitando a identificação dos gestores que já realizaram o preenchimento e daqueles que ainda estão pendentes.