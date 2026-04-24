Como melhoria arquitetural aplicada no ciclo evolutivo do LogiTrack, foi implementado um padrão de resiliência e desacoplamento entre containers críticos, visando reduzir falhas em cascata nas integrações externas.
A principal evolução foi a introdução de um modelo híbrido de comunicação assíncrona + proteção de dependências externas, baseado em três decisões arquiteturais:


Implementação de uma fila de eventos (Message Queue) para desacoplar o fluxo de solicitação de rotas do processamento de IA.


Aplicação do padrão Circuit Breaker nas chamadas às APIs externas de mapas e tráfego, evitando falhas em cascata quando serviços externos estão indisponíveis.


Introdução de cache de respostas de rotas frequentes, reduzindo latência e custo computacional em requisições repetidas.


Essa evolução está documentada na nova ADR:

ADR-02 — Introdução de Arquitetura Assíncrona e Resiliência nas Integrações Externas

Impactos diretos nos RNFs priorizados:


Performance: redução de latência percebida no cálculo de rotas.


Confiabilidade: isolamento de falhas externas.


Escalabilidade: suporte a picos de requisições via fila de eventos.


Integração: maior tolerância a indisponibilidade de APIs terceiras.


Do ponto de vista arquitetural, a mudança aproxima o sistema de um estilo event-driven híbrido com camadas desacopladas, mantendo compatibilidade com o modelo atual, mas preparando o sistema para evolução futura sem ruptura estrutural.