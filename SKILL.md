---
name: meta-ads-manager
description: Gestão de campanhas PAGAS no Meta Ads (Facebook e Instagram) — estrutura, objetivos, públicos e lookalike, Advantage+, criativos (feed/stories/reels), Meta Pixel e Conversions API (CAPI), budget, identidade da Página/Instagram e o fluxo real de criação no Gerenciador de Anúncios. Use SEMPRE que o usuário mencionar Meta Ads, Facebook Ads, Instagram Ads, Meta Pixel, CAPI, Advantage+, público semelhante/lookalike, retargeting no Meta, subir/otimizar/auditar campanha de tráfego ou conversão, criativo de anúncio, impulsionar post (boost) no Facebook/Instagram, anúncio Click-to-WhatsApp, ou problemas de identidade do Instagram no anúncio (ex.: erro 1815199, anúncio saindo com o perfil errado) — mesmo sem dizer a palavra skill. NÃO use para Google Ads (use google-ads-manager-2026), TikTok Ads, LinkedIn Ads, conteúdo ORGÂNICO de redes sociais SEM impulsionamento, copywriting/headlines isoladas (use marketing-expert), SEO, GA4 ou landing pages.
metadata:
  version: 2026.1
  sources:
    - kostja94/marketing-skills meta-ads v1.1.1 (base conceitual)
    - Operação real MegaAPI — campanha Tráfego do zero (fluxo + armadilhas)
    - Operação real MegaAPI — Click-to-WhatsApp Fase 2 + fix CAPI server-side + criativo animado (2026-06)
---

# Meta Ads Manager (2026)

Playbook para montar, otimizar e auditar campanhas no **Gerenciador de Anúncios da Meta** (Facebook + Instagram). A Meta brilha em **geração de demanda** e produtos visuais. Combina o framework conceitual (estrutura, públicos, Advantage+) com o **fluxo prático de criação** e as **armadilhas reais** que só aparecem fazendo de verdade.

**Ao usar**: na primeira vez, abra com 1–2 frases do que cobre; depois vá direto ao output. Para ações que gastam dinheiro ou publicam (ativar campanha, publicar anúncio, mudar budget), **confirme antes** — monte tudo e pare antes do "Publicar" para o usuário revisar.

---

## 1. Estrutura da conta

**Hierarquia (3 níveis):** Campanha → Conjunto de anúncios (ad set) → Anúncio (ad).

**Princípio:** **um objetivo por campanha**. Várias campanhas para o mesmo objetivo dividem budget e dados, atrasando a fase de aprendizado do algoritmo. Consolide por objetivo; estrutura limpa + decisões guiadas por dados.

```
Conta
├── Campanha: Prospecção
│   ├── Conjunto: Lookalike 1%
│   └── Conjunto: Amplo (Advantage+)
├── Campanha: Retargeting
└── Campanha: Teste
```

**Nomenclatura:** `META_[Objetivo]_[Público]_[Oferta]_[Data]`
(ex.: `META_Conv_Lookalike-Clientes_TesteGratis_2026Q2`). No nível do anúncio, evite nomes genéricos tipo "Cópia" — renomeie para algo legível (não afeta veiculação; é interno).

---

## 2. Objetivos de campanha

| Objetivo | Quando usar |
|----------|-------------|
| **Reconhecimento** | Alcance, lembrança de marca |
| **Tráfego** | Cliques para o site (warm-up do pixel; **não** otimiza cadastro) |
| **Conversões/Leads** | Cadastros, vendas, instalações |
| **Engajamento** | Views de vídeo, engajamento no post |

> **Fase 1 → Fase 2:** comece em **Tráfego** para esquentar o pixel e juntar sinal; depois de acumular eventos (idealmente ~50 conv/semana) **gradue para Conversões/Leads**, que é o que traz cliente barato de verdade. Registre essa decisão; Tráfego sozinho não otimiza cadastro.

---

## 3. Advantage+ & automação

| Recurso | Uso |
|---------|-----|
| **Advantage+ Shopping** | E-commerce; descoberta automática de público |
| **Anúncios dinâmicos** | Catálogo de produto; criativo auto-gerado |
| **Posicionamentos automáticos** | Deixa a Meta otimizar Feed/Stories/Reels |
| **Público Advantage+** | Segmentação ampla; o algoritmo acha quem converte |

