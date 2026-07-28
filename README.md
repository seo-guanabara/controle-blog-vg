# Controle de Publicação — Blog Viaje Guanabara

Dashboard que verifica automaticamente, todo dia às 6h (horário de Brasília), se cada um dos 95 artigos planejados para o blog da Viaje Guanabara está realmente publicado — consultando a API pública do WordPress, sem precisar de login.

## Como configurar (primeira vez)

### 1. Criar o repositório

No GitHub, clique em **New repository** (botão verde no canto superior direito da sua conta). Dê um nome (ex: `controle-blog-vg`), marque como **Public** (necessário para o GitHub Pages gratuito funcionar) e clique em **Create repository**.

### 2. Subir os arquivos

Na página do repositório recém-criado, clique em **uploading an existing file** (ou arraste os arquivos direto). Suba **todos** os arquivos e pastas desta entrega, mantendo a estrutura exata:

```
.github/workflows/validar.yml
data/resultado.json
index.html
validar_publicacao.py
README.md
```

**Atenção:** a pasta `.github` começa com ponto — alguns navegadores escondem isso ao arrastar arquivos. Se o GitHub não reconhecer a pasta `.github/workflows/`, use o botão **Add file → Create new file**, digite o caminho completo `.github/workflows/validar.yml` na caixa de nome (o GitHub cria as pastas automaticamente) e cole o conteúdo.

Depois de subir tudo, clique em **Commit changes**.

### 3. Ativar o GitHub Pages

Vá em **Settings** (do repositório) → **Pages** (menu lateral esquerdo) → em "Source", selecione **Deploy from a branch** → branch **main**, pasta **/ (root)** → **Save**.

Depois de 1-2 minutos, o GitHub mostra o link da sua página (algo como `https://seu-usuario.github.io/controle-blog-vg/`).

### 4. Rodar a primeira validação manualmente

Vá na aba **Actions** do repositório → clique no workflow **"Validar Publicação dos Artigos"** na lista à esquerda → botão **Run workflow** → **Run workflow** de novo para confirmar.

Aguarde cerca de 1 minuto. Quando o círculo ficar verde, o arquivo `data/resultado.json` foi atualizado com os dados reais.

### 5. Pronto

A partir daqui, o workflow roda sozinho todo dia às 6h. Para forçar uma atualização fora do horário, repita o passo 4 a qualquer momento.

## Estrutura dos arquivos

- **`index.html`** — o dashboard visual (filtros, busca, ordenação)
- **`validar_publicacao.py`** — script que consulta a API do WordPress e gera o resultado
- **`data/resultado.json`** — o resultado mais recente (atualizado automaticamente)
- **`.github/workflows/validar.yml`** — a automação que roda o script periodicamente

## Por que isso funciona sem bloqueio de CORS

O script roda **no servidor do GitHub** (dentro do GitHub Actions), não no navegador de quem abre a página. A restrição de CORS existe apenas para proteger requisições feitas de dentro do navegador — chamadas servidor-para-servidor não passam por essa regra. O `index.html` nunca faz chamada direta ao WordPress; ele só lê o arquivo `data/resultado.json`, que já está no mesmo domínio da própria página (GitHub Pages), então não existe CORS ali também.
