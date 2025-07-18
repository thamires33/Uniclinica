# HealthHeaven Clinic: Análise de Dados e Dashboard Power BI

## Visão Geral do Projeto

Este projeto demonstra a capacidade de construir um pipeline completo de dados, desde a geração de dados fictícios até a criação de um dashboard interativo no Power BI. O foco é simular as operações de uma clínica médica fictícia, a "HealthHeaven", para fornecer insights acionáveis sobre pacientes, consultas, exames, medicamentos e internações.

Desenvolvido como parte de um portfólio em Ciência e Análise de Dados, este repositório mostra habilidades em:
* **Modelagem de Dados:** Criação de esquemas de banco de dados robustos e relacionamentos.
* **Engenharia de Dados:** Geração de dados sintéticos e processos ETL básicos.
* **Análise de Dados:** Extração de insights significativos a partir dos dados.
* **Visualização de Dados:** Construção de dashboards interativos e intuitivos no Power BI.

## Funcionalidades do Dashboard Power BI

O dashboard interativo da HealthHeaven Clinic permite explorar diversas métricas e tendências, incluindo:

* **Visão Geral da Clínica:** Resumo de consultas, pacientes, receita e principais KPIs.
* **Análise de Pacientes:** Perfil demográfico, distribuição por plano de saúde, pacientes novos vs. recorrentes.
* **Performance de Médicos e Departamentos:** Volume de consultas por especialidade/médico, tempo médio de atendimento.
* **Gestão de Agendamentos:** Taxas de comparecimento, cancelamento e tipos de consultas mais frequentes.
* **Monitoramento de Exames:** Tipos de exames mais solicitados, volume de resultados e (se dados gerados permitirem) uma visão de resultados anormais.
* **Consumo de Medicamentos:** Medicamentos mais prescritos e seus custos associados.
* **Visão Financeira:** Receita gerada por consultas e exames, custos de medicamentos e internações.
* **Análise de Internações:** Duração média, motivos e custos de internação.

## Arquitetura do Projeto

A solução foi desenvolvida seguindo as seguintes etapas e tecnologias:

### 1. Modelagem de Dados

A fase de modelagem foi crucial para definir a estrutura e os relacionamentos dos dados da clínica.

#### 1.1. Diagrama de Modelo Conceitual

Este diagrama representa a visão de alto nível das entidades de negócio e seus relacionamentos.

```mermaid
erDiagram
    Paciente ||--o{ Consulta : "realiza"
    Medico ||--o{ Consulta : "atende"
    Consulta ||--o{ Exame : "solicita"
    Consulta ||--o{ Prescricao : "gera"
    Exame ||--o{ Resultado_Exame : "gera"
    Prescricao ||--o{ Medicamento : "contem"
    Paciente ||--o| Plano_Saude : "possui"
    Medico ||--o| Departamento : "pertence_a"
    Paciente ||--o{ Internacao : "passa_por"

    erDiagram
    Pacientes {
        UUID id_paciente PK "UUID único do paciente"
        string nome
        string sobrenome
        date data_nascimento
        string genero "Masculino, Feminino, Outro"
        string endereco
        string telefone
        UUID id_plano_saude FK "Chave estrangeira para Plano_Saude"
    }

    Medicos {
        UUID id_medico PK "UUID único do médico"
        string nome
        string sobrenome
        string especialidade
        string crm "Registro médico"
        UUID id_departamento FK "Chave estrangeira para Departamentos"
    }

    Consultas {
        UUID id_consulta PK "UUID único da consulta"
        UUID id_paciente FK "Chave estrangeira para Pacientes"
        UUID id_medico FK "Chave estrangeira para Medicos"
        datetime data_hora_consulta
        string tipo_consulta "Rotina, Emergência, Retorno"
        string status_consulta "Agendada, Realizada, Cancelada"
        decimal valor_consulta
    }

    Exames {
        UUID id_exame PK "UUID único do tipo de exame"
        string nome_exame "Nome do exame (Ex: Hemograma)"
        string tipo_exame "Laboratorial, Imagem, Cardiológico"
        decimal custo_exame
        string unidade_medida_padrao "mg/dL, %"
    }

    Resultados_Exames {
        UUID id_resultado_exame PK "UUID único do resultado"
        UUID id_consulta FK "Chave estrangeira para Consultas (pode ser nulo)"
        UUID id_paciente FK "Chave estrangeira para Pacientes"
        UUID id_exame FK "Chave estrangeira para Exames (tipo)"
        datetime data_hora_coleta
        datetime data_hora_resultado
        string valor_resultado "Valor numérico ou textual"
        string unidade_medida_resultado
        string status_resultado "Normal, Anormal, Aguardando"
        string comentarios_medicos
    }

    Prescricoes {
        UUID id_prescricao PK "UUID único da prescrição"
        UUID id_consulta FK "Chave estrangeira para Consultas"
        UUID id_medicamento FK "Chave estrangeira para Medicamentos"
        int quantidade
        string dosagem
        string instrucoes_uso
    }

    Medicamentos {
        UUID id_medicamento PK "UUID único do medicamento"
        string nome_medicamento
        string fabricante
        string tipo_medicamento "Antibiótico, Analgésico"
        decimal preco_unitario
    }

    Plano_Saude {
        UUID id_plano_saude PK "UUID único do plano"
        string nome_plano
        string tipo_cobertura "Básica, Premium"
    }

    Departamentos {
        UUID id_departamento PK "UUID único do departamento"
        string nome_departamento
    }

    Internacoes {
        UUID id_internacao PK "UUID único da internação"
        UUID id_paciente FK "Chave estrangeira para Pacientes"
        datetime data_internacao
        datetime data_alta
        string motivo_internacao
        decimal custo_internacao
    }


    Pacientes ||--o{ Consultas : "tem"
    Medicos ||--o{ Consultas : "realiza"
    Consultas ||--o{ Resultados_Exames : "inclui_exames"
    Consultas ||--o{ Prescricoes : "gera"
    Exames ||--o{ Resultados_Exames : "tipo_de"
    Prescricoes ||--o{ Medicamentos : "refere_se_a"
    Pacientes ||--o| Plano_Saude : "possui"
    Medicos ||--o| Departamentos : "pertence_a"
    Pacientes ||--o{ Internacoes : "teve_internacao"