Forneça **assets diversos** (vários formatos/ângulos) — o algoritmo performa melhor com variedade. ⚠️ Posicionamentos Advantage+ **incluem Instagram**, então exigem uma **identidade de Instagram válida** (ver §6).

---

## 4. Públicos (targeting)

| Tipo | Melhor para |
|------|-------------|
| **Lookalikes** | Baseie nos **melhores** clientes (por LTV), não em todos |
| **Interesse/comportamento** | Amplo; deixe o algoritmo otimizar |
| **Advantage+** | Automatizado; menos controle manual |
| **Retargeting** | Visitantes do site, engajadores, públicos personalizados |

**Exclusões:** clientes atuais; quem converteu recentemente (7–14 dias).

---

## 5. Criativos

**Specs e princípios:**
- **Imagem:** produto claro; antes/depois; rostos humanos; **texto < 20%** da arte.
- **Vídeo (15–30s):** hook 0–3s · problema 3–8s · solução 8–20s · CTA 20–30s.
- **Posicionamentos:** Feed (FB/IG) **1:1** (1080×1080); Stories/Reels **vertical 9:16** (ou 2:3 1080×1350). Use **personalização por posicionamento** para a arte certa em cada lugar.
- **Volume:** 3–5 variações de anúncio por conjunto para testar.

**Pipeline de geração (real, MegaAPI):**
1. Gerar imagens com **Replicate `openai/gpt-image-2`** (qualidade alta) nos formatos feed 1:1 + stories 2:3.
2. Compor overlay (logo + headline + CTA) com **Pillow**; logo branco a partir de SVG via **@resvg/resvg-js** (no Windows, evita dependência nativa do cairo).
3. Subtexto legível: faixa escura (scrim) atrás do texto sobre imagens "ocupadas".
4. No editor, **desligar todos os aprimoramentos de IA** (Aprimorar texto da mídia, Adicionar música, sobreposições, retoques) para manter controle de marca — a IA reescreve/altera sua arte e copy.

> ⚠️ **Upload de arquivo local não é automatizável** pelas ferramentas de browser atuais — o upload dos criativos é feito **pelo usuário** (botão "+ Carregar" na aba Mídia / "Alterar seleções"). Forneça os caminhos e os formatos. A tool `file_upload` do browser **rejeita paths do host** ("no longer accepts host filesystem paths") — confirmado, é sempre manual.

**Animar criativo estático (imagem → vídeo) sem deformar texto (real, MegaAPI):**
i2v (image-to-video) **deforma texto/logo**. Solução = animar a **base limpa** (sem texto) e sobrepor o texto estático depois:
1. **Replicate `wan-video/wan-2.2-i2v-fast`** (o mais barato/rápido, ~centavos/5s) anima a base `_creatives_*` (sem overlay). Input: `image`(URL via Files API), `prompt` conservador ("subtle ambient motion… stays steady and readable, no text"), `num_frames`=81, `resolution`=720p, `interpolate_output`=true (→30fps). Mantém o aspect da imagem (1:1→960², 2:3→retrato).
2. **Pillow** gera a camada de texto **transparente** (RGBA: scrims + logo + headline + CTA) no tamanho da base.
3. **ffmpeg** compõe: `scale=W:H:flags=lanczos[bg];[bg][1:v]overlay=0:0,format=yuv420p` → libx264 crf18. Resultado: fundo animado + texto 100% nítido.
- Token Replicate antigo pode expirar (401) — validar com `GET /v1/account` antes. Usar stdlib `urllib` (sem libs externas) se o env Python estiver quebrado.
- Vídeo resolve o alerta Meta "posicionamento exige vídeo" (ex.: Audience Network vídeo-incentivo) que a imagem não cobre.

---

## 6. ⚠️ Identidade Facebook + Instagram (a armadilha #1)

Esta é a parte que **trava mais** e quase não aparece em guias. Anúncio precisa de uma **Página do Facebook** e, se os posicionamentos incluem Instagram (Advantage+ quase sempre inclui), de uma **identidade de Instagram**.

**Erro clássico `#1815199`** — "A conta de anúncios não tem acesso à conta do Instagram": acontece quando a Página **não tem Instagram vinculado** e o único IG do business é um perfil pessoal sem acesso da conta de anúncios.

