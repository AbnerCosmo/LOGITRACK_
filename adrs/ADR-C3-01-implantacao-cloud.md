# ADR-C3-01 — Estratégia de Implantação Cloud

## Projeto: LogiTrack

Status: Proposto

---

## Contexto

O LogiTrack é uma plataforma de otimização logística baseada em dados geoespaciais e inteligência artificial para cálculo inteligente de rotas e previsão de ETA em tempo real.

O sistema possui os seguintes requisitos arquiteturais prioritários:

- alta performance no cálculo de rotas;
- integração estável com APIs externas de mapas e tráfego;
- escalabilidade para suportar múltiplas requisições simultâneas;
- confiabilidade no processamento de eventos logísticos;
- baixa latência na atualização de entregas.

Além disso, o projeto apresenta as seguintes restrições:

- equipe pequena de desenvolvimento;
- necessidade de reduzir esforço operacional;
- budget limitado para infraestrutura;
- dependência de APIs externas;
- necessidade de evolução gradual sem migração prematura para microsserviços completos.

A arquitetura atual segue o modelo:
- Arquitetura Hexagonal (Ports & Adapters)
- Monólito Modular Evolutivo
- Event-Driven híbrido com mensageria
- Cache distribuído
- Integration Gateway

---

## Decisão

Adotar uma estratégia híbrida baseada em:

- PaaS gerenciado para hospedagem da aplicação backend;
- PostgreSQL gerenciado em nuvem;
- Redis para cache distribuído;
- RabbitMQ para comunicação assíncrona;
- Deploy containerizado via Docker;
- Pipeline CI/CD automatizado.

Tecnologias escolhidas:

- Railway / Render para aplicação principal;
- Supabase PostgreSQL para persistência;
- Redis Cloud para cache;
- CloudAMQP para mensageria;
- Docker para empacotamento da aplicação.

---

## Trade-off Aceito

Ganhamos:

- velocidade de implantação;
- redução de esforço operacional;
- maior disponibilidade;
- escalabilidade inicial simplificada;
- integração facilitada com serviços gerenciados;
- menor tempo de setup da infraestrutura;
- desacoplamento assíncrono entre módulos críticos.

Abrimos mão de:

- controle total da infraestrutura;
- customizações avançadas de rede;
- tuning granular de containers;
- orquestração avançada via Kubernetes;
- controle completo de observabilidade nativa.

Esse trade-off é aceitável porque o contexto atual do LogiTrack prioriza:

- evolução rápida do produto;
- estabilidade operacional;
- redução de complexidade;
- foco no domínio logístico e IA;
- preparação arquitetural gradual para futura escalabilidade.

A adoção imediata de Kubernetes aumentaria significativamente:
- custo operacional;
- complexidade arquitetural;
- curva de aprendizado;
- esforço de manutenção.

Para o estágio atual do projeto, o ganho não justificaria o investimento.

---

## Consequências

### Impactos Arquiteturais

A arquitetura passa a operar com modelo híbrido síncrono + assíncrono:

- requisições críticas utilizam APIs HTTP;
- processamento pesado de IA é desacoplado via fila;
- cache reduz chamadas repetidas às APIs externas;
- Circuit Breaker protege integrações críticas;
- Integration Gateway centraliza comunicação externa.

Essa decisão melhora diretamente os RNFs priorizados:

- Performance → menor latência via cache;
- Escalabilidade → processamento desacoplado via mensageria;
- Confiabilidade → isolamento de falhas externas;
- Integração → tolerância a indisponibilidade de APIs terceiras.

---

## Próximos 30 Dias

- Containerizar todos os módulos principais;
- Configurar CI/CD automatizado;
- Implantar Redis e RabbitMQ gerenciados;
- Configurar observabilidade e logs centralizados;
- Implementar monitoramento de filas;
- Validar estratégia de fallback das APIs externas;
- Criar ambientes separados de staging e produção;
- Executar testes de carga nas rotas de otimização.

---

## Decisões Futuras Forçadas

Essa escolha arquitetural irá forçar decisões futuras relacionadas a:

- migração parcial para microsserviços;
- adoção futura de Kubernetes;
- implementação de observabilidade distribuída;
- estratégia de service discovery;
- autoscaling avançado;
- governança de eventos e contratos assíncronos;
- divisão do módulo de IA em serviço independente.

Caso o volume de entregas cresça significativamente, será necessário evoluir para uma arquitetura mais distribuída e com orquestração avançada.