# 🚀 Guia Rápido - Criador UGC

## 📥 Instalação (60 segundos)

### Opção 1: Servidor Web Local
```bash
# Copiar arquivo para servidor
cp ugc-creator.html /var/www/html/

# Acessar no browser
http://localhost/ugc-creator.html
```

### Opção 2: Abrir Direto no Browser
```bash
# Windows
start ugc-creator.html

# macOS
open ugc-creator.html

# Linux
xdg-open ugc-creator.html
```

### Opção 3: Servidor Python Rápido
```bash
# Python 3
python -m http.server 8000

# Acessar em http://localhost:8000/ugc-creator.html
```

---

## ⚙️ Configuração

### Alterar URL do Webhook
Encontre esta linha no código (aproximadamente linha 360):
```javascript
const response = await fetch('http://localhost:5678/webhook-test/ugc', {
    method: 'POST',
    body: formData
});
```

**Para ambiente de produção:**
```javascript
const response = await fetch('https://seu-dominio.com/webhook/ugc', {
    method: 'POST',
    body: formData
});
```

### Customizar Cores
Procure pela seção `/* Paleta de Cores */` no CSS:
```css
/* Primário - Roxo/Azul */
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);

/* Sucesso - Verde */
background: #48bb78;

/* Erro - Vermelho */
background: #c53030;
```

**Trocar tema (exemplo: para azul):**
```css
/* De: */
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);

/* Para: */
background: linear-gradient(135deg, #0ea5e9 0%, #1e40af 100%);
```

### Customizar Textos
Procure pelos elementos com `textContent` e `innerHTML`:
```javascript
// Mensagens
<p>Processando seu vídeo...</p>
<small>Isso pode levar até 5 minutos</small>

// Labels
<label>Imagem do Produto</label>

// Placeholders
placeholder="Ex: Fone Bluetooth Premium"
```

---

## 🎨 Opções de Tema Pré-configuradas

### Tema Dark
```css
body {
    background: linear-gradient(135deg, #1a202c 0%, #2d3748 100%);
}

.container {
    background: #2d3748;
    color: #e2e8f0;
}
```

### Tema Verde (Eco-friendly)
```css
.btn-generate {
    background: linear-gradient(135deg, #10b981 0%, #047857 100%);
}
```

### Tema Rosa (Moderno)
```css
.btn-generate {
    background: linear-gradient(135deg, #ec4899 0%, #be185d 100%);
}
```

---

## 📱 Responsividade - Testes

### Mobile (iPhone)
```
Viewport: 375x812px
Teste: Clique no upload area
Resultado esperado: Foto da câmera/galeria abre
```

### Tablet (iPad)
```
Viewport: 768x1024px
Teste: Arraste imagem
Resultado esperado: Preview aparece suavemente
```

### Desktop (Full)
```
Viewport: 1920x1080px
Teste: Hover nos botões
Resultado esperado: Elevação com sombra
```

---

## ✅ Checklist de Funcionalidades

### Upload
- [ ] Clique abre seletor de arquivo
- [ ] Drag & drop funciona
- [ ] Preview mostra imagem
- [ ] Botão "Remover" funciona
- [ ] Validação de tipo (apenas imagem)
- [ ] Validação de tamanho (máx 10MB)

### Formulário
- [ ] Campo de produto obrigatório
- [ ] Focus visual no input
- [ ] Placeholder descritivo
- [ ] Botão gerar desabilitado sem imagem/nome

### Processamento
- [ ] Loading spinner aparece
- [ ] Botões desabilitados durante envio
- [ ] Requisição POST correta
- [ ] Timeout adequado (até 5 min)

### Resultado
- [ ] Vídeo carrega e exibe
- [ ] Controles de reprodução funcionam
- [ ] Download link ativo
- [ ] Alerta de sucesso mostra

### Erro
- [ ] Mensagens de erro claras
- [ ] Alert desaparece automaticamente
- [ ] Usuário pode tentar novamente
- [ ] Estado volta ao normal

---

## 🔗 Estrutura de Requisição/Resposta

### REQUEST (Client → Server)
```
POST /webhook-test/ugc
Content-Type: multipart/form-data

FormData:
├── image: <File>          // Arquivo da imagem
└── productName: <String>  // Ex: "Fone Bluetooth"
```

### RESPONSE (Server → Client)
```json
{
    "videoUrl": "https://cdn.example.com/video.mp4",
    "status": "success",
    "processingTime": "120" // segundos (opcional)
}
```

### RESPONSE ERROR
```json
{
    "error": "Erro ao processar imagem",
    "status": "error"
}
```

---

## 🐛 Troubleshooting

