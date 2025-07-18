# HealthHeaven Clinic: Análise de Dados e Dashboard Power BI

## Visão Geral do Projeto

Este projeto demonstra a construção de um pipeline completo de dados, desde a geração de dados fictícios até a criação de um dashboard interativo no Power BI. O foco é simular as operações de uma clínica médica fictícia, a "HealthHeaven", com o objetivo de fornecer insights sobre pacientes, consultas, exames, medicamentos e internações.

Este repositório foi desenvolvido como parte de um portfólio em Ciência e Análise de Dados, e demonstra habilidades nas seguintes áreas:

- **Modelagem de Dados:** Criação de esquemas de banco de dados relacionais.
- **Engenharia de Dados:** Geração de dados sintéticos e processos de ETL.
- **Análise de Dados:** Extração de insights relevantes a partir dos dados.
- **Visualização de Dados:** Construção de dashboards no Power BI.

## Arquitetura do Projeto

O projeto foi dividido nas seguintes etapas:

1. **Modelagem Conceitual**
2. **Modelagem Lógica**
3. **Geração de Dados Sintéticos**
4. **Carga e Limpeza dos Dados**
5. **Análise e Visualização no Power BI**

## Modelo Conceitual (Mermaid)

```mermaid
erDiagram
    Paciente ||--o{ Consulta : realiza
    Medico ||--o{ Consulta : atende
    Consulta ||--o{ Exame : solicita
    Consulta ||--o{ Prescricao : gera
    Exame ||--o{ Resultado_Exame : gera
    Prescricao ||--o{ Medicamento : contem
    Paciente }o--|| Plano_Saude : possui
    Medico }o--|| Departamento : pertence
    Paciente ||--o{ Internacao : passa_por
