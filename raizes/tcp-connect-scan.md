# 🔎 TCP Connect Scanning: O Primeiro Passo do Reconhecimento

**Origem do Cultivo:** Estudo arquitetural focado no desenvolvimento do [Scarlet Scanner](https://github.com/suares13/ScarletScanner) utilizando a biblioteca `socket` em Python.

---

### 🌰 1. A Semente (O Conceito Técnico)
O **TCP Connect Scanning** é o método mais fundamental e confiável de varredura de rede. Ele opera utilizando a API de rede nativa do sistema operacional (como a função `connect()` em C ou `connect_ex()` em Python) para tentar estabelecer uma conexão TCP completa com um host alvo em uma porta específica. 

Isso significa completar integralmente o *TCP 3-Way Handshake* (SYN -> SYN-ACK -> ACK). Se o sistema operacional confirma o estabelecimento da conexão e a finaliza de forma limpa, a porta está inegavelmente aberta e o serviço está escutando.

### 🗣️ 2. O Solo (A Tradução)
Imagine que um servidor é um prédio corporativo e as portas TCP são as salas de reunião. 
Fazer um TCP Connect Scan é o equivalente a caminhar pelo corredor, bater em uma porta e esperar. Se alguém abrir e disser "Pois não?" (o aperto de mão completo do TCP), você tem a confirmação de que há um serviço rodando ali. 

Diferente de *scans* mais furtivos (como o SYN Scan, onde você bate na porta e foge antes da pessoa abrir totalmente), o TCP Connect é um processo "educado" e completo, mas que tem um custo: ele deixa a sua assinatura no livro de visitas do prédio (os *logs* da aplicação).

### 📉 3. A Praga (A Visão de Risco e GRC)
Por que uma porta aberta derruba corporações? Porque uma porta exposta sem necessidade é uma falha direta de Governança. Bancos de dados (como porta 3306 - MySQL ou 5432 - PostgreSQL) expostos para a internet geralmente são frutos de erros de *deploy* ou de infraestruturas invisíveis à equipe de segurança (o famoso *Shadow IT*). 

Em termos de compliance e GRC, cada serviço exposto sem validação é uma violação de políticas de segurança (desrespeitando os controles do NIST e ISO 27001) e o primeiro passo para um vazamento de dados que resulta em multas regulatórias pesadas.

### ⚔️ 4. A Ofensiva (Perspectiva Red Team)
Na visão de um atacante, você não invade o que não conhece. O mapeamento via TCP Connect (ou equivalentes automatizados) é a fundação da fase de *Recon*. O objetivo não é ser silencioso, mas ter 100% de precisão sobre a superfície de ataque. É a partir da identificação exata dos serviços ativos que o *Red Team* seleciona qual arsenal de *exploits* será utilizado para a intrusão ou movimento lateral.

### 🛡️ 5. A Poda (Mitigação e Defesa)
Como o *Blue Team* e a Arquitetura neutralizam isso:
- **Default Deny (Negar por Padrão):** O Firewall deve descartar (*Drop*) todas as conexões de entrada por padrão, permitindo furos estritamente para portas de negócio (ex: liberar apenas a porta 443 para uma aplicação web) e utilizando VPNs/Túneis para o resto.
- **Gestão de Superfície e Auditoria:** Escaneamentos internos regulares para comparar o que está rodando com o que deveria estar rodando.
- **Monitoramento e Alertas:** Sistemas de Prevenção/Detecção (IPS/IDS) configurados para disparar alertas quando um mesmo IP de origem tenta realizar conexões com múltiplas portas diferentes em uma janela de tempo muito curta.