**Diagnóstico (read-only) antes de mexer:**
1. Business Settings → **Contas do Instagram**: quais IGs existem? A conta de anúncios tem acesso (aba "Ativos conectados")?
2. **Página → Configurações → Contas vinculadas / Instagram**: a Página tem IG vinculado?

**Correções (o usuário executa — é permissão/config; você guia):**
- **Vincular IG à Página** via Meta Business Suite → link "**Conectar o Instagram**" no topo da Página (pede login do IG — você NÃO digita credencial).
- Dar acesso da conta de anúncios ao IG (Business Settings → Contas do Instagram → "Conectar ativos").
- Decisão de marca: usar IG da marca (@marca) > perfil pessoal (off-brand).

**O BUG QUE VOCÊ PRECISA SABER:** mesmo com a identidade certa criada, o **combobox "Perfil do Instagram" do editor não troca a identidade** — clique nativo, teclado, JS e o clique manual do usuário **todos revertem**. O anúncio fica preso na identidade antiga.
**✅ Solução comprovada: DUPLICAR o anúncio** (na tabela do Gerenciador → "Duplicar configuração original"). O duplicado **nasce já com a identidade resolvida pela Página** e copia todo o conteúdo (mídia, textos, UTMs). Depois exclua o anúncio travado.

> Editor "standalone" (edição rápida) **não** duplica/exclui — use a **tabela** do Gerenciador (manage/ads) para Duplicar e Excluir.

---

## 7. Rastreamento (Pixel / CAPI / eventos)

