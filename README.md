# Documento de Especificação: Produto Mínimo Viável (MVP)

---

## 1. Visão Geral do Produto

**Nome:** JuntaAí <br>
**Modelo de Negócio:** B2C (Business-to-Consumer) / Utilitário Web

### 1.1. O Problema
A organização de eventos sociais informais (confraternizações, reuniões de equipe, viagens) sofre com a fragmentação e perda de informações. Plataformas de mensageria instantânea, embora eficientes para comunicação síncrona, são ineficazes para a retenção de dados essenciais. Isso resulta em comunicação redundante, onde dados críticos (endereço, dados bancários para rateio, listas de suprimentos) precisam ser repetidos constantemente aos participantes.

### 1.2. O Público-Alvo
Indivíduos responsáveis pela organização e gestão de eventos sociais e corporativos informais, bem como grupos de pessoas que necessitam de um ponto centralizado de informações para engajamentos coletivos.

### 1.3. A Proposta de Valor
Fornecer um "hub" temporário e centralizado para eventos, atuando como a única fonte de verdade para os participantes. A solução visa eliminar a fricção na comunicação, consolidando dados logísticos e financeiros em uma interface de acesso universal e instantâneo.

---

## 2. Escopo Funcional (Core Features)

As funcionalidades abaixo representam o conjunto mínimo necessário para validar a hipótese do produto, focando na jornada principal do usuário.

| Funcionalidade | Descrição Técnica e de Negócio |
| :--- | :--- |
| **Criação de Eventos (Frictionless)** | Interface de formulário para input de dados do evento (Título, Data/Hora, Coordenadas geográficas/Link Maps, Chave PIX e Instruções). Não requer autenticação inicial para reduzir a barreira de adoção. |
| **Geração de URL Única** | Processamento no backend para registrar a entidade no banco de dados e gerar um identificador único (URI amigável), permitindo o compartilhamento facilitado. |
| **Landing Page do Evento** | Página de visualização *Mobile-First* otimizada para carregamento rápido. Apresenta os dados consolidados com ações rápidas integradas ao sistema operacional do usuário (ex: *Copy-to-Clipboard* para o PIX, *Deep Linking* para aplicativos de navegação GPS). |
| **Gestão de Suprimentos (Opcional)** | Módulo colaborativo simples onde convidados podem registrar itens que irão fornecer, atualizando o estado da página em tempo real ou no recarregamento para todos os visualizadores. |

---

## 3. Requisitos Não Funcionais (Atributos de Qualidade)

* **Segurança e Sanitização (via Flask-WTF):** O sistema deve garantir integridade e segurança na entrada de dados. A implementação exige tokens de mitigação contra ataques CSRF (Cross-Site Request Forgery) em todas as submissões. Além disso, as entradas de texto livre devem passar por processos estritos de validação e *escaping* (via Jinja2) para prevenir vulnerabilidades de injeção, como XSS (Cross-Site Scripting).
* **Acessibilidade e Usabilidade:** A interface do usuário (UI) consumidora deve dispensar a criação de credenciais de acesso (Logins/Senhas). A arquitetura visual deve priorizar resoluções móveis (*Mobile-First*), garantindo legibilidade e interatividade adequadas em telas reduzidas.
* **Ciclo de Vida de Dados (Efemeridade):** Implementação de uma rotina de *garbage collection* ou exclusão lógica/física programada no banco de dados para remover registros de eventos que ultrapassarem 7 dias da data de execução estipulada, otimizando o uso de armazenamento.
* **Manutenibilidade e Qualidade de Código:** O desenvolvimento deve ser estritamente orientado pelas práticas de Clean Code e pelos princípios SOLID, assegurando que o código seja escalável para iterações futuras. O fluxo de desenvolvimento também deve prever a criação de testes estruturais e funcionais, garantindo a resiliência das regras de negócio, a validação correta dos formulários e a estabilidade das rotas antes de cada implantação.

---

## 4. Arquitetura e Stack Tecnológico

A escolha do *stack* prioriza o desenvolvimento ágil por um desenvolvedor solo, aliando ferramentas consolidadas de mercado com facilidade de implantação (*deployment*).

| Camada | Tecnologia | Justificativa |
| :--- | :--- | :--- |
| **Backend & Roteamento** | Python + Flask | Framework *micro* que oferece flexibilidade e rapidez na construção de APIs e rotas web sem *overhead* arquitetural. |
| **Validação & Segurança** | Flask-WTF (WTForms) | Abstração robusta para tratamento de formulários, garantindo validação de tipagem, limites de caracteres e proteção nativa de segurança. |
| **Frontend (Apresentação)** | Jinja2 + Tailwind CSS | Renderização *Server-Side* via templates Jinja acoplados ao Flask, estilizados com classes utilitárias do Tailwind para acelerar a construção de interfaces responsivas. |
| **Persistência de Dados** | SQLite + SQLAlchemy | Abordagem orientada a objetos (ORM) para mapeamento de banco de dados. O SQLite elimina a necessidade de infraestrutura de banco de dados no MVP, e o SQLAlchemy garante portabilidade. |
| **Infraestrutura (Deploy)** | Render ou PythonAnywhere | Plataformas de hospedagem baseadas em *Platform as a Service* (PaaS) que suportam ecossistemas Python nativamente com planos iniciais de custo zero. |
