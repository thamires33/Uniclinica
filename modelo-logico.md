# Modelo Lógico – Uniclinica


Este arquivo contém a definição detalhada das tabelas e seus atributos, além dos relacionamentos entre elas.

```mermaid
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

    Pacientes ||--o{ Consultas : tem
    Medicos ||--o{ Consultas : realiza
    Consultas ||--o{ Resultados_Exames : inclui_exames
    Consultas ||--o{ Prescricoes : gera
    Exames ||--o{ Resultados_Exames : tipo_de
    Prescricoes ||--o{ Medicamentos : refere_se_a
    Pacientes }o--|| Plano_Saude : possui
    Medicos }o--|| Departamentos : pertence
    Pacientes ||--o{ Internacoes : teve_internacao
