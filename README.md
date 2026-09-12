# LP Kamilla Fertonani — Nutricionista

Landing page de captação de leads para a nutricionista **Kamilla Fertonani** (emagrecimento definitivo pela saúde intestinal). Objetivo único: levar a visitante ao formulário de agendamento da consulta online.

- **Site no ar:** https://kamillafertonani.sitepreviavisual.site
- **Hospedagem:** GitHub Pages, branch `main`, raiz do repositório
- **Domínio:** `sitepreviavisual.site` fica na Hostinger; o subdomínio é um registro CNAME `kamillafertonani` → `devikison.github.io`. HTTPS obrigatório já ativado no GitHub.
- **Cliente:** Instagram [@kamillafertonani](https://www.instagram.com/kamillafertonani/) · empresa Maka Estética e Saúde Ltda (Santana, São Paulo) · CNPJ 43.480.002/0001-38 · e-mail kamillafertonani@gmail.com

## Como continuar o trabalho em outra máquina ou conta

```bash
git clone https://github.com/Devikison/LP-KAMILLA-FERTONANI.git
```

Abra a pasta no Claude Code (ou no editor) e diga: *"Esta é a landing page da nutricionista Kamilla Fertonani; leia o README.md e o index.html antes de mexer."* Tudo o que existe está em um único arquivo `index.html`, sem build, sem framework, sem dependências além das fontes do Google Fonts.

**Publicar uma alteração:**

```bash
git add -A
git commit -m "descrição da mudança"
git pull --rebase origin main
git push origin main
```

O GitHub Pages republica sozinho em 1 a 2 minutos. Como o `cname` já foi mexido pela API, o GitHub às vezes cria commits "Create CNAME"/"Delete CNAME": por isso o `pull --rebase` antes do push.

**Preview local:** não há Python nem Node na máquina original. Qualquer servidor estático serve (`npx serve`, `python -m http.server`, extensão Live Server). Abrir o `index.html` direto por `file://` também funciona, mas o vídeo e as imagens com `loading="lazy"` podem demorar.

## Estrutura

```
index.html        página inteira: CSS no <style>, JS no <script> final
CNAME             subdomínio do GitHub Pages (não apagar)
assets/
  hero-kamilla.jpg    foto do hero (também usada provisoriamente na seção Sobre)
  dor-01.jpg          card 01 de dores: mulher no sofá com dor na barriga (800x500)
  dor-02.jpg          card 02: mulher frustrada com balança (800x500)
  dor-03.jpg          card 03: mulher desanimada com prato de dieta (800x500)
  orbit.mp4           vídeo da órbita dos 5 pilares (10 MB, 1080x1350, 12,6 s)
```

Imagens que **ainda não existem** e caem em placeholder amarelo ou fallback automaticamente:

| Arquivo | Onde aparece | Tamanho |
|---|---|---|
| `assets/sobre-kamilla.jpg` | seção Sobre (hoje usa a foto do hero) | 550 x 650 |
| `assets/depoimento-01.jpg`, `-02`, `-03` | carrossel de depoimentos (prints de WhatsApp) | 350 x 450 |
| `assets/kamilla-avatar.jpg` | popup flutuante (hoje mostra as iniciais KF) | 200 x 200 |

Basta salvar o arquivo com esse nome na pasta `assets` e fazer o push: as tags `<img>` já existem com `onerror` para esconder quando o arquivo falta. Para a seção Sobre, trocar o `src` de `hero-kamilla.jpg` para `sobre-kamilla.jpg`.

## Ordem das seções

1. **Header** fixo (sticky) com lockup da marca (NUTRICIONISTA em cima, nome embaixo), menu em pílulas "liquid glass" e CTA. No mobile vira hambúrguer.
2. **Hero** — pílula glass com bolinha verde pulsando, título em duas linhas fixas no mobile ("seu intestino." em dourado itálico), texto com frase final em serifa, CTA, linha de apoio, prova social em pílula com avatares e estrelas, foto.
3. **Faixa marquee** dourada com 5 frases em loop contínuo (duplicada e animada em −50%).
4. **Dores** ("Reconhece isso?") — 3 cards com foto, selo numerado e texto. No mobile fazem scroll stacking (sticky).
5. **Comparativo** — card único com toggle Antes/Agora (`#cmpx`, classe `is-new`). Vira sozinho para "Agora" 1,8 s depois de entrar na tela, se ninguém clicou.
6. **Sobre** — foto com selo "+1.000" (contador em câmera lenta + flutuação no scroll), título em duas linhas, lista de 3 cards com check.
7. **Método** — timeline com trilho em tubo de vidro que se preenche com o scroll (`#tlFill`) e medalhões 1, 2, ✓ que acendem.
8. **5 pilares** — órbita com vídeo central e 5 ícones girando; à direita 5 cards que fazem scroll stacking (a órbita fica sticky no desktop).
9. **Depoimentos** — carrossel horizontal com setas.
10. **Quiz** "Como está o seu intestino hoje?" — 5 perguntas, pontuação 0–15, resultado em anel SVG com 3 faixas (verde / dourado / terracota), CTA para o formulário.
11. **FAQ** — 7 perguntas em acordeão (uma aberta por vez).
12. **Formulário** — nome, e-mail, WhatsApp, objetivo (select), mensagem. **Só simula o envio** (mostra a tela de sucesso); não há backend.
13. **Rodapé** — CTA final, colunas Marca / Contato / Redes (5 ícones), linha de direitos com ano automático.
14. **Flutuantes** — popup de chat à direita (aparece após 4 s, fecha por sessão) e aviso de cookies à esquerda (decisão salva em `localStorage` na chave `kf_cookie`).

## Padrões usados no código (para não quebrar ao editar)

- **Cores (CSS vars em `:root`):** `--green #2d5a37`, `--green-bright #3c9a68`, `--sage #6b8e73`, `--gold #c59b5f`, `--gold-light #dfba81`, `--terra #c47c6a`, `--bg #f7f5f4`. Dourado escuro para texto: `#a8813f`.
- **Fontes:** títulos em DM Serif Display (só tem peso 400; nunca usar negrito nela), rótulos e botões em Montserrat, texto em Inter.
- **Reveal no scroll:** qualquer elemento com `data-reveal="up|down|left|right|scale"` entra animado (IntersectionObserver, classe `is-in`; ao terminar ganha `is-revealed`). Em um contêiner, `data-reveal-children="up"` aplica aos filhos com atraso escalonado (`data-reveal-step`). Atraso individual via `style="--rd:.1s"`. O CSS fica em `@layer reveal` para não brigar com os hovers.
- **Scroll stacking:** função `initStack(lista, {gap, only})` no JS. Usada nos 5 pilares (sempre) e nos cards de dores (só `(max-width: 860px)`). Os itens são `position: sticky` com `top` escalonado por `--i`.
- **Contadores:** `data-count="1000" data-prefix="+" data-duration="2600"` sobe de 0 ao valor ao entrar na tela.
- **Flutuação no scroll:** `data-float="14"` (paralaxe suave em px).
- **Quebras de linha fixas:** classe `.h2--split` (cada `<span>` filho vira uma linha) e spans específicos no hero (`.h1-l1`, `.h1-l2`) e no Sobre (`.about__h2-l1`, `.about__h2-l2`).
- **Palavras de impacto:** `<strong>` dentro dos parágrafos vira negrito verde (branco na seção do formulário).
- **Cursor:** seta dourada em SVG aplicada a tudo em dispositivos com mouse (`@media (hover:hover) and (pointer:fine)`). Não existe cursor de "mãozinha": é a mesma seta em links e botões, por decisão do cliente.
- **Barra de rolagem:** estilizada em verde com filete dourado (WebKit + Firefox).
- **Importante:** o contêiner `.wrap` usa `overflow: clip`, e não `hidden`. `hidden` anula todos os `position: sticky` da página (header, órbita, cards).
- **Âncoras internas:** os links `#...` rolam suave e limpam o hash da URL; atualizar a página sempre volta ao topo.
- **Acessibilidade:** tudo respeita `prefers-reduced-motion` (animações desligadas, conteúdo visível).

## Pendências conhecidas

- **CRN** da Kamilla: hoje aparece "CRN-XX" no rodapé (coluna Contato). Trocar nas duas ocorrências.
- **Número do WhatsApp:** os CTAs, o popup e o ícone de WhatsApp do rodapé apontam para `#agendar` (formulário). Se quiser link direto, usar `https://wa.me/55DDDNUMERO?text=...`.
- **Redes sociais:** Facebook, LinkedIn e TikTok no rodapé estão com `href="#"`. Instagram já aponta para o perfil.
- **Formulário sem backend:** integrar com o destino desejado (planilha, e-mail, CRM, n8n). O `<form id="bookForm">` já tem `name` em todos os campos.
- **Vídeo da órbita pesado (10 MB):** exportar de novo em 720 x 900 com bitrate menor para ficar com 2–3 MB.
- **Fotos faltantes:** ver tabela na seção Estrutura.

## Histórico resumido

Criada a partir de um design do Claude Design (formato `.dc.html`), convertida para HTML puro em 11/09/2026 e evoluída em 12/09/2026 com copy focada em conversão (público: mulheres 35–55, dores reais: platô de peso, "canetinhas" Ozempic/Mounjaro, perimenopausa), seções novas (comparativo com toggle e quiz), efeitos de scroll, liquid glass e publicação no GitHub Pages com subdomínio na Hostinger.
