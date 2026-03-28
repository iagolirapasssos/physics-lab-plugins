# physics-lab-plugins
Physics Lab IDE Pro - Plugin Repository

Bem-vindo ao repositório oficial de plugins para o **Physics Lab IDE Pro**! 

Este repositório contém plugins desenvolvidos pela comunidade para estender as funcionalidades do Physics Lab IDE Pro, adicionando novos objetos, ferramentas e simulações físicas.

## 📋 Índice

- [O que é Physics Lab IDE Pro?](#o-que-é-physics-lab-ide-pro)
- [Plugins Disponíveis](#plugins-disponíveis)
- [Como Instalar Plugins](#como-instalar-plugins)
- [Como Criar um Plugin](#como-criar-um-plugin)
- [API e Referência](#api-e-referência)
- [Contribuindo](#contribuindo)
- [Licença](#licença)

## 🚀 O que é Physics Lab IDE Pro?

Physics Lab IDE Pro é uma plataforma interativa para simulação de fenômenos físicos, desenvolvida para professores e estudantes. Com ela, você pode:

- Criar e simular experimentos físicos em tempo real
- Desenhar objetos e convertê-los em corpos físicos
- Traçar raios de luz e simular óptica geométrica
- Simular fluidos com dinâmica SPH
- Conectar objetos com cordas, hastes e molas

Os plugins permitem estender essas capacidades com novos objetos personalizados!

## 📦 Plugins Disponíveis

### Mecânica
| Plugin | Descrição | Autor |
|--------|-----------|-------|
| Double Pendulum | Simulação de pêndulo duplo com caos | [@user1](https://github.com/user1) |
| Atwood Machine | Máquina de Atwood com polia real | [@user2](https://github.com/user2) |
| Rocket Simulator | Propulsão de foguete com combustível | [@user3](https://github.com/user3) |

### Óptica
| Plugin | Descrição | Autor |
|--------|-----------|-------|
| Diffraction Grating | Rede de difração com espectro de cores | [@user4](https://github.com/user4) |
| Optical Fiber | Simulação de fibra óptica | [@user5](https://github.com/user5) |

### Eletromagnetismo
| Plugin | Descrição | Autor |
|--------|-----------|-------|
| Magnetic Field | Campo magnético de ímãs e correntes | [@user6](https://github.com/user6) |
| RLC Circuit | Circuito RLC com gráficos | [@user7](https://github.com/user7) |

## 🔧 Como Instalar Plugins

### Método 1: Via Interface do Physics Lab

1. Abra o Physics Lab IDE Pro
2. Clique no botão "Gerenciar Plugins" no painel de propriedades
3. Na aba "GitHub", insira a URL do repositório
4. Clique em "Carregar do GitHub"
5. Selecione os plugins desejados e clique em "Instalar"

### Método 2: Instalação Manual

1. Baixe o arquivo `.plugin.json` desejado
2. No Physics Lab, abra o Gerenciador de Plugins
3. Clique em "Upload Plugin (.json)"
4. Selecione o arquivo baixado

## 🛠️ Como Criar um Plugin

### Estrutura Básica

```json
{
  "name": "Meu Plugin",
  "version": "1.0.0",
  "author": "Seu Nome",
  "description": "Descrição do seu plugin",
  "category": "mechanics",
  "objects": [...],
  "tools": [...],
  "simulations": [...]
}
```

### Criando um Objeto Simples

```json
{
  "id": "custom_ball",
  "name": "Bola Personalizada",
  "icon": "circle",
  "type": "shape",
  "defaultProps": {
    "w": 30,
    "mass": 1,
    "fill": "#ff6b6b",
    "bounciness": 0.8
  },
  "draw": "function(ctx, obj) { ... }",
  "update": "function(obj, dt) { ... }"
}
```

## 📚 API e Referência

### Objetos Disponíveis

O sistema fornece acesso a todos os objetos do Physics Lab através do namespace `Lab`:

```javascript
// Acessar objetos
Lab.state.objects          // Todos os objetos
Lab.objects.get(id)        // Obter objeto por ID
Lab.objects.add(obj)       // Adicionar novo objeto
Lab.objects.update(id, props) // Atualizar objeto
Lab.objects.remove(id)     // Remover objeto

// Acessar estado global
Lab.state.W                // Largura do canvas
Lab.state.H                // Altura do canvas
Lab.state.gravity          // Gravidade atual
Lab.state.isSimulating     // Status da simulação

// Utilitários
Lab.utils.getCenter(obj)   // Obter centro do objeto
Lab.utils.rotPoint(px, py, cx, cy, rad) // Rotacionar ponto
Lab.utils.raySegIntersect(...) // Interseção raio-segmento
```

### Funções de Desenho

```javascript
// Contexto do Canvas (ctx) está disponível
// Exemplo de draw function
draw: `function(ctx, obj) {
  ctx.save();
  
  // Aplicar rotação se necessário
  if (obj.angle) {
    const center = Lab.utils.getCenter(obj);
    ctx.translate(center.x, center.y);
    ctx.rotate(obj.angle * Math.PI / 180);
    ctx.translate(-center.x, -center.y);
  }
  
  // Desenhar o objeto
  ctx.fillStyle = obj.fill;
  ctx.fillRect(obj.x, obj.y, obj.w, obj.h);
  
  // Texto adicional
  ctx.fillStyle = '#ffffff';
  ctx.font = '12px monospace';
  ctx.fillText('⭐', obj.x + 10, obj.y + 20);
  
  ctx.restore();
}`
```

### Funções de Física

```javascript
update: `function(obj, dt) {
  // Aplicar forças personalizadas
  const forceX = 10;
  const forceY = -5;
  
  obj.vx += forceX * dt / (obj.mass || 1);
  obj.vy += forceY * dt / (obj.mass || 1);
  
  // Atualizar posição
  obj.x += obj.vx * dt;
  obj.y += obj.vy * dt;
}`
```

### Criando Ferramentas

```javascript
{
  "id": "my_tool",
  "name": "Minha Ferramenta",
  "icon": "build",
  "category": "mechanics",
  "action": `function() {
    alert("Ferramenta ativada!");
    
    // Criar um novo objeto
    Lab.objects.add({
      type: 'circle',
      x: Lab.state.W/2,
      y: Lab.state.H/2,
      w: 30,
      fill: '#ff6b6b',
      mass: 1
    });
  }`
}
```

### Criando Simulações

```javascript
{
  "id": "my_simulation",
  "name": "Minha Simulação",
  "description": "Descrição detalhada",
  "icon": "science",
  "setup": `function() {
    console.log("Simulação iniciada");
    // Criar objetos iniciais
    Lab.state.objects.push({
      id: Lab.state.nextId++,
      type: 'circle',
      x: Lab.state.W/2,
      y: Lab.state.H/2,
      w: 20,
      mass: 1,
      vx: 50,
      vy: 0
    });
    Lab.render.all();
  }`,
  "update": `function(dt) {
    // Lógica de atualização a cada frame
    for (const obj of Lab.state.objects) {
      if (obj.type === 'circle') {
        // Força central
        const center = { x: Lab.state.W/2, y: Lab.state.H/2 };
        const dx = center.x - obj.x;
        const dy = center.y - obj.y;
        const r = Math.hypot(dx, dy);
        const force = 100 / (r * r);
        
        obj.vx += (dx / r) * force * dt;
        obj.vy += (dy / r) * force * dt;
      }
    }
  }`,
  "cleanup": `function() {
    console.log("Simulação finalizada");
    // Limpar objetos criados
    Lab.state.objects = Lab.state.objects.filter(o => o.type !== 'circle');
    Lab.render.all();
  }`
}
```

## 🔧 Ferramentas de Apoio

### 1. Validador de Plugin

Use o script de validação para verificar seu plugin:

```bash
node scripts/validate-plugin.js meu-plugin.plugin.json
```

### 2. Template Generator

```javascript
// Gerador de template
function generatePluginTemplate(name, author) {
  return {
    name: name,
    version: "1.0.0",
    author: author,
    description: `Plugin ${name} para Physics Lab`,
    category: "mechanics",
    objects: [],
    tools: [],
    simulations: []
  };
}
```

### 3. Debug Tools

```javascript
// Adicione ao seu plugin para debug
const debug = {
  log: (msg) => console.log(`[Plugin] ${msg}`),
  drawBoundingBox: (ctx, obj) => {
    ctx.strokeStyle = 'red';
    ctx.strokeRect(obj.x, obj.y, obj.w, obj.h);
  },
  drawVelocity: (ctx, obj) => {
    ctx.beginPath();
    ctx.moveTo(obj.x, obj.y);
    ctx.lineTo(obj.x + obj.vx, obj.y + obj.vy);
    ctx.strokeStyle = 'blue';
    ctx.stroke();
  }
};
```

## 📝 Contribuindo

1. Fork este repositório
2. Crie sua branch (`git checkout -b feature/meu-plugin`)
3. Adicione seu plugin na pasta apropriada
4. Atualize o README com a descrição do seu plugin
5. Commit suas mudanças (`git commit -m 'Adiciona plugin: Meu Plugin'`)
6. Push para a branch (`git push origin feature/meu-plugin`)
7. Abra um Pull Request

### Checklist para Pull Request

- [ ] Plugin validado com o script de validação
- [ ] Documentação atualizada
- [ ] Testado no Physics Lab IDE Pro
- [ ] Não contém código malicioso
- [ ] Segue as boas práticas

## 📄 Licença

MIT License - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 🙏 Agradecimentos

Agradecemos a todos os contribuidores que ajudam a tornar o Physics Lab IDE Pro uma ferramenta cada vez mais poderosa!

---

**Links Úteis:**
- [Physics Lab IDE Pro - Site Oficial](https://physics-lab.example.com)
- [Documentação da API](docs/api-reference.md)
- [Fórum da Comunidade](https://community.physics-lab.example.com)
- [Reportar Bug](https://github.com/yourusername/physics-lab-plugins/issues)
```

---

## Tutorial de Criação de Plugins

### Passo 1: Configure seu ambiente

```bash
# Crie uma pasta para seu plugin
mkdir meu-plugin-physics
cd meu-plugin-physics

# Inicie um repositório git
git init

# Crie o arquivo do plugin
touch meu-plugin.plugin.json
```

### Passo 2: Estruture seu plugin

```json
{
  "name": "Meu Primeiro Plugin",
  "version": "1.0.0",
  "author": "Seu Nome",
  "description": "Um plugin simples que adiciona uma bola saltitante personalizada",
  "category": "mechanics",
  "objects": [
    {
      "id": "bouncing_ball",
      "name": "Bola Saltitante",
      "icon": "sports_basketball",
      "type": "shape",
      "defaultProps": {
        "w": 25,
        "mass": 1,
        "fill": "#ff6b6b",
        "bounciness": 0.9,
        "vx": 50,
        "vy": 0
      },
      "draw": "function(ctx, obj) {\n  ctx.save();\n  \n  // Desenhar sombra\n  ctx.shadowBlur = 10;\n  ctx.shadowColor = 'rgba(0,0,0,0.3)';\n  \n  // Desenhar círculo\n  ctx.beginPath();\n  ctx.arc(obj.x, obj.y, obj.w, 0, 2 * Math.PI);\n  \n  // Gradiente para efeito 3D\n  const grad = ctx.createRadialGradient(\n    obj.x - obj.w/3, obj.y - obj.w/3, obj.w/4,\n    obj.x, obj.y, obj.w\n  );\n  grad.addColorStop(0, obj.fill);\n  grad.addColorStop(1, '#c44545');\n  ctx.fillStyle = grad;\n  ctx.fill();\n  \n  // Brilho\n  ctx.beginPath();\n  ctx.arc(obj.x - obj.w/4, obj.y - obj.w/4, obj.w/6, 0, 2 * Math.PI);\n  ctx.fillStyle = 'rgba(255,255,255,0.6)';\n  ctx.fill();\n  \n  ctx.restore();\n}",
      "update": "function(obj, dt) {\n  // Aplicar gravidade\n  obj.vy += Lab.state.gravity * dt;\n  \n  // Atualizar posição\n  obj.x += obj.vx * dt;\n  obj.y += obj.vy * dt;\n  \n  // Colisão com bordas\n  const radius = obj.w;\n  if (obj.y + radius > Lab.state.H) {\n    obj.y = Lab.state.H - radius;\n    obj.vy = -obj.vy * obj.bounciness;\n  }\n  if (obj.y - radius < 0) {\n    obj.y = radius;\n    obj.vy = -obj.vy * obj.bounciness;\n  }\n  if (obj.x + radius > Lab.state.W) {\n    obj.x = Lab.state.W - radius;\n    obj.vx = -obj.vx * obj.bounciness;\n  }\n  if (obj.x - radius < 0) {\n    obj.x = radius;\n    obj.vx = -obj.vx * obj.bounciness;\n  }\n}",
      "properties": [
        {
          "name": "mass",
          "label": "Massa (kg)",
          "type": "number",
          "default": 1,
          "min": 0.1,
          "max": 10
        },
        {
          "name": "fill",
          "label": "Cor",
          "type": "color",
          "default": "#ff6b6b"
        },
        {
          "name": "bounciness",
          "label": "Elasticidade",
          "type": "range",
          "default": 0.9,
          "min": 0,
          "max": 1,
          "step": 0.01
        },
        {
          "name": "vx",
          "label": "Velocidade X (px/s)",
          "type": "number",
          "default": 50,
          "min": -200,
          "max": 200
        }
      ]
    }
  ],
  "tools": [
    {
      "id": "spawn_ball",
      "name": "Criar Bola",
      "icon": "add_circle",
      "category": "mechanics",
      "action": "function() {\n  const x = Lab.state.W/2;\n  const y = 100;\n  \n  Lab.objects.add({\n    type: 'circle',\n    pluginId: 'bouncing_ball',\n    x: x,\n    y: y,\n    w: 25,\n    mass: 1,\n    fill: '#ff6b6b',\n    bounciness: 0.9,\n    vx: (Math.random() - 0.5) * 100,\n    vy: 0\n  });\n  \n  alert('Bola criada!');\n}"
    }
  ],
  "simulations": [
    {
      "id": "bouncing_balls",
      "name": "Múltiplas Bolas",
      "description": "Cria várias bolas saltitantes",
      "icon": "casino",
      "setup": "function() {\n  for (let i = 0; i < 10; i++) {\n    Lab.objects.add({\n      type: 'circle',\n      pluginId: 'bouncing_ball',\n      x: 100 + i * 80,\n      y: 100,\n      w: 20 + Math.random() * 15,\n      mass: 0.5 + Math.random(),\n      fill: `hsl(${Math.random() * 360}, 70%, 60%)`,\n      bounciness: 0.7 + Math.random() * 0.3,\n      vx: (Math.random() - 0.5) * 80,\n      vy: 0\n    });\n  }\n  alert('10 bolas criadas!');\n}",
      "update": "function(dt) {\n  // Atualização já é feita pelo update de cada objeto\n}",
      "cleanup": "function() {\n  Lab.state.objects = Lab.state.objects.filter(o => o.pluginId !== 'bouncing_ball');\n  alert('Todas as bolas removidas!');\n}"
    }
  ]
}
```

### Passo 3: Teste seu plugin

```javascript
// No console do navegador, com o Physics Lab aberto:
Lab.plugins.loadPluginFromURL('http://localhost:8000/meu-plugin.plugin.json');
```

### Passo 4: Publique no GitHub

```bash
# Adicione o arquivo
git add meu-plugin.plugin.json
git commit -m "Adiciona plugin: Bola Saltitante"
git push origin main
```

---

## APIs e Ferramentas Disponíveis

### 1. Namespace Lab - Objetos Globais

```javascript
// Estado global
Lab.state = {
  objects: [],           // Array de objetos
  strokes: [],           // Desenhos livres
  selectedId: null,      // ID do objeto selecionado
  isSimulating: false,   // Simulação ativa
  gravity: 9.8,          // Gravidade (m/s²)
  W: 1100,               // Largura do canvas
  H: 700                 // Altura do canvas
}

// Sistema de objetos
Lab.objects = {
  add(obj) {},           // Adicionar objeto
  get(id) {},            // Obter por ID
  update(id, props) {},  // Atualizar
  remove(id) {}          // Remover
}

// Sistema de renderização
Lab.render = {
  all() {}               // Renderizar tudo
}

// Utilitários
Lab.utils = {
  getCenter(obj) {},     // Centro do objeto
  rotPoint(px, py, cx, cy, rad) {}, // Rotacionar ponto
  hitTest(x, y) {},      // Teste de colisão
  pointToSegmentDistance(px, py, x1, y1, x2, y2) {} // Distância ponto-segmento
}
```

### 2. Constantes e Configurações

```javascript
Lab.config = {
  MAX_RAY_BOUNCES: 10,
  RAY_MIN_INTENSITY: 0.02,
  LASER_COLORS: { red: '#ef4444', green: '#10b981', violet: '#8b5cf6' },
  N_AIR: 1.0
}
```

### 3. Eventos Disponíveis

```javascript
// No objeto, você pode usar estes eventos
{
  onClick: "function(obj, mouseX, mouseY) { ... }",
  onDrag: "function(obj, dx, dy) { ... }",
  onSelect: "function(obj) { ... }",
  onDeselect: "function(obj) { ... }",
  onCollision: "function(obj, otherObj) { ... }"
}
```

### 4. Ferramentas de Desenho

```javascript
// Contexto 2D completo disponível
// Propriedades úteis:
ctx.fillStyle = '#ff0000';
ctx.strokeStyle = '#000000';
ctx.lineWidth = 2;
ctx.font = '12px monospace';

// Métodos úteis:
ctx.fillRect(x, y, w, h);
ctx.strokeRect(x, y, w, h);
ctx.beginPath();
ctx.arc(x, y, radius, 0, 2 * Math.PI);
ctx.fill();
ctx.stroke();
ctx.save();
ctx.restore();
ctx.translate(x, y);
ctx.rotate(angle);
ctx.scale(x, y);
```

### 5. Sistema de Propriedades

```javascript
// Tipos de propriedades suportados
properties: [
  { name: "number_prop", label: "Número", type: "number", default: 0, min: -100, max: 100 },
  { name: "range_prop", label: "Slider", type: "range", default: 0.5, min: 0, max: 1, step: 0.01 },
  { name: "color_prop", label: "Cor", type: "color", default: "#ff0000" },
  { name: "boolean_prop", label: "Opção", type: "boolean", default: false },
  { name: "select_prop", label: "Escolha", type: "select", default: "opcao1", options: ["opcao1", "opcao2"] }
]
```

### 6. Scripts de Validação

Crie `scripts/validate-plugin.js`:

```javascript
const fs = require('fs');

function validatePlugin(filePath) {
  const content = fs.readFileSync(filePath, 'utf8');
  const plugin = JSON.parse(content);
  
  const errors = [];
  
  // Validar campos obrigatórios
  if (!plugin.name) errors.push('Campo "name" é obrigatório');
  if (!plugin.version) errors.push('Campo "version" é obrigatório');
  if (!plugin.author) errors.push('Campo "author" é obrigatório');
  
  // Validar objetos
  if (plugin.objects) {
    plugin.objects.forEach((obj, i) => {
      if (!obj.id) errors.push(`Objeto ${i}: campo "id" obrigatório`);
      if (!obj.name) errors.push(`Objeto ${i}: campo "name" obrigatório`);
      if (!obj.draw) errors.push(`Objeto ${i}: campo "draw" obrigatório`);
      
      // Validar que a função draw é uma string válida
      try {
        new Function('ctx', 'obj', obj.draw);
      } catch(e) {
        errors.push(`Objeto ${i}: função draw inválida - ${e.message}`);
      }
    });
  }
  
  if (errors.length > 0) {
    console.error('❌ Erros encontrados:');
    errors.forEach(e => console.error(`  - ${e}`));
    return false;
  }
  
  console.log('✅ Plugin válido!');
  return true;
}

const filePath = process.argv[2];
if (!filePath) {
  console.error('Uso: node validate-plugin.js <arquivo.json>');
  process.exit(1);
}

validatePlugin(filePath);
```

---

## Exemplo de Plugin Simples e Funcional

### arquivo: `plugins/mechanics/simple-bounce.plugin.json`

```json
{
  "name": "Simple Bounce",
  "version": "1.0.0",
  "author": "Physics Lab Team",
  "description": "Adiciona uma bola saltitante simples para demonstração",
  "category": "mechanics",
  "objects": [
    {
      "id": "simple_bouncing_ball",
      "name": "Bola Saltitante",
      "icon": "sports_basketball",
      "type": "shape",
      "defaultProps": {
        "w": 25,
        "mass": 1,
        "fill": "#ff6b6b",
        "stroke": "#c44545",
        "bounce": 0.85,
        "vx": 60,
        "vy": 0
      },
      "draw": "function(ctx, obj) {\n  ctx.save();\n  ctx.beginPath();\n  ctx.arc(obj.x, obj.y, obj.w, 0, Math.PI * 2);\n  ctx.fillStyle = obj.fill;\n  ctx.fill();\n  ctx.strokeStyle = obj.stroke;\n  ctx.lineWidth = 2;\n  ctx.stroke();\n  \n  ctx.beginPath();\n  ctx.arc(obj.x - obj.w/3, obj.y - obj.w/3, obj.w/5, 0, Math.PI * 2);\n  ctx.fillStyle = 'rgba(255,255,255,0.8)';\n  ctx.fill();\n  ctx.restore();\n}",
      "update": "function(obj, dt) {\n  obj.vy += 9.8 * dt;\n  obj.x += obj.vx * dt;\n  obj.y += obj.vy * dt;\n  \n  const r = obj.w;\n  if (obj.y + r > 700) {\n    obj.y = 700 - r;\n    obj.vy = -obj.vy * obj.bounce;\n  }\n  if (obj.x + r > 1100) {\n    obj.x = 1100 - r;\n    obj.vx = -obj.vx * obj.bounce;\n  }\n  if (obj.x - r < 0) {\n    obj.x = r;\n    obj.vx = -obj.vx * obj.bounce;\n  }\n}",
      "properties": [
        {"name": "mass", "label": "Massa (kg)", "type": "number", "default": 1, "min": 0.1, "max": 10},
        {"name": "fill", "label": "Cor", "type": "color", "default": "#ff6b6b"},
        {"name": "bounce", "label": "Elasticidade", "type": "range", "default": 0.85, "min": 0, "max": 1, "step": 0.01},
        {"name": "vx", "label": "Velocidade X", "type": "number", "default": 60, "min": -200, "max": 200}
      ]
    }
  ],
  "tools": [
    {
      "id": "add_bouncing_ball",
      "name": "Adicionar Bola",
      "icon": "add_circle",
      "category": "mechanics",
      "action": "function() {\n  Lab.objects.add({\n    type: 'circle',\n    pluginId: 'simple_bouncing_ball',\n    x: Math.random() * 1000 + 50,\n    y: 100,\n    w: 25,\n    mass: 1,\n    fill: `hsl(${Math.random() * 360}, 70%, 60%)`,\n    bounce: 0.85,\n    vx: (Math.random() - 0.5) * 100,\n    vy: 0\n  });\n}"
    }
  ]
}
```

### Como usar este plugin:

1. Salve o arquivo acima como `simple-bounce.plugin.json`
2. No Physics Lab, abra o Gerenciador de Plugins
3. Clique em "Upload Plugin (.json)" e selecione o arquivo
4. O novo objeto "Bola Saltitante" aparecerá na categoria Mecânica
5. Clique no ícone para criar bolas saltitantes!

---

## Boas Práticas

### 1. Performance
```javascript
// ✅ Bom: Cache de cálculos caros
let cachedValue = null;
update: function(obj, dt) {
  if (cachedValue === null || performance.now() - lastUpdate > 100) {
    cachedValue = expensiveCalculation(obj);
    lastUpdate = performance.now();
  }
  useValue(cachedValue);
}

// ❌ Ruim: Recalcular tudo a cada frame
update: function(obj, dt) {
  const value = expensiveCalculation(obj); // Executa 60x/segundo
}
```

### 2. Memória
```javascript
// ✅ Bom: Limpar recursos
cleanup: function() {
  this.timer && clearInterval(this.timer);
  this.elements && this.elements.forEach(el => el.remove());
}

// ❌ Ruim: Deixar recursos órfãos
```

### 3. Compatibilidade
```javascript
// ✅ Bom: Verificar existência de funções
if (Lab.fluids && typeof Lab.fluids.createFluidWall === 'function') {
  Lab.fluids.createFluidWall(x1, y1, x2, y2);
}

// ❌ Ruim: Assumir que funções existem
```

### 4. Documentação
```javascript
// ✅ Bom: Comentários no código
/**
 * Atualiza a física da bola
 * @param {Object} obj - Objeto da bola
 * @param {number} dt - Delta time em segundos
 */
update: function(obj, dt) { ... }
```

### 5. Testes
Sempre teste seu plugin em diferentes cenários:
- Com simulação ativa e inativa
- Com diferentes valores de gravidade
- Com outros objetos na tela
- Com zoom e pan aplicados

---
