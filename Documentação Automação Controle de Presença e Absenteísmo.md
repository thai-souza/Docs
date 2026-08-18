**Introdução**
Com o objetivo de otimizar tempo de consolidação das informações e melhorar a organização dos dados do controle de presença e absenteísmo da operação, foram desenvolvidas automações utilizando as ferramentas Microsoft Power Automate, Microsoft Excel e Microsoft Forms, que armazenam respostas enviadas por usuários de um formulário em uma planilha e verifica o envio diário das mesmas, atualizando o status do gestor como "*Enviado*" ou "*Pendente*" de acordo com o prazo de envio limite.

## Automação 1: Adição de Respostas do Formulário em uma Tabela

![[Documentação Automação Controle de Presença e Absenteísmo-1787051752767.webp|220x272]]
***Imagem 1:*** *Fluxo de coleta e adição de respostas do formulário à uma tabela do Excel.*

1. Quando uma nova resposta é enviada: Gatilho para a execução da automação ao ser enviada uma resposta ao formulário "Controle de Presença e Absenteísmo".
2. Obter os  detalhes da resposta: Obtém os detalhes da resposta do formulário "Controle de Presença e Absenteísmo".
3. Adicionar uma linha em uma tabela: Adiciona os parâmetros da resposta do formulário na coluna correspondente da tabela "RespostasForms".

### Configurações: Passo a Passo
No Microsoft Excel:
1. Criar uma nova pasta de trabalho em branco.
2. Adicionar na primeira linha os seguintes valores em cada coluna: Email, Data, Nome Completo, Gestor Responsável, Setor, Tipo de Expedição, Turno, Assistente, Ajudante, Auxiliar, Analista, Aprendiz, Empilhador, Paleteiro, Inspetor, Supervisor, Ausência, Atestado, Afastado, Folga e Férias;
3. Selecionar linha, formatar como tabela e "minha tabela tem cabeçalhos".
   
![[Documentação Automação Controle de Presença e Absenteísmo-1787058992779.webp]]
**Imagem 2:** *Tabela "RespostasForms".*

No Microsoft Power Automate:
	1. **Criar novo fluxo:** Modelo Fluxo da Nuvem Automatizado.
		- Inserir nome do fluxo.
		- Inserir gatilho: Quando uma nova resposta é enviada - Microsoft Forms.
	2. **No gatilho** "Quando uma nova resposta é enviada", **configurar parâmetros:**
		- ID de formulário: formulário que será utilizado para a coleta das respostas (Controle de Presença e Absenteísmo).
		  ![[Documentação Automação Controle de Presença e Absenteísmo-1787056636951.webp]]
		**Imagem 3:** *Configuração do gatilho "Quando uma resposta é enviada".*
	3. Nova ação: "Obter os detalhes da resposta". Configurar parâmetros:
		- ID de formulário: Controle de Presença e Absenteísmo.
		- ID da resposta: Inserir dados de uma ação anterior (símbolo de raio **⚡**) - ID da resposta. 
		![[Documentação Automação Controle de Presença e Absenteísmo-1787057070455.webp|500x131]]
		**Imagem 4:** *Configuração da ação "Obter detalhes da resposta".*
	4.  Nova ação: "Adicionar uma linha em uma tabela". Configurar parâmetros:
		- Localização: SharePoint Site - Sala PCP - Online/A.B.S;
		- Biblioteca de Documentos: Data Lakehouse;
		- Arquivo: /Compartilhados/Controle de Presença/Automação/RespostasForms.xlsx;
		- Tabela: RespostasForms;
			- Parâmetros Avançados: Selecionar colunas da tabela que serão passadas as respostas do formulário (Email, Data, Nome Completo, Gestor Responsável, Setor, Tipo de Expedição, Turno, Assistente, Ajudante, Auxiliar, Analista, Aprendiz, Empilhador, Paleteiro, Inspetor, Supervisor, Ausência, Atestado, Afastado, Folga e Férias), e preencher com o parâmetro da resposta do formulário correspondente.
			![[Documentação Automação Controle de Presença e Absenteísmo-1787058328858.webp|508x698]]
			**Imagem 5:** *Configuração da ação "Adicionar uma linha em uma tabela"*.

## Automação 2: Controle Diário - Status de Envio do Formulário Gestores
![[Documentação Automação Controle de Presença e Absenteísmo-1787063719387.webp]]
**Imagem 6:** *Fluxo de leitura das repostas do formulário e adição do nome do gestor com o status de envio (Enviado ou Pendente).*

1. Recorrência: gatilho para execução da automação.
2. Listar linhas presentes - Tabela Cobranca: Faz a listagem dos itens presentes da tabela "Cobranca"
3. Aplicar a cada - Valores Tabela Cobranca: A cada item atual, excluir uma linha da tabela -> Objetivo principal: apagar dados anteriores e atualizar somente com os do dia.
4. Listar linhas presentes - Tabela RespostasForms: Faz a listagem dos itens presentes da tabela "RespostasForms".
5. Filtro - Data do Dia: Filtra os valores da tabela "RespostasForms" com a data do dia.
6. Listar linhas presentes - Tabela Gestores: Faz a listagem dos itens presente da tabela "Gestores"
7. Aplicar a cada - Valores da Tabela RespostasForms: A cada gestor presente na tabela "Gestores", é verificado se ele fez o envio do formulário no dia.
8. Filtro - Gestor (Tabelas RespostasForms e Gestores): Filtra os gestor atual da lista de gestores sendo igual ao gestor que respondeu o formulário. 
9. Condição - Se o Gestor Atual Enviou: Caso o gestor atual tenha enviado ao menos uma resposta:
   - Verdadeiro: Se o resultado da condição for verdadeira, é adicionada uma linha na tabela "Cobranca" com o nome do gestor e status "Enviado".
   - Falso; Se o resultado da condição for falso, é adicionada uma linha na tabela "Cobranca" com o nome do gestor e status "Pendente".

### Configurações: Passo a Passo
No Microsoft Excel:
1. 