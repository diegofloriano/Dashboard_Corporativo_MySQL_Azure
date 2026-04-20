Criação de um Dashboard corporativo com integração com MySQL e Azure
# Desafio de Projeto: Processamento de Dados com MySQL Azure e Power BI

## 📝 Descrição do Projeto
Este projeto faz parte do desafio de "Integração de Dados com MySQL Azure e Transformação com Power BI". O objetivo foi criar uma infraestrutura de dados na nuvem, realizar a migração de uma base relacional empresarial e aplicar técnicas de ETL (Extract, Transform, Load) para preparar os dados para relatórios e modelagem futura.

## 🛠️ Passo a Passo do Desenvolvimento

### 1. Configuração do Banco de Dados na Azure
- **Criação da Instância:** Foi criada uma instância do "Azure Database for MySQL" para hospedar o banco de dados.
- **Segurança:** Configuração das Regras de Firewall para permitir o acesso via Cloud Shell, Workbench local e Power BI.
- **Conexão:** Acesso ao banco realizado via Workbench para execução dos scripts de definição e manipulação.

### 2. Criação do Schema e Povoamento
- **Script de Criação:** Implementação das tabelas `employee`, `departament`, `dept_locations`, `project`, `works_on` e `dependent`.
- **Integridade Referencial:** Configuração de chaves primárias e estrangeiras. Houve uma atenção especial à tabela `departament` (nomeada assim conforme o script original) e suas restrições de gerente.
- **Carga de Dados:** Inserção dos registros de teste para popular o ambiente e permitir as transformações.

### 💡 Solução de Problemas (Troubleshooting) e Boas Práticas

**1. Dependência Circular no Povoamento do Banco:**
Durante a inserção dos dados (`INSERT`), o MySQL bloqueou a operação devido a restrições de chaves estrangeiras (ex: um empregado precisava de um gerente que ainda não havia sido inserido, e o departamento precisava do empregado). 
* **Solução aplicada:** O script de inserção foi ajustado encapsulando os comandos entre `SET FOREIGN_KEY_CHECKS = 0;` e `SET FOREIGN_KEY_CHECKS = 1;`. Isso permitiu o povoamento inicial sem ferir a integridade do banco, reativando as travas logo em seguida.

**2. Correção de Nomenclatura via ETL:**
O banco de dados original possuía um erro de digitação na tabela de departamentos (`departament`). Seguindo as boas práticas de não alterar a estrutura da fonte de dados (banco de produção), a correção para o inglês correto (`department`) ou tradução para o português foi realizada durante a etapa de transformação no **Power Query**, garantindo um modelo final limpo para o relatório.

### 3. Integração e Transformação com Power BI
Após conectar o Power BI à instância da Azure, foram aplicadas as seguintes transformações no Power Query:

- **Cabeçalhos e Tipos:** Ajuste manual de cabeçalhos e verificação dos tipos de dados. A coluna `Salary` foi convertida para **Número Decimal Fixo (Double preciso)** para evitar perdas em cálculos.
- **Tratamento de Nulos:**
    - Na coluna `Super_ssn` da tabela `employee`, os valores nulos foram identificados como o topo da hierarquia (gerentes gerais) e mantidos conforme análise.
    - Verificação de departamentos sem gerentes e preenchimento das lacunas conforme a regra de negócio do desafio.
- **Análise de Horas:** Verificação e limpeza dos dados de horas na tabela `works_on`.
- **Separação de Colunas:** Colunas com dados complexos foram divididas para simplificar a estrutura atômica dos dados.
- **Criação de Nomes Únicos:** Mescla das colunas `Fname` e `Lname` para criar a coluna `Nome Completo`.
- **Mesclas de Tabelas (Joins):**
    - **Employee + Departament:** Junção (Left Join) para trazer o nome do departamento para a linha de cada colaborador, eliminando colunas redundantes.
    - **Colaborador + Gerente:** Realização de um self-join ou consulta para associar o nome do gerente a cada funcionário.
    - **Departamento + Localização:** Mescla dos nomes de departamentos e locais para criar combinações únicas, essencial para a construção de um futuro modelo estrela.
- **Agrupamento:** Agrupamento de dados para contabilizar a quantidade de colaboradores por gerente.

## 🧠 Justificativas Teóricas do Desafio

### Por que usar "Mesclar" e não "Atribuir/Acrescentar" no caso de Departamento e Local?
Durante o processo, surge a dúvida sobre qual ferramenta de combinação utilizar. Neste cenário, utilizamos o **Mesclar (Merge)** e não o **Acrescentar/Atribuir (Append)** pelos seguintes motivos:

1. **Direção da Combinação:** O "Mesclar" realiza uma junção horizontal (similar ao JOIN do SQL). Como queríamos adicionar a informação de *Localização* (que estava em outra tabela) à nossa tabela de *Departamentos*, precisávamos unir as colunas baseadas em uma chave comum (`Dnumber`).
2. **Estrutura dos Dados:** O "Acrescentar" serve para empilhar linhas (junção vertical) de tabelas que possuem a mesma estrutura. Como as tabelas de Departamento e Localização possuem informações e colunas distintas, o "Acrescentar" apenas criaria uma tabela longa com muitos valores nulos, enquanto o "Mesclar" cria uma tabela rica e consolidada, facilitando a criação de dimensões únicas para o modelo de BI.

---
**Projeto desenvolvido para a Formação Power BI Analyst.**
