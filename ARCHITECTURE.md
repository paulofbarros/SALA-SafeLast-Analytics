# SALA • SafeLast Analytics - System Architecture

Este documento descreve a infraestrutura técnica e o fluxo de dados do ecossistema SALA, projetado para mitigação de riscos biomecânicos e fadiga em operações de transporte profissional na Noruega.

## 1. Stack Tecnológica (The Engine)

O SALA foi desenhado para ser leve, escalável e de baixo custo operacional:

* **Backend:** Python (Google Cloud Functions / AWS Lambda) - Processamento "Serverless" do algoritmo de risco.
* **Database:** Firebase Realtime Database - Para sincronização instantânea entre o camião e o portal da empresa.
* **Frontend:** HTML5, CSS3, JavaScript (Vanilla) - Interfaces otimizadas para baixo consumo de dados.
* **Integração de Dados:** API da MET Norway (Meteorologia) e Google Maps Platform (Telemetria).

## 2. Fluxo do Algoritmo (Data Pipeline)

O cálculo do **SALA Risk Index** segue este fluxo de 4 etapas:

1.  **Ingestão:** O sistema recebe dados de telemetria (Tempo de condução, Acelerómetro, Localização GPS).
2.  **Enriquecimento:** O algoritmo cruza a localização com APIs meteorológicas para determinar o *Surface Friction Coefficient* (Sf) em tempo real (ex: Neve vs. Asfalto seco).
3.  **Processamento:** O motor de cálculo em Python processa a fórmula biomecânica:
    `SALA_Risk = (Fatigue_Level * 1.5) + (Surface_Risk * 1.2) + (Biomechanic_Load)`
4.  **Entrega:** O resultado é enviado via WebSockets para o Dashboard do motorista e o Portal da Empresa em < 200ms.

## 3. Estratégia de Implementação (PoC)

Para garantir a precisão e segurança, o algoritmo será testado em três fases:

* **Fase A (Shadow Mode):** Testes em "back-testing" com dados históricos de frotas de Larvik para validar a taxa de acerto do algoritmo contra incidentes reais já ocorridos.
* **Fase B (Controlled Pilot):** Implementação em 5 veículos operando na rota E18 (Corredor Sul), recolhendo dados reais sem intervenção direta.
* **Fase C (Full Integration):** Lançamento do portal de BI para gestores de frota e integração com sistemas de RH para monitorização de HMS (Saúde, Meio Ambiente e Segurança).

## 4. Segurança e Compliance (GDPR)

* **Anonimização:** Os dados de risco são processados de forma a proteger a identidade do motorista, focando na segurança operacional.
* **Criptografia:** Toda a comunicação entre o veículo e a nuvem utiliza protocolos TLS 1.3.
* **Conformidade:** Alinhado com a **Arbeidsmiljøloven § 4-4** (Lei do Ambiente de Trabalho da Noruega).

---
*Autor: Paulo Fernando de Barros*
*Status: Architecture Design Phase*