- **Meta Pixel + Conversions API (CAPI):** rode os dois (navegador + servidor) com **deduplicação** (mesmo `event_id`). CAPI server-side recupera conversões perdidas no pós-cookie. Pode rodar via **GTM server-side**.
- **⚠️ Test Event Code (armadilha que custa tempo):** na tag CAPI (GTM server) o campo **"Test Event Code" deve ficar VAZIO em produção**. Se ficar um código fixo (ex.: `TEST9853`), a CAPI manda **TODOS os eventos como "teste"** — eles caem na aba "Eventos de teste" e **nunca** contam como produção, fazendo a CAPI parecer morta. Caso real: ficou preenchido por 3 anos. Como detectar: no Events Manager, o evento mostra integração **só "Navegador"** (nunca "Navegador e servidor"). Correção: limpar o campo e republicar o container.
- **Pixel dedicado:** evite pixel órfão (fora do seu Business Manager). Crie um pixel próprio e migre client + CAPI para ele. ⚠️ Migre **as duas pontas**: variável do Pixel no GTM **web** E a tag CAPI no GTM **server** — esquecer o server deixa a CAPI mandando pro pixel velho.
- **Validar tracking:** Events Manager → o evento bom mostra **"Navegador e servidor"** com dedup. Só "Navegador" = CAPI não está alimentando aquele pixel (ver os 2 itens acima).
- **⚠️ Token CAPI inválido pro pixel (causa #1 de "só Navegador" depois de trocar pixel):** ao migrar pro pixel novo, **regenere o token de acesso no pixel novo** e atualize a variável do token no GTM server. Token velho (do pixel antigo) → Meta **rejeita em silêncio** (o Diagnóstico **não acusa** porque nada server-side completa). Caso real: trocaram o pixel mas o `{{Facebook Token}}` continuou inválido → CAPI morta por dias. Fix: Events Manager → dataset → Configurações → **"API de Conversões" → "Configurar sem a Dataset Quality API" → "Gerar token de acesso"** → colar na variável do token no server container → **publicar nova versão**.

**Playbook de diagnóstico CAPI server-side (GTM Preview / Tag Assistant):**
A cadeia é: `form submit → evento dataLayer (ex. sign_up) → tag web "GA4 event" (transport_url=server) → server container recebe → trigger → Conversions API Tag → Meta`. Para achar onde quebra:
1. **GTM web** container → botão **"Visualizar"** → Tag Assistant → conectar à URL onde o evento dispara (ex. o register). Gerar 1 conversão real.
2. No Tag Assistant, abrir o evento e a tag CAPI-web: confirmar que **disparou/concluiu**, ver em **"Valores"** (não "Nomes") o `transport_url` (= URL do server) e o `event_id`. "Hits enviados → G-XXXX" = mandou pro server com aquele measurement.
3. **GTM server** container → "Visualizar" também → repetir 1 conversão → ver se a Conversions API Tag dispara e a **resposta do Meta** (erro de token aparece aqui).
4. Checklist server: variável do **Pixel** = pixel certo? variável do **Token** corresponde a esse pixel? **Test Event Code** vazio? **trigger** aciona (RegEx `.+` aciona qualquer evento)?
- ⚠️ **GTM server Preview é frágil:** a aba `…/gtm/debug` fecha sozinha e o **link direto expira** — sempre religar pelo botão **"Visualizar"** do container, não pela URL. Renderer do GTM trava muito; usar `get_page_text` quando o screenshot falhar.
- **GTM tem 2 containers:** **web** (GTM-xxxx, tags Pixel/GA4/CAPI-web que mandam pro server) e **server** (Conversions API Tag → Meta). Confirme o login Google certo (authuser) — o container pode estar em outra conta Google que não a logada.
- **Verificação de domínio:** verifique o domínio no Business Manager (meta-tag ou DNS TXT). ⚠️ Cuidado com a **zona DNS certa** se houver múltiplos domínios/subdomínios. **Não confunda** com a **lista de permissão (allowlist) do pixel** (Events Manager → dataset → Permissões de tráfego): são controles diferentes de nome parecido. O alerta "Confirme os domínios que pertencem a você" no Diagnóstico é a allowlist (confirmar que o domínio pode mandar eventos), **não** a verificação de domínio — por isso continua pendente mesmo com o domínio já verificado.
- **Correspondência avançada automática:** ative (Configurações → "Correspondência automática de site") — usa email/telefone com hash dos formulários para casar mais conversões com contas Meta (melhora atribuição e remarketing).
- **AEM (Aggregated Event Measurement):** priorize até 8 eventos por domínio (Lead no topo) — necessário pós-iOS14 para medir usuários de iPhone.
- **UTMs:** no editor novo, os parâmetros de URL ficam em **"Rastreamento" → Parâmetros de URL** (não anexe na URL de destino). Ex.: `utm_source=meta&utm_medium=cpc&utm_campaign=trafego_fase1`.

---

## 8. Budget, lance e aprendizado

- **Fase de aprendizado:** ~**50 conversões por conjunto por semana** para sair; **evite mudanças frequentes** durante o aprendizado (resseta).
- **CBO vs ABO:** Campaign Budget Optimization consolida o gasto — use ao **escalar**; ABO dá controle por conjunto no início.
- **Frequência:** mantenha **< 3** para evitar fadiga.
- **Refresh de criativo:** fadiga criativa é a principal alavanca — planeje teste contínuo e **troque quando a performance cair**.
- **Fase 1 enxuta:** R$10–20/dia para tráfego é suficiente para começar a juntar sinal sem queimar budget.
- **Onde ver o gasto:** Gerenciador de Anúncios → coluna **"Valor usado"** (por campanha/conjunto, filtra período) · **Visão geral da conta** ("Valor gasto últimos 7 dias", conta inteira) · **Cobrança e pagamentos** (fatura real). Meta e Google Ads têm faturas **separadas**.
- **Travar teto:** Configurações da conta → **"Limite de gasto da conta"** — máximo absoluto; a Meta para de veicular ao atingir. Recomende setar para não escapar do orçamento.

---

## 9. Fluxo real de criação (Ads Manager)

Ordem para montar do zero (parando antes de publicar):

1. **Campanha:** objetivo (Tráfego/Conversões) → orçamento → Advantage+ (público/posicionamentos) conforme caso.
2. **Conjunto:** budget/dia, localização, público (Amplo/Advantage+ ou Lookalike), posicionamentos.
3. **Anúncio:**
   - **Identidade:** Página + Instagram (resolva §6 ANTES — senão trava no publish).
   - **Formato:** "Imagem ou vídeo único" (single) ou flexível; "Anúncios com vários anunciantes" ok.
   - **Mídia:** usuário sobe os criativos (manual); você seleciona. "Personalizar mídia → Cortar" **só enquadra** (não troca arquivo) — para arte/vídeo dedicado de Stories (2:3) selecione as **duas mídias na criação** (§11).
   - **Texto:** até 5 textos principais, 2 títulos, descrição; **CTA** (ex.: "Saiba mais"). Use JS para setar campos com **acentos** corretos (digitação direta às vezes perde acentuação; anúncio público exige ortografia correta).
   - **Aprimoramentos IA:** todos **OFF**.
   - **Destino:** URL final + UTMs no Rastreamento. ⚠️ O Google/Meta bloqueia **display ≠ destino** (DESTINATION_MISMATCH): se a URL redireciona, use a URL final real.
4. **Parar antes de Publicar** → usuário revisa e publica. Status "Em processamento" = revisão da Meta (normal, minutos a ~1 dia).

**Variante: Impulsionar post existente (Boost)**

Quando o usuário quer promover uma publicação orgânica do feed:
1. Acesse a publicação no Facebook/Instagram → botão **"Impulsionar publicação"**, ou
2. No Gerenciador: Criar → selecione **"Usar publicação existente"** na etapa de anúncio (mais controle de público e budget).
3. Configure público, budget diário e duração.
4. **Diferença importante:** boost não tem pixel de conversão por padrão — se quiser rastrear ações além de cliques, crie o anúncio no Gerenciador apontando para a mesma publicação (opção 2 acima).
5. Objetivo recomendado para boost: **Engajamento** (aumentar curtidas/comentários) ou **Tráfego** (levar ao site).

> ⚠️ Boost via botão direto na publicação tem menos opções de público e não aparece no Gerenciador com detalhes completos — prefira o fluxo pelo Gerenciador quando precisar de controle ou integração com pixel.

---

## 10. Checklist pré-lançamento

- [ ] Pixel instalado; CAPI configurada (dedup OK)
- [ ] Eventos de conversão disparando corretamente
- [ ] Domínio verificado + AEM priorizado
- [ ] **Identidade Instagram resolvida** (sem erro #1815199) — ver §6
- [ ] Landing mobile-friendly e rápida; URL final = destino (sem redirect quebrando)
- [ ] 3+ criativos por conjunto; arte certa por posicionamento
- [ ] Exclusões de público setadas
- [ ] UTMs no Rastreamento
- [ ] Aprimoramentos de IA desligados (se controle de marca importa)

---

## 11. Armadilhas reais (gotchas)

- **#1815199 / identidade IG** → vincule IG à Página e **duplique** o anúncio (§6).
- **Combobox de identidade não troca** → só a duplicação resolve; não perca tempo no clique.
- **DESTINATION_MISMATCH** → URL de exibição ≠ destino (redirect). Use a URL final.
- **Upload de criativo** → não é automatizável; usuário sobe.
- **Acentos no texto** → setar via JS (`value` + evento `input`) preserva; digitação pode falhar.
- **Editor standalone** não duplica/exclui → use a tabela.
- **Pixel órfão** → crie um dedicado no seu BM.
- **Aprimoramentos IA ligados por padrão** → reescrevem texto/arte; desligue se quer controle.
- **Renderer do editor trava** com scroll pesado → recarregue a página para destravar.
- **CAPI "morta" (só Navegador)** → ordem de suspeita: **(1) token inválido pro pixel** após troca de pixel · (2) **Test Event Code preenchido** na tag CAPI · (3) tag server apontando pro pixel antigo (§7). Não é o tráfego. Confirme com GTM Preview.
- **"Confirme os domínios" não some** mesmo com domínio verificado → é a **allowlist do pixel**, não a verificação de domínio (§7).
- **Texto principal "some" no editor** → ao reabrir o anúncio, o campo Texto principal aparece **vazio mesmo já salvo** (Título/Descrição persistem). **Sempre re-verificar e repreencher via JS antes de Publicar**, senão publica sem texto principal.
- **Aprimoramentos IA religam sozinhos** → "Adicionar detalhes", "Revelar detalhes com o tempo", "Retoques/Animação" voltam a cada interação/wizard. Não vale lutar com todos — desligue os **destrutivos de copy** (Gerar CTA, Melhorias/Aprimorar texto) e aceite os benignos de exibição.
- **Seletor de mídia bagunça a seleção** → na biblioteca ("Alterar seleções"), clicar nos thumbs toggla de forma imprevisível (add quando esperava remover). Se a contagem desandar: **Cancelar e reabrir** o seletor (volta ao estado salvo = só a mídia atual) e refazer a seleção limpa. Vídeos ficam na aba **"Vídeos"** (não "Imagens").
- **"Personalizar mídia" só CROPA** → não troca o arquivo por posicionamento, só enquadra a mesma mídia (Quadrado/Vertical/Horizontal → Original ou Cortar). Cortar vertical num 1:1 **corta logo/CTA**. Para vídeo vertical dedicado no Stories: **selecionar as duas mídias (feed + stories) na criação** e deixar a Meta atribuir — não dá pra "substituir" nesse painel.
- **Editar campanha ativa reseta aprendizado** (mudança de público/criativo). Em anúncio recém-publicado o custo é baixo; evite mexer repetido.

---

## 12. Click-to-WhatsApp (anúncio que abre conversa no WhatsApp)

Formato muito usado no Brasil: o clique no anúncio abre o WhatsApp Business direto — sem landing page intermediária.

**Quando usar:** serviços locais, vendas consultivas, agendamentos, qualquer negócio que fecha no chat.

**Configuração no Gerenciador:**
1. Campanha → objetivo **Engajamento** (ou Leads) com "Destinos das mensagens" → WhatsApp. **Não use Tráfego** (manda pro link, não pro WhatsApp).
2. Conjunto → **Destino manual** = o número WhatsApp Business; "Maximizar conversas".
3. Vincule o **WhatsApp Business** à Página do Facebook (Business Settings → Contas do WhatsApp).
4. **Configurador de conversa** (botão "Editar" na seção Conversas) = **2 campos**: **Mensagem de boas-vindas** (saudação automática que a empresa envia) + **Mensagem pronta** (o que o cliente envia ao abrir, ≤80 chars). Setar via JS (acentos).
5. **CTA:** "Enviar mensagem pelo WhatsApp".
6. **Autenticar o número (passo do usuário, pós-publish):** o número entra como "Precisa de autenticação"; depois de publicar, o Facebook manda um **código** pra autenticar no app **WhatsApp Business**. Sem isso, o anúncio roda mas a conversa não abre.

**Rastreamento:** o Meta registra o evento `MessagingConversationStarted` como conversão — configure isso no AEM (§7) se quiser otimizar por esse evento.

**⚠️ Qualidade do lead (lição real):**
- **Engajamento + "Maximizar conversas" atrai LIXO** — otimiza por volume de chat, traz curioso (não comprador). Esperar mais **não conserta**; o algoritmo só fica melhor em trazer "conversador". É estrutural, não fase de aprendizado.
- **Esse objetivo + Advantage+ NÃO expõe segmentação por interesses** — público forçado "amplo" (só dá pra mexer em posicionamento e slider Limitado↔Amplo). Segmentação real por interesse só **recriando a campanha** com público controlado.
- **Saudação qualificadora** (filtro grátis, não reseta nada): faça a mensagem de boas-vindas perguntar o fit — ex. *"Você é desenvolvedor, agência ou tem um negócio querendo automatizar o WhatsApp?"*. Quem não tem fit não responde.
- **Médio prazo:** com a **CAPI saudável** (Lead server-side), migrar de Engajamento → **Conversões/Leads** otimizando por cadastro real → traz cliente, não curioso. A CAPI é pré-requisito dessa evolução.

**Armadilhas:**
- Número desvinculado da Página / **não autenticado** → erro silencioso; anúncio vai pro ar mas destino não abre.
- WhatsApp pessoal (não Business) → Meta recusa a vinculação.
- Objetivo errado (Tráfego) → link quebrado no destino.
- Vídeo 1:1 no Stories/Status do WhatsApp → roda com letterbox (1:1 é aceito); 2:3 **não** está na lista de proporções do Status (9:16, 1.91:1, 16:9, 1:1, 4:5).

---

## Skills relacionadas

- **marketing-expert** — copy, headlines e gatilhos mentais para os criativos.
- **google-ads-manager-2026** — campanhas no Google Ads (Search/PMax/Demand Gen).
- **svg-logo-designer / social-image-studio** — arte e logo dos criativos.
