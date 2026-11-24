# Pacote Completo — ComprasPro para GitHub

Aqui estão os arquivos prontos para você subir no seu repositório GitHub.

---

## 1. `manifest.json`

```json
{
  "name": "ComprasPro",
  "short_name": "ComprasPro",
  "description": "Controle rápido e offline de compras, peças, gastos de viagem e relatórios.",
  "start_url": "./index.html",
  "scope": "./",
  "display": "standalone",
  "orientation": "portrait",
  "background_color": "#eceff3",
  "theme_color": "#101c2e",
  "icons": [
    {
      "src": "data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><rect width='100' height='100' rx='22' fill='%23101c2e'/><text x='50' y='66' font-size='54' text-anchor='middle'>🛍️</text></svg>",
      "sizes": "192x192",
      "type": "image/svg+xml",
      "purpose": "any maskable"
    },
    {
      "src": "data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><rect width='100' height='100' rx='22' fill='%23101c2e'/><text x='50' y='66' font-size='54' text-anchor='middle'>🛍️</text></svg>",
      "sizes": "512x512",
      "type": "image/svg+xml",
      "purpose": "any maskable"
    }
  ]
}
```

---

## 2. `sw.js`

```javascript
const CACHE_NAME = 'compraspro-offline-v4';
const ASSETS = [
  './',
  './index.html',
  './manifest.json'
];

self.addEventListener('install', (event) => {
  self.skipWaiting();
  event.waitUntil(
    caches.open(CACHE_NAME).then((cache) => cache.addAll(ASSETS)).catch(() => {})
  );
});

self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((keys) =>
      Promise.all(keys.filter((k) => k !== CACHE_NAME).map((k) => caches.delete(k)))
    ).then(() => self.clients.claim())
  );
});

self.addEventListener('fetch', (event) => {
  if (event.request.method !== 'GET') return;
  event.respondWith(
    caches.match(event.request).then((cached) => {
      return cached || fetch(event.request).then((response) => {
        if (response && response.status === 200) {
          const copy = response.clone();
          caches.open(CACHE_NAME).then((cache) => cache.put(event.request, copy)).catch(() => {});
        }
        return response;
      }).catch(() => caches.match('./index.html'));
    })
  );
});
```

---

## 3. `README.md`

```markdown
# ComprasPro

Aplicativo leve, rápido e instalável (PWA) para controle de compras em viagem, gestão de fornecedores, gastos de viagem e relatórios consolidados.

## Arquivos do projeto
- `index.html`: Aplicativo completo.
- `manifest.json`: Configuração para instalar como aplicativo no celular.
- `sw.js`: Cache offline para abrir mesmo sem internet.

## Como publicar no GitHub Pages
1. Crie um repositório no GitHub (ex: `compraspro`).
2. Envie os arquivos (`index.html`, `manifest.json`, `sw.js` e `README.md`) para a raiz do repositório.
3. Acesse **Settings > Pages** no repositório.
4. Em **Source**, selecione **Deploy from a branch**, escolha `main` e `/ (root)` e clique em **Save**.
5. Abra o link gerado no celular e toque em **Instalar aplicativo** (Android/Chrome) ou **Adicionar à Tela de Início** (iPhone/Safari).
```

---

## 4. `index.html`

O código completo do `index.html` já está na aba **`Página Principal (GitHub Pages)`** (`index.html`) e também em **`ComprasPro Organizado e Protegido`** (`compraspro_viagem.html`).