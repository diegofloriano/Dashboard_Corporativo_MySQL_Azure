Criação de um Dashboard corporativo com integração com MySQL e Azure

# Desafio de Projeto: Integrando Dados com MySQL Azure e Transformando com Power BI

## 📝 Descrição do Desafio
[cite_start]Este projeto tem como objetivo a criação de um ecossistema de dados integrado, partindo da configuração de uma instância de banco de dados **MySQL na Azure**, passando pela manipulação via scripts SQL, até a extração e transformação refinada utilizando o **Power BI**[cite: 2, 48]. [cite_start]O foco está no processo de ETL (Extract, Transform, Load) para preparar uma base de dados empresarial para futuras análises dimensionais[cite: 33, 99].

## 🛠️ Etapas do Processo

### 1. Configuração do Ambiente na Azure
* [cite_start]**Criação da Instância:** Foi criada uma instância do MySQL no Azure para hospedar os dados do projeto[cite: 5, 12].
* [cite_start]**Segurança e Acesso:** Configuração de regras de firewall para permitir a conexão do Cloud Shell, Workbench e do Power BI Desktop ao servidor[cite: 8].

### 2. Implementação do Banco de Dados
* [cite_start]**Criação do Schema:** Utilizou-se um script para criar o schema `azure_company` e as tabelas `employee`, `departament`, `dept_locations`, `project`, `works_on` e `dependent`[cite: 63].
* [cite_start]**Ajustes de Nomenclatura:** Durante a criação, garantiu-se a consistência entre os nomes das tabelas (como o ajuste da tabela `departament`) para evitar erros nas consultas subsequentes[cite: 84].
* [cite_start]**Povoamento:** A base foi populada com um script de inserção de dados de teste[cite: 13, 47].

### 3. Integração com Power BI
* [cite_start]**Conexão:** Realizou-se a integração do Power BI com o MySQL na Azure utilizando as credenciais de acesso do servidor[cite: 10, 64].
* [cite_start]**Análise Inicial:** Verificação de cabeçalhos, tipos de dados e detecção de possíveis anomalias na base importada[cite: 15, 65].

### 4. Transformação dos Dados (ETL no Power Query)
Seguindo as diretrizes de transformação do desafio, foram executados os seguintes passos:

* **Tipagem de Dados:** Os cabeçalhos foram revisados e os tipos de dados ajustados. [cite_start]Em especial, os valores monetários foram modificados para o tipo **Número Decimal Fixo (Double preciso)**[cite: 17, 18, 71].
* **Tratamento de Nulos:**
    * Analisou-se a coluna `Super_ssn` da tabela `employee`. [cite_start]Os valores nulos foram preservados para os colaboradores que são gerentes de topo (sem supervisores)[cite: 19, 20, 73].
    * [cite_start]Verificou-se a existência de departamentos sem gerentes, realizando o preenchimento necessário com base nos dados disponíveis[cite: 21, 77].
* **Refinamento de Colunas:**
    * [cite_start]**Mescla de Nome e Sobrenome:** As colunas `Fname` e `Lname` foram mescladas para formar uma única coluna de "Nome Completo" dos colaboradores[cite: 31, 94].
    * [cite_start]**Separação de Colunas Complexas:** Colunas que continham informações compostas (como endereços ou dados mesclados) foram separadas para facilitar a filtragem[cite: 24, 80].
    * [cite_start]**Eliminação de Dados Desnecessários:** Colunas que não agregam valor ao relatório final foram removidas para otimizar o modelo[cite: 27, 36, 109].
* **Relacionamentos e Agrupamentos:**
    * [cite_start]**Junção de Colaboradores e Departamentos:** Realizou-se a mescla das tabelas `employee` e `departament` (usando `Dno` e `Dnumber`) para que cada colaborador tenha o nome do seu departamento associado[cite: 25, 84].
    * [cite_start]**Junção Colaborador-Gerente:** Criou-se uma relação para identificar os gerentes de cada colaborador, utilizando consultas SQL ou a função de mescla do Power BI[cite: 28, 91].
    * [cite_start]**Unique Combinations:** Os nomes de departamentos e localizações foram mesclados (`Dname` + `Dlocation`) para garantir que cada combinação local-departamento seja única, auxiliando o futuro modelo estrela[cite: 32, 98].
    * [cite_start]**Contagem de Equipes:** Agrupou-se os dados para identificar o número de colaboradores subordinados a cada gerente[cite: 35, 108].

## 🧠 Justificativas Teóricas

### [cite_start]Por que usar "Mesclar" e não "Atribuir/Acrescentar" no caso dos Departamentos e Localização? [cite: 34, 100]
No Power BI, a operação de **Acrescentar (Append)** é utilizada quando queremos empilhar dados de tabelas que possuem a mesma estrutura (junção vertical). 

Neste caso específico, utilizamos o **Mesclar (Merge)** pois precisávamos realizar uma **junção horizontal**. O objetivo era estender a tabela de departamentos adicionando informações de localização baseadas em uma chave comum. [cite_start]Isso cria um registro enriquecido onde o departamento e sua localização específica tornam-se um par único, algo que o simples empilhamento de linhas não conseguiria realizar[cite: 34, 100].

---
[cite_start]**Projeto desenvolvido como parte da Formação Power BI Analyst.** [cite: 38]