### Problema: Imagem não aparece após upload
**Solução:**
```javascript
// Adicione console.log para debug
reader.onload = (e) => {
    console.log('Imagem carregada:', e.target.result);
    imagePreview.src = e.target.result;
};
```

### Problema: Webhook retorna erro
**Checklist:**
- [ ] URL está correta?
- [ ] Server está rodando?
- [ ] CORS está configurado?
- [ ] Dados estão no formato correto?

```javascript
// Teste com curl
curl -F "image=@imagem.jpg" \
     -F "productName=Produto" \
     http://localhost:5678/webhook-test/ugc
```

### Problema: Vídeo não carrega
**Solução:**
```javascript
videoPlayer.addEventListener('error', (e) => {
    console.error('Erro ao carregar vídeo:', e);
    showAlert('Vídeo não pode ser reproduzido', 'error');
});
```

### Problema: Timeout
**Se o servidor demora muito:**
```javascript
// Aumentar timeout (definio como 10 minutos)
const response = await fetch(url, {
    method: 'POST',
    body: formData,
    signal: AbortSignal.timeout(600000) // 10 min
});
```

---

## 📊 Analytics (Opcional)

### Rastrear Submissões
```javascript
// Google Analytics
gtag('event', 'ugc_video_generated', {
    product_name: productName.value,
    file_size: selectedImage.size
});

// Ou seu próprio backend
fetch('http://seu-servidor.com/analytics', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        event: 'video_generated',
        timestamp: new Date(),
        productName: productName.value
    })
});
```

---

## 🔒 Segurança - Recomendações

### 1. Validação no Backend
```javascript
// Seu webhook deve:
✓ Validar tipo MIME do arquivo
✓ Verificar dimensões mínimas da imagem
✓ Limpar nome do produto (sanitizar)
✓ Implementar rate limiting
✓ Armazenar logs de submissão
```

### 2. CORS Configuração
```javascript
// Se usar servidor Node/Express
app.use(cors({
    origin: 'https://seu-dominio.com',
    methods: ['POST'],
    credentials: false
}));
```

### 3. Limite de Arquivo
```javascript
// Aumentar/diminuir no código:
// Linha ~200
if (file.size > 10 * 1024 * 1024) { // 10MB
    // Rejeitar
}
```

---

## 📈 Performance

### Otimizações Implementadas
- ✓ Zero dependências externas
- ✓ CSS inline (sem requests extras)
- ✓ JavaScript vanilla (sem framework)
- ✓ Animações via CSS (GPU accelerated)
- ✓ Lazy loading de vídeo

### Métricas Esperadas
```
Tamanho do arquivo: ~17KB
Load time: <100ms
First Contentful Paint: <500ms
Time to Interactive: <1s
Lighthouse Score: 95+
```

---

## 🎓 Personalizações Avançadas

### Adicionar Validação Customizada
```javascript
function validateProductName(name) {
    const minLength = 3;
    const maxLength = 50;
    
    if (name.length < minLength || name.length > maxLength) {
        showAlert(`Nome deve ter entre ${minLength} e ${maxLength} caracteres`, 'error');
        return false;
    }
    return true;
}

// Usar no submit:
if (!validateProductName(productName.value)) return;
```

### Adicionar Preview de Vídeo
```javascript
// Mostrar thumbnail enquanto carrega
const videoThumbnail = document.createElement('img');
videoThumbnail.src = data.thumbnailUrl; // Se webhook retornar
videoContainer.appendChild(videoThumbnail);
```

### Suporte a Múltiplas Imagens
```javascript
let images = [];

function addAnotherImage() {
    // Criar novo upload area
    // Permitir múltiplos arquivos
    // Enviar array para webhook
}
```

---

## 📞 Suporte & Recursos

### Documentação Completa
Veja `ANALISE-SISTEMA.md` para detalhes técnicos

### Stack Utilizado
- HTML5 (Semantic)
- CSS3 (Gradients, Animations, Media Queries)
- JavaScript ES6+ (Fetch, FormData, FileReader)

### Browser Support
- Chrome/Edge 88+
- Firefox 87+
- Safari 14+
- Mobile browsers (iOS Safari, Chrome Android)

---

## 🎉 Próximos Passos

1. **Implemente o Webhook**
   - Receba POST com imagem + nome
   - Processe com seu SDK de IA
   - Retorne URL do vídeo

2. **Teste Localmente**
   - Configure servidor Python/Node
   - Execute o HTML
   - Submeta dados de teste

3. **Deploy**
   - Escolha host (Vercel, Netlify, próprio servidor)
   - Configure domínio
   - Atualize URLs no código

4. **Monitore**
   - Rastreie erros e performance
   - Colete feedback dos usuários
   - Otimize continuamente

---

**Pronto para começar? Abra `ugc-creator.html` no seu browser! 🚀**
