# Site do AstroQuiz

Página de suporte e arquivo `app-ads.txt` do **AstroQuiz Challenge**.

| Arquivo | Para quê |
|---|---|
| `index.html` | página de suporte — é a URL declarada na App Store |
| `app-ads.txt` | verificação do AdMob |
| `astroquiz/index.html` | redireciona para a raiz (a App Store ainda aponta para cá) |

---

## Por que este repositório precisa de um domínio próprio

O rastreador do AdMob **remove o subdomínio** e procura o `app-ads.txt` no domínio raiz.
A tabela oficial do Google mostra o comportamento:

| Site do desenvolvedor | Onde o AdMob procura |
|---|---|
| `https://exemplo.com/jogo` | `exemplo.com/app-ads.txt` |
| `https://ajuda.exemplo.com/jogo` | `exemplo.com/app-ads.txt` |
| `https://www.exemplo.com/jogo` | `exemplo.com/app-ads.txt` |

Como este site vive em `robertomuricy.github.io`, o AdMob procura em **`github.io/app-ads.txt`** —
domínio do GitHub, não nosso. Por isso a verificação falha, e falharia para sempre.

O mesmo vale para qualquer subdomínio de serviço: `*.vercel.app`, `*.netlify.app`, `*.pages.dev`.

**Só um domínio próprio resolve.**

---

## Passo a passo depois de registrar o domínio

Supondo `astroquiz.app` como exemplo — troque pelo que você registrar.

### 1. DNS, no painel do registrador

Para o domínio apex (sem `www`), crie **quatro registros A** apontando para o GitHub Pages:

```
A    @    185.199.108.153
A    @    185.199.109.153
A    @    185.199.110.153
A    @    185.199.111.153
```

E um CNAME para o `www`:

```
CNAME    www    robertomuricy.github.io
```

### 2. No repositório

**Settings → Pages → Custom domain**, informe `astroquiz.app` e salve.
O GitHub cria o arquivo `CNAME` sozinho. Marque **Enforce HTTPS** assim que o certificado for emitido
(costuma levar de minutos a uma hora).

### 3. Confirme que o arquivo responde na raiz

```bash
curl -s https://astroquiz.app/app-ads.txt
# google.com, pub-2123152010659436, DIRECT, f08c47fec0942fa0
```

### 4. Atualize a ficha da App Store

Em **App Store Connect → versão → URL de marketing**, troque para `https://astroquiz.app`.

O AdMob lê o `app-ads.txt` a partir do campo **URL de marketing** — não do de suporte.
Vale atualizar os dois, mas o de marketing é o que decide a verificação.

### 5. No AdMob

**Apps → AstroQuiz (iOS) → Verificar se há atualizações.**
A Apple pode levar até 24 h para propagar a mudança da ficha, e o rastreador do Google
tem o seu próprio ciclo. Não refaça o arquivo enquanto espera: cada alteração reinicia a contagem.

### 6. Android, quando for publicado

O mesmo `app-ads.txt` serve para as duas lojas. Basta declarar o mesmo domínio
no campo de site do desenvolvedor no Google Play.
