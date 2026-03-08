# 🎬 Motion Design & Animações Web — Guia Completo de Boas Práticas

> **Versão:** 1.0.0 · **Licença:** MIT · **Mantido por:** Comunidade Open Source  
> Guia gratuito e aberto para todos. Contribuições são bem-vindas via Pull Request.

---

## 📋 Índice

1. [Filosofia & Princípios](#1-filosofia--princípios)
2. [Fundamentos CSS](#2-fundamentos-css)
3. [JavaScript Puro para Animações](#3-javascript-puro-para-animações)
4. [GSAP — GreenSock Animation Platform](#4-gsap--greensock-animation-platform)
5. [Framer Motion — React](#5-framer-motion--react)
6. [Three.js & React Three Fiber — 3D na Web](#6-threejs--react-three-fiber--3d-na-web)
7. [Lottie — After Effects na Web](#7-lottie--after-effects-na-web)
8. [Rive — Animações Interativas](#8-rive--animações-interativas)
9. [Scroll Animations](#9-scroll-animations)
10. [Microinterações & UX Motion](#10-microinterações--ux-motion)
11. [Performance & Otimização](#11-performance--otimização)
12. [Acessibilidade em Animações](#12-acessibilidade-em-animações)
13. [Design System com Motion Tokens](#13-design-system-com-motion-tokens)
14. [Workflow com Claude + VSCode](#14-workflow-com-claude--vscode)
15. [Componentes Prontos para Copiar](#15-componentes-prontos-para-copiar)
16. [Ferramentas & Recursos](#16-ferramentas--recursos)
17. [Referências & Inspiração](#17-referências--inspiração)

---

## 1. Filosofia & Princípios

### Os 12 Princípios do Disney (aplicados à web)

Criados por Ollie Johnston e Frank Thomas, esses princípios são a base de qualquer animação profissional:

| Princípio | Descrição Web | Exemplo CSS/JS |
|-----------|--------------|----------------|
| **Squash & Stretch** | Elementos se deformam ao mover | `scaleX(1.2) scaleY(0.8)` |
| **Anticipation** | Prepare o usuário para a ação | Botão recua antes de avançar |
| **Staging** | Foco no que importa | Escurecer fundo ao abrir modal |
| **Straight Ahead / Pose to Pose** | Keyframes intermediários | `@keyframes` com múltiplos stops |
| **Follow Through** | Movimento continua após parar | `cubic-bezier` com overshoot |
| **Overlapping Action** | Partes se movem em tempos diferentes | `animation-delay` em filhos |
| **Slow In & Slow Out** | Ease in/out naturais | `ease-in-out`, `cubic-bezier` |
| **Arcs** | Movimento em curva, não linear | `motion-path`, SVG paths |
| **Secondary Action** | Movimento de suporte | Sombra que acompanha hover |
| **Timing** | Velocidade comunica peso | Duração curta = leve, longa = pesado |
| **Exaggeration** | Intensifique para comunicar | `scale(1.05)` em hover de CTA |
| **Solid Drawing** | Consistência visual | Design system com tokens |

### Regras de Ouro do Motion Design Web

```
✅ FAÇA                              ❌ NÃO FAÇA
──────────────────────────────────────────────────────
Anime propriedades GPU-friendly      Animar width/height diretamente
(transform, opacity)                 (causa reflow)

Use duration proporcional à          Usar a mesma duração para tudo
distância e importância

Respeite prefers-reduced-motion      Ignorar usuários com sensibilidade

Anime com intenção e propósito       Animar por animar (decoração vazia)

Mantenha < 60fps como meta          Animar elementos fora do viewport

Teste em dispositivos lentos         Assumir que todos têm hardware top
```

### A Hierarquia de Motion

```
1. 🎯 FUNCIONAL    → Feedback de ação (click, hover, load)
2. 🧭 NAVEGACIONAL → Transições entre estados/páginas
3. 🎨 EXPRESSIVO   → Personalidade da marca
4. ✨ DECORATIVO   → Apenas se não prejudicar experiência
```

---

## 2. Fundamentos CSS

### Propriedades Essenciais

```css
/* ─── TRANSFORM (GPU-accelerated) ─── */
.element {
  transform: translateX(100px);     /* Move horizontal */
  transform: translateY(-50%);      /* Move vertical */
  transform: scale(1.05);           /* Escala uniforme */
  transform: rotate(45deg);         /* Rotação */
  transform: skewX(10deg);          /* Distorção */
  
  /* Combinando (ordem importa!) */
  transform: translateX(100px) rotate(45deg) scale(1.2);
  
  /* Origin do transform */
  transform-origin: center center;
  transform-origin: top left;
  transform-origin: 50% 100%;
}

/* ─── TRANSITION ─── */
.button {
  transition: transform 0.2s ease-out,
              opacity 0.3s ease,
              box-shadow 0.2s ease;
  
  /* Atalho: property duration timing-function delay */
  transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.button:hover {
  transform: translateY(-3px) scale(1.02);
  box-shadow: 0 20px 40px rgba(0,0,0,0.2);
}

/* ─── ANIMATION ─── */
.spinner {
  animation: spin 1s linear infinite;
  animation: pulse 2s ease-in-out infinite alternate;
  
  /* Múltiplas animações */
  animation: 
    fadeIn 0.5s ease forwards,
    float 3s ease-in-out 0.5s infinite;
}
```

### Curvas de Bezier — O Vocabulário do Motion

```css
/* ─── PRÉ-DEFINIDAS ─── */
transition-timing-function: linear;        /* Robótico, evitar */
transition-timing-function: ease;          /* Padrão do browser */
transition-timing-function: ease-in;       /* Acelera (saída de cena) */
transition-timing-function: ease-out;      /* Desacelera (entrada de cena) */
transition-timing-function: ease-in-out;   /* Suave dos dois lados */

/* ─── CUSTOMIZADAS (recomendadas) ─── */

/* Natural/Orgânico */
--ease-natural: cubic-bezier(0.25, 0.46, 0.45, 0.94);

/* Overshoot (spring-like) */
--ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);

/* Entrada dramática */
--ease-in-expo: cubic-bezier(0.95, 0.05, 0.795, 0.035);

/* Saída suave */
--ease-out-expo: cubic-bezier(0.19, 1, 0.22, 1);

/* Bounce sutil */
--ease-bounce: cubic-bezier(0.68, -0.55, 0.265, 1.55);

/* Material Design */
--ease-material-standard: cubic-bezier(0.4, 0, 0.2, 1);
--ease-material-enter: cubic-bezier(0, 0, 0.2, 1);
--ease-material-exit: cubic-bezier(0.4, 0, 1, 1);
```

### Keyframes — Padrões Reutilizáveis

```css
/* ─── ENTRADA ─── */
@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}

@keyframes slideInUp {
  from { opacity: 0; transform: translateY(40px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes slideInLeft {
  from { opacity: 0; transform: translateX(-40px); }
  to   { opacity: 1; transform: translateX(0); }
}

@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.8); }
  to   { opacity: 1; transform: scale(1); }
}

@keyframes revealFromMask {
  from { clip-path: inset(0 100% 0 0); }
  to   { clip-path: inset(0 0% 0 0); }
}

/* ─── LOOPS ─── */
@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50%       { transform: translateY(-20px); }
}

@keyframes pulse {
  0%, 100% { transform: scale(1); opacity: 1; }
  50%       { transform: scale(1.05); opacity: 0.8; }
}

@keyframes shimmer {
  from { background-position: -200% center; }
  to   { background-position: 200% center; }
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

/* ─── ATENÇÃO ─── */
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  20%       { transform: translateX(-8px); }
  40%       { transform: translateX(8px); }
  60%       { transform: translateX(-5px); }
  80%       { transform: translateX(5px); }
}

@keyframes bounce {
  0%, 20%, 53%, 100% { transform: translateY(0); }
  40%, 43%           { transform: translateY(-20px); }
  70%                { transform: translateY(-10px); }
  90%                { transform: translateY(-4px); }
}

/* ─── TEXTO ─── */
@keyframes typewriter {
  from { width: 0; }
  to   { width: 100%; }
}

@keyframes blink {
  0%, 100% { border-color: transparent; }
  50%       { border-color: currentColor; }
}
```

### Stagger — Animações em Cascata com CSS

```css
/* Técnica: animation-delay progressivo nos filhos */
.list-item:nth-child(1) { animation-delay: 0ms; }
.list-item:nth-child(2) { animation-delay: 80ms; }
.list-item:nth-child(3) { animation-delay: 160ms; }
.list-item:nth-child(4) { animation-delay: 240ms; }
.list-item:nth-child(5) { animation-delay: 320ms; }

/* Com CSS Custom Properties (mais elegante) */
.list-item {
  --stagger-delay: 80ms;
  animation: slideInUp 0.5s var(--ease-out-expo) both;
  animation-delay: calc(var(--index) * var(--stagger-delay));
}

/* Aplicar via JavaScript */
document.querySelectorAll('.list-item').forEach((el, i) => {
  el.style.setProperty('--index', i);
});
```

### CSS Variables como Motion Tokens

```css
:root {
  /* ─── DURATION ─── */
  --duration-instant:   50ms;
  --duration-fast:      100ms;
  --duration-normal:    200ms;
  --duration-slow:      350ms;
  --duration-slower:    500ms;
  --duration-slowest:   800ms;

  /* ─── EASING ─── */
  --ease-linear:        linear;
  --ease-in:            cubic-bezier(0.4, 0, 1, 1);
  --ease-out:           cubic-bezier(0, 0, 0.2, 1);
  --ease-in-out:        cubic-bezier(0.4, 0, 0.2, 1);
  --ease-spring:        cubic-bezier(0.34, 1.56, 0.64, 1);
  --ease-bounce:        cubic-bezier(0.68, -0.55, 0.265, 1.55);

  /* ─── DELAY ─── */
  --delay-stagger-sm:   60ms;
  --delay-stagger-md:   100ms;
  --delay-stagger-lg:   150ms;
}
```

### View Transitions API (nativa, moderna)

```css
/* Transição de página nativa do browser */
@view-transition {
  navigation: auto;
}

/* Customizar o elemento específico */
.hero-image {
  view-transition-name: hero;
}

/* Animar a transição */
::view-transition-old(hero) {
  animation: 300ms ease-out both fade-out;
}
::view-transition-new(hero) {
  animation: 300ms ease-in both fade-in;
}

/* Via JavaScript */
document.startViewTransition(() => {
  updateDOM(); /* sua mudança de estado aqui */
});
```

---

## 3. JavaScript Puro para Animações

### requestAnimationFrame — A Base

```javascript
// Loop de animação otimizado para o browser
class Animator {
  constructor() {
    this.rafId = null;
    this.startTime = null;
  }

  animate(duration, onUpdate, onComplete) {
    this.startTime = performance.now();

    const frame = (currentTime) => {
      const elapsed = currentTime - this.startTime;
      const progress = Math.min(elapsed / duration, 1);
      const eased = this.easeOutExpo(progress);

      onUpdate(eased);

      if (progress < 1) {
        this.rafId = requestAnimationFrame(frame);
      } else {
        onComplete?.();
      }
    };

    this.rafId = requestAnimationFrame(frame);
  }

  stop() {
    if (this.rafId) cancelAnimationFrame(this.rafId);
  }

  // Funções de easing
  easeOutExpo(t) {
    return t === 1 ? 1 : 1 - Math.pow(2, -10 * t);
  }

  easeInOutCubic(t) {
    return t < 0.5 ? 4 * t * t * t : 1 - Math.pow(-2 * t + 2, 3) / 2;
  }

  easeSpring(t) {
    const c4 = (2 * Math.PI) / 3;
    return t === 0 ? 0 : t === 1 ? 1
      : Math.pow(2, -10 * t) * Math.sin((t * 10 - 0.75) * c4) + 1;
  }
}

// Uso
const anim = new Animator();
const el = document.querySelector('.box');

anim.animate(
  600,                              // duração em ms
  (progress) => {
    el.style.transform = `translateY(${(1 - progress) * 100}px)`;
    el.style.opacity = progress;
  },
  () => console.log('Animação concluída!')
);
```

### Intersection Observer — Reveal on Scroll

```javascript
// Padrão production-ready para revelar elementos no scroll
class ScrollReveal {
  constructor(options = {}) {
    this.options = {
      threshold: 0.1,
      rootMargin: '0px 0px -50px 0px',
      once: true,
      ...options
    };

    this.observer = new IntersectionObserver(
      this.handleIntersection.bind(this),
      this.options
    );
  }

  handleIntersection(entries) {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-visible');
        if (this.options.once) {
          this.observer.unobserve(entry.target);
        }
      } else if (!this.options.once) {
        entry.target.classList.remove('is-visible');
      }
    });
  }

  observe(selector) {
    document.querySelectorAll(selector).forEach(el => {
      this.observer.observe(el);
    });
    return this;
  }
}

// CSS complementar
const styles = `
  [data-reveal] {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.6s var(--ease-out), 
                transform 0.6s var(--ease-out);
  }

  [data-reveal].is-visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* Variações */
  [data-reveal="left"]  { transform: translateX(-40px); }
  [data-reveal="right"] { transform: translateX(40px); }
  [data-reveal="scale"] { transform: scale(0.9); }
`;

// Uso
const reveal = new ScrollReveal({ threshold: 0.15 });
reveal.observe('[data-reveal]');
```

### Parallax Scroll

```javascript
class Parallax {
  constructor() {
    this.elements = [];
    this.ticking = false;
    window.addEventListener('scroll', () => this.onScroll());
  }

  add(selector, speed = 0.5) {
    document.querySelectorAll(selector).forEach(el => {
      this.elements.push({ el, speed });
    });
    return this;
  }

  onScroll() {
    if (!this.ticking) {
      requestAnimationFrame(() => {
        this.update();
        this.ticking = false;
      });
      this.ticking = true;
    }
  }

  update() {
    const scrollY = window.scrollY;
    this.elements.forEach(({ el, speed }) => {
      const rect = el.getBoundingClientRect();
      const centerY = rect.top + rect.height / 2;
      const offset = (centerY - window.innerHeight / 2) * speed;
      el.style.transform = `translateY(${offset}px)`;
    });
  }
}

// Uso
new Parallax()
  .add('.hero-bg', 0.3)
  .add('.hero-text', -0.2)
  .add('.floating-element', 0.6);
```

### Magnetic Cursor Effect

```javascript
class MagneticElement {
  constructor(selector, strength = 0.3) {
    this.elements = document.querySelectorAll(selector);
    this.strength = strength;
    this.init();
  }

  init() {
    this.elements.forEach(el => {
      el.addEventListener('mousemove', (e) => this.onMouseMove(e, el));
      el.addEventListener('mouseleave', (e) => this.onMouseLeave(el));
    });
  }

  onMouseMove(e, el) {
    const rect = el.getBoundingClientRect();
    const centerX = rect.left + rect.width / 2;
    const centerY = rect.top + rect.height / 2;
    const deltaX = (e.clientX - centerX) * this.strength;
    const deltaY = (e.clientY - centerY) * this.strength;

    el.style.transition = 'transform 0.2s ease';
    el.style.transform = `translate(${deltaX}px, ${deltaY}px)`;
  }

  onMouseLeave(el) {
    el.style.transition = 'transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1)';
    el.style.transform = 'translate(0, 0)';
  }
}

// Uso
new MagneticElement('.btn-magnetic', 0.4);
```

---

## 4. GSAP — GreenSock Animation Platform

> A biblioteca mais poderosa e precisa para animações web. Padrão da indústria.

### Instalação

```bash
# npm
npm install gsap

# CDN (HTML puro)
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
```

### Conceitos Fundamentais

```javascript
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import { TextPlugin } from 'gsap/TextPlugin';

gsap.registerPlugin(ScrollTrigger, TextPlugin);

// ─── gsap.to() — Do estado atual ATÉ os valores ───
gsap.to('.box', {
  x: 200,           // translateX (shorthand GSAP)
  y: -100,          // translateY
  rotation: 360,    // rotate
  scale: 1.5,
  opacity: 0,
  duration: 1.2,
  ease: 'power2.out',
  delay: 0.3,
});

// ─── gsap.from() — DOS valores ATÉ o estado atual ───
gsap.from('.hero-title', {
  y: 60,
  opacity: 0,
  duration: 0.8,
  ease: 'expo.out',
});

// ─── gsap.fromTo() — Controle total ───
gsap.fromTo('.card',
  { x: -100, opacity: 0, scale: 0.8 },   // from
  { x: 0, opacity: 1, scale: 1,          // to
    duration: 0.6, ease: 'back.out(1.7)' }
);

// ─── gsap.set() — Definir sem animar ───
gsap.set('.element', { opacity: 0, y: 30 });
```

### Timelines — Orquestração

```javascript
// Timeline é o superpoder do GSAP
const tl = gsap.timeline({
  defaults: { duration: 0.6, ease: 'expo.out' },
  onComplete: () => console.log('Animação da hero concluída!'),
});

// Sequencial
tl.from('.nav', { y: -60, opacity: 0 })
  .from('.hero-tag', { opacity: 0, x: -20 })
  .from('.hero-title .line', { y: 100, opacity: 0, stagger: 0.1 })
  .from('.hero-description', { opacity: 0, y: 20 }, '-=0.3') // overlap 0.3s
  .from('.hero-cta', { opacity: 0, y: 20, scale: 0.9 }, '-=0.2')
  .from('.hero-image', { opacity: 0, scale: 1.1 }, '<0.2'); // "<" = junto com anterior

// Controles da Timeline
tl.pause();
tl.play();
tl.reverse();
tl.seek(1.5);     // ir para o segundo 1.5
tl.timeScale(2);  // dobrar a velocidade
```

### ScrollTrigger — Animações no Scroll

```javascript
// ─── Básico ───
gsap.from('.section-title', {
  scrollTrigger: {
    trigger: '.section-title',
    start: 'top 80%',   // quando o topo do elemento chega a 80% da viewport
    end: 'top 20%',
    toggleActions: 'play none none reverse',
    // onEnter onLeave onEnterBack onLeaveBack
  },
  y: 60,
  opacity: 0,
  duration: 0.8,
});

// ─── Scrub — Animação ligada ao scroll ───
gsap.to('.parallax-element', {
  scrollTrigger: {
    trigger: '.section',
    start: 'top bottom',
    end: 'bottom top',
    scrub: 1,           // 1 = lag suave de 1 segundo
  },
  y: -150,
  ease: 'none',
});

// ─── Pin — Fixar elemento durante scroll ───
gsap.timeline({
  scrollTrigger: {
    trigger: '.sticky-section',
    start: 'top top',
    end: '+=200%',
    pin: true,
    scrub: true,
  }
})
.from('.slide-1', { opacity: 0 })
.to('.slide-1', { opacity: 0 })
.from('.slide-2', { opacity: 0 });

// ─── Stagger com ScrollTrigger ───
gsap.from('.card', {
  scrollTrigger: {
    trigger: '.cards-grid',
    start: 'top 75%',
  },
  y: 50,
  opacity: 0,
  duration: 0.6,
  stagger: {
    amount: 0.8,        // total de tempo distribuído
    from: 'start',      // 'start', 'end', 'center', 'random'
    grid: 'auto',       // detecta grid automaticamente
  },
});

// ─── Horizontal Scroll ───
const sections = gsap.utils.toArray('.h-section');

gsap.to(sections, {
  xPercent: -100 * (sections.length - 1),
  ease: 'none',
  scrollTrigger: {
    trigger: '.horizontal-container',
    pin: true,
    scrub: 1,
    snap: 1 / (sections.length - 1),
    end: () => `+=${document.querySelector('.horizontal-container').offsetWidth}`,
  }
});
```

### Easings do GSAP

```javascript
// Família Power (suaves)
ease: 'power1.in'     // ease-in suave
ease: 'power2.out'    // ease-out médio (muito usado)
ease: 'power3.inOut'  // ease-in-out forte
ease: 'power4.out'    // ease-out muito forte

// Bounce & Elastic
ease: 'bounce.out'
ease: 'elastic.out(1, 0.3)'   // (amplitude, period)
ease: 'back.out(1.7)'         // overshoot

// Expo & Circ (dramáticos)
ease: 'expo.out'     // desacelera drasticamente
ease: 'circ.out'     // arco circular

// Stepped (digital)
ease: 'steps(5)'     // discreto, retro

// Custom
ease: 'M0,0 C0.126,0.382 0.282,0.674 0.44,0.822 C0.632,1.002 0.818,1.001 1,1'
```

### GSAP + Texto

```javascript
import { SplitText } from 'gsap/SplitText';  // Club GSAP
// Alternativa free: splitting.js

gsap.registerPlugin(SplitText);

const split = new SplitText('.headline', { type: 'chars,words,lines' });

gsap.from(split.chars, {
  opacity: 0,
  y: 80,
  rotationX: -90,
  stagger: 0.02,
  duration: 0.8,
  ease: 'back.out',
  scrollTrigger: {
    trigger: '.headline',
    start: 'top 80%',
  }
});

// Typewriter effect (sem plugin)
gsap.to('.typewriter', {
  text: {
    value: 'Olá, mundo! ✨',
    delimiter: '',  // caractere por caractere
  },
  duration: 2,
  ease: 'none',
});
```

---

## 5. Framer Motion — React

### Instalação

```bash
npm install framer-motion
```

### Componentes Básicos

```jsx
import { motion, AnimatePresence } from 'framer-motion';

// ─── motion.div — Wrapper mágico ───
function FadeIn({ children }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 30 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -20 }}
      transition={{ duration: 0.5, ease: [0.22, 1, 0.36, 1] }}
    >
      {children}
    </motion.div>
  );
}

// ─── Hover & Tap ───
function AnimatedButton({ children, onClick }) {
  return (
    <motion.button
      whileHover={{ scale: 1.05, y: -3 }}
      whileTap={{ scale: 0.97 }}
      transition={{ type: 'spring', stiffness: 400, damping: 25 }}
      onClick={onClick}
    >
      {children}
    </motion.button>
  );
}

// ─── AnimatePresence — Mount/Unmount ───
function Modal({ isOpen, onClose, children }) {
  return (
    <AnimatePresence>
      {isOpen && (
        <>
          <motion.div
            className="backdrop"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            onClick={onClose}
          />
          <motion.div
            className="modal"
            initial={{ opacity: 0, scale: 0.9, y: 30 }}
            animate={{ opacity: 1, scale: 1, y: 0 }}
            exit={{ opacity: 0, scale: 0.95, y: 20 }}
            transition={{ type: 'spring', stiffness: 500, damping: 35 }}
          >
            {children}
          </motion.div>
        </>
      )}
    </AnimatePresence>
  );
}
```

### Variants — Sistema de Animação

```jsx
// Variants são como um design system para animações
const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1,   // delay entre filhos
      delayChildren: 0.2,     // delay inicial
    }
  }
};

const itemVariants = {
  hidden: { opacity: 0, y: 30, scale: 0.95 },
  visible: {
    opacity: 1, y: 0, scale: 1,
    transition: {
      type: 'spring',
      stiffness: 300,
      damping: 24,
    }
  }
};

function AnimatedList({ items }) {
  return (
    <motion.ul
      variants={containerVariants}
      initial="hidden"
      animate="visible"
    >
      {items.map(item => (
        <motion.li
          key={item.id}
          variants={itemVariants}  // herda o estado do pai!
          whileHover={{ x: 8 }}
        >
          {item.label}
        </motion.li>
      ))}
    </motion.ul>
  );
}
```

### useScroll & useTransform

```jsx
import { useScroll, useTransform, useSpring, motion } from 'framer-motion';

function ParallaxHero() {
  const { scrollYProgress } = useScroll();

  // Mapear scroll para valores de animação
  const y = useTransform(scrollYProgress, [0, 0.5], ['0%', '30%']);
  const opacity = useTransform(scrollYProgress, [0, 0.3], [1, 0]);
  const scale = useTransform(scrollYProgress, [0, 0.3], [1, 1.1]);

  // Spring para suavizar
  const springY = useSpring(y, { stiffness: 100, damping: 30 });

  return (
    <section style={{ height: '200vh' }}>
      <div style={{ position: 'sticky', top: 0, height: '100vh', overflow: 'hidden' }}>
        <motion.div
          style={{ y: springY, scale, opacity }}
          className="hero-bg"
        />
        <motion.h1 style={{ opacity }}>
          Título da Hero
        </motion.h1>
      </div>
    </section>
  );
}

// Scroll por elemento específico
function CardReveal() {
  const ref = useRef(null);
  const { scrollYProgress } = useScroll({
    target: ref,
    offset: ['start end', 'end start'],
  });

  const opacity = useTransform(scrollYProgress, [0, 0.3], [0, 1]);
  const y = useTransform(scrollYProgress, [0, 0.3], [50, 0]);

  return (
    <motion.div ref={ref} style={{ opacity, y }}>
      Conteúdo revelado no scroll
    </motion.div>
  );
}
```

### Layout Animations

```jsx
import { motion, LayoutGroup } from 'framer-motion';

// Framer Motion detecta mudanças de layout e anima automaticamente!
function FilteredGrid({ items }) {
  const [filter, setFilter] = useState('all');
  const filtered = items.filter(i => filter === 'all' || i.category === filter);

  return (
    <LayoutGroup>
      <div className="grid">
        <AnimatePresence>
          {filtered.map(item => (
            <motion.div
              key={item.id}
              layout                    // ← magia acontece aqui
              initial={{ opacity: 0, scale: 0.8 }}
              animate={{ opacity: 1, scale: 1 }}
              exit={{ opacity: 0, scale: 0.8 }}
              transition={{ type: 'spring', stiffness: 350, damping: 30 }}
            >
              <Card item={item} />
            </motion.div>
          ))}
        </AnimatePresence>
      </div>
    </LayoutGroup>
  );
}
```

### Gestos & Drag

```jsx
function DraggableCard() {
  return (
    <motion.div
      drag
      dragConstraints={{ top: -100, bottom: 100, left: -100, right: 100 }}
      dragElastic={0.1}
      dragTransition={{ bounceStiffness: 600, bounceDamping: 20 }}
      whileDrag={{ scale: 1.05, boxShadow: '0 25px 50px rgba(0,0,0,0.3)' }}
      className="draggable-card"
    >
      Arraste-me!
    </motion.div>
  );
}

// Slider com gesture
function Carousel({ slides }) {
  const [current, setCurrent] = useState(0);

  return (
    <motion.div
      drag="x"
      dragConstraints={{ left: 0, right: 0 }}
      onDragEnd={(e, { offset, velocity }) => {
        const swipe = Math.abs(offset.x) * velocity.x;
        if (swipe < -10000) setCurrent(prev => Math.min(prev + 1, slides.length - 1));
        if (swipe > 10000)  setCurrent(prev => Math.max(prev - 1, 0));
      }}
    >
      <motion.div
        animate={{ x: `-${current * 100}%` }}
        transition={{ type: 'spring', stiffness: 300, damping: 30 }}
        className="slides-track"
      >
        {slides.map((slide, i) => <div key={i} className="slide">{slide}</div>)}
      </motion.div>
    </motion.div>
  );
}
```

---

## 6. Three.js & React Three Fiber — 3D na Web

### Setup React Three Fiber

```bash
npm install three @react-three/fiber @react-three/drei
```

```jsx
import { Canvas } from '@react-three/fiber';
import { OrbitControls, Float, MeshDistortMaterial, Environment } from '@react-three/drei';
import { useFrame } from '@react-three/fiber';
import { useRef } from 'react';

// ─── Cena básica ───
function Scene() {
  return (
    <Canvas
      camera={{ position: [0, 0, 5], fov: 75 }}
      dpr={[1, 2]}           // pixel ratio responsivo
      gl={{ antialias: true }}
    >
      <ambientLight intensity={0.5} />
      <pointLight position={[10, 10, 10]} intensity={1} />
      <Environment preset="city" />

      <Float
        speed={2}
        rotationIntensity={0.5}
        floatIntensity={0.8}
      >
        <AnimatedSphere />
      </Float>

      <OrbitControls enableZoom={false} />
    </Canvas>
  );
}

// ─── Objeto animado ───
function AnimatedSphere() {
  const meshRef = useRef();

  useFrame((state, delta) => {
    meshRef.current.rotation.x += delta * 0.3;
    meshRef.current.rotation.y += delta * 0.5;

    // Animação baseada no tempo
    meshRef.current.scale.setScalar(
      1 + Math.sin(state.clock.elapsedTime * 2) * 0.05
    );
  });

  return (
    <mesh ref={meshRef}>
      <icosahedronGeometry args={[1.5, 4]} />
      <MeshDistortMaterial
        color="#6366f1"
        distort={0.4}
        speed={2}
        roughness={0}
        metalness={0.5}
      />
    </mesh>
  );
}
```

### Shaders Personalizados

```jsx
import { shaderMaterial } from '@react-three/drei';
import { extend } from '@react-three/fiber';
import * as THREE from 'three';

// Criar material com shader customizado
const WaveMaterial = shaderMaterial(
  { time: 0, color: new THREE.Color(0.2, 0.5, 1.0) },

  // Vertex Shader
  `
    varying vec2 vUv;
    uniform float time;

    void main() {
      vUv = uv;
      vec3 pos = position;
      pos.z += sin(pos.x * 3.0 + time) * 0.2;
      pos.z += sin(pos.y * 2.0 + time * 0.8) * 0.1;
      gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.0);
    }
  `,

  // Fragment Shader
  `
    varying vec2 vUv;
    uniform vec3 color;
    uniform float time;

    void main() {
      vec3 finalColor = color;
      finalColor += 0.2 * sin(vUv.x * 10.0 + time);
      gl_FragColor = vec4(finalColor, 1.0);
    }
  `
);

extend({ WaveMaterial });

function WavePlane() {
  const ref = useRef();
  useFrame(({ clock }) => {
    ref.current.time = clock.elapsedTime;
  });

  return (
    <mesh rotation={[-Math.PI / 2, 0, 0]} position={[0, -1, 0]}>
      <planeGeometry args={[10, 10, 50, 50]} />
      <waveMaterial ref={ref} />
    </mesh>
  );
}
```

---

## 7. Lottie — After Effects na Web

```bash
npm install lottie-web
# ou para React
npm install @lottiefiles/react-lottie-player
```

```jsx
// React Component
import { Player, Controls } from '@lottiefiles/react-lottie-player';

function LottieAnimation({ src, autoplay = true, loop = true }) {
  const playerRef = useRef(null);

  return (
    <Player
      ref={playerRef}
      src={src}              // URL ou objeto JSON
      autoplay={autoplay}
      loop={loop}
      style={{ height: '300px', width: '300px' }}
      onEvent={(event) => {
        if (event === 'complete') {
          console.log('Animação completa!');
        }
      }}
    />
  );
}

// Controle programático
function InteractiveLottie({ animationData }) {
  const playerRef = useRef(null);

  const handlePlay = () => playerRef.current.play();
  const handlePause = () => playerRef.current.pause();
  const handleStop = () => playerRef.current.stop();

  // Navegar para frame específico
  const goToFrame = (frame) => playerRef.current.goToAndStop(frame, true);

  return (
    <div>
      <Player ref={playerRef} src={animationData} />
      <button onClick={handlePlay}>▶ Play</button>
      <button onClick={handlePause}>⏸ Pause</button>
    </div>
  );
}
```

**Fontes de Animações Lottie Gratuitas:**
- [LottieFiles.com](https://lottiefiles.com) — Maior biblioteca gratuita
- [Icons8 Lottie](https://icons8.com/animated-icons) — Ícones animados
- [Iconscout](https://iconscout.com/lottie-animations) — Alta qualidade

---

## 8. Rive — Animações Interativas

```bash
npm install @rive-app/react-canvas
```

```jsx
import { useRive, useStateMachineInput } from '@rive-app/react-canvas';

function RiveButton() {
  const { rive, RiveComponent } = useRive({
    src: '/button.riv',
    stateMachines: 'button-state',
    autoplay: true,
  });

  // Controlar via State Machine
  const isHovered = useStateMachineInput(rive, 'button-state', 'isHovered');
  const isPressed = useStateMachineInput(rive, 'button-state', 'isPressed');

  return (
    <RiveComponent
      style={{ width: 200, height: 60 }}
      onMouseEnter={() => isHovered && (isHovered.value = true)}
      onMouseLeave={() => isHovered && (isHovered.value = false)}
      onMouseDown={() => isPressed && (isPressed.value = true)}
      onMouseUp={() => isPressed && (isPressed.value = false)}
    />
  );
}
```

---

## 9. Scroll Animations

### CSS Scroll-Driven Animations (nativo, sem JS!)

```css
/* Animação baseada no scroll — suporte crescente nos browsers */
@keyframes reveal {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.scroll-reveal {
  animation: reveal linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 50%;
}

/* Progress bar de leitura */
.reading-progress {
  position: fixed;
  top: 0;
  left: 0;
  height: 3px;
  background: linear-gradient(to right, #6366f1, #8b5cf6);
  animation: progress-bar linear;
  animation-timeline: scroll();
  transform-origin: left;
}

@keyframes progress-bar {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}
```

### GSAP ScrollTrigger — Patterns Avançados

```javascript
// ─── Revelar texto linha por linha ───
function animateTextLines() {
  const texts = document.querySelectorAll('[data-text-reveal]');

  texts.forEach(text => {
    // Quebrar em linhas (use SplitText ou splitting.js)
    const lines = text.querySelectorAll('.line');

    gsap.from(lines, {
      scrollTrigger: {
        trigger: text,
        start: 'top 80%',
      },
      y: '110%',
      opacity: 0,
      duration: 0.7,
      stagger: 0.1,
      ease: 'power3.out',
    });
  });
}

// ─── Counter animado no scroll ───
function animateCounters() {
  document.querySelectorAll('[data-count]').forEach(el => {
    const target = parseInt(el.dataset.count);
    const obj = { value: 0 };

    gsap.to(obj, {
      value: target,
      duration: 2,
      ease: 'power2.out',
      snap: { value: 1 },
      scrollTrigger: { trigger: el, start: 'top 80%' },
      onUpdate: () => {
        el.textContent = obj.value.toLocaleString('pt-BR');
      }
    });
  });
}

// ─── Morphing de SVG ───
gsap.to('#blob', {
  scrollTrigger: {
    trigger: '#morphSection',
    scrub: 2,
  },
  attr: {
    d: 'M421.5,289.5Q365,329,339.5,369.5Q314,410,258,422Q202,434,168.5,391Q135,348,87.5,322.5Q40,297,45,237Q50,177,86,132Q122,87,181,73Q240,59,299.5,74.5Q359,90,406,133Q453,176,421.5,289.5Z'
  }
});
```

---

## 10. Microinterações & UX Motion

### Padrões Essenciais

```css
/* ─── Botão com feedback ───  */
.btn {
  position: relative;
  overflow: hidden;
  transition: transform 0.15s ease, box-shadow 0.2s ease;
}

/* Ripple effect */
.btn::after {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle, rgba(255,255,255,0.3) 0%, transparent 70%);
  transform: scale(0);
  opacity: 0;
  transition: transform 0.4s ease, opacity 0.4s ease;
}

.btn:active::after {
  transform: scale(2);
  opacity: 1;
  transition: 0s;
}

.btn:hover { transform: translateY(-2px); box-shadow: 0 10px 30px rgba(0,0,0,0.2); }
.btn:active { transform: translateY(0px); }

/* ─── Input com label flutuante ─── */
.input-wrapper {
  position: relative;
}

.input-label {
  position: absolute;
  left: 16px;
  top: 50%;
  transform: translateY(-50%);
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  pointer-events: none;
  color: #9ca3af;
}

.input:focus ~ .input-label,
.input:not(:placeholder-shown) ~ .input-label {
  top: 0;
  transform: translateY(-50%) scale(0.85);
  color: #6366f1;
  background: white;
  padding: 0 4px;
}

/* ─── Toggle Switch ─── */
.toggle {
  width: 52px; height: 28px;
  border-radius: 14px;
  background: #e5e7eb;
  transition: background 0.2s ease;
  cursor: pointer;
  position: relative;
}

.toggle::after {
  content: '';
  position: absolute;
  width: 20px; height: 20px;
  border-radius: 50%;
  background: white;
  top: 4px; left: 4px;
  transition: transform 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
}

.toggle.active { background: #6366f1; }
.toggle.active::after { transform: translateX(24px); }

/* ─── Skeleton Loading ─── */
.skeleton {
  background: linear-gradient(
    90deg,
    #f0f0f0 25%,
    #e0e0e0 50%,
    #f0f0f0 75%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite;
  border-radius: 8px;
}

@keyframes shimmer {
  from { background-position: 200% center; }
  to   { background-position: -200% center; }
}
```

### Feedback Visual de Estados

```javascript
// Sistema de notificações animadas
class NotificationSystem {
  constructor() {
    this.container = this.createContainer();
    document.body.appendChild(this.container);
  }

  createContainer() {
    const el = document.createElement('div');
    el.style.cssText = `
      position: fixed; top: 20px; right: 20px;
      display: flex; flex-direction: column; gap: 12px;
      z-index: 9999; pointer-events: none;
    `;
    return el;
  }

  show(message, type = 'success', duration = 3000) {
    const notification = document.createElement('div');
    notification.className = `notification notification--${type}`;
    notification.textContent = message;
    notification.style.cssText = `
      padding: 14px 20px;
      border-radius: 12px;
      background: ${type === 'success' ? '#10b981' : '#ef4444'};
      color: white;
      font-weight: 500;
      pointer-events: auto;
      cursor: pointer;
      transform: translateX(120%);
      opacity: 0;
      transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1), 
                  opacity 0.3s ease;
    `;

    this.container.appendChild(notification);

    // Entrar
    requestAnimationFrame(() => {
      notification.style.transform = 'translateX(0)';
      notification.style.opacity = '1';
    });

    // Sair
    const dismiss = () => {
      notification.style.transform = 'translateX(120%)';
      notification.style.opacity = '0';
      setTimeout(() => notification.remove(), 400);
    };

    notification.addEventListener('click', dismiss);
    setTimeout(dismiss, duration);
  }
}

const notify = new NotificationSystem();
notify.show('Salvo com sucesso! ✓', 'success');
notify.show('Erro ao salvar. Tente novamente.', 'error');
```

---

## 11. Performance & Otimização

### Propriedades GPU-Friendly

```
✅ ANIMAR SEMPRE QUE POSSÍVEL:
   - transform: translate, scale, rotate, skew
   - opacity
   - filter (com moderação)

⚠️  ANIMAR COM CUIDADO (podem causar reflow/repaint):
   - border-radius
   - box-shadow
   - clip-path
   - background-position (parallax)

❌ NUNCA ANIMAR (causam layout reflow):
   - width, height
   - top, left, right, bottom (use transform!)
   - margin, padding
   - font-size
   - display
```

### will-change — Dica para o Browser

```css
/* Adicione ANTES da animação, remova DEPOIS */
.will-animate {
  will-change: transform, opacity;
}

/* Via JavaScript (ideal) */
element.addEventListener('mouseenter', () => {
  element.style.willChange = 'transform';
});

element.addEventListener('mouseleave', () => {
  element.style.willChange = 'auto'; /* limpar após uso */
});

/* ⚠️  NÃO use will-change em tudo — consome memória GPU */
```

### Checklist de Performance

```javascript
// 1. Verificar se animação está na GPU (DevTools > Layers)

// 2. Limitar elementos animados simultaneamente
const MAX_CONCURRENT = 6;

// 3. Debounce em resize e scroll
const debouncedResize = debounce(() => {
  ScrollTrigger.refresh(); // GSAP
}, 250);
window.addEventListener('resize', debouncedResize);

// 4. Usar compositing layers estrategicamente
.composited-layer {
  transform: translateZ(0); /* ou translate3d(0,0,0) */
  isolation: isolate;
}

// 5. Pausar animações fora da tela
const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    const animation = entry.target.getAnimations()[0];
    if (animation) {
      entry.isIntersecting ? animation.play() : animation.pause();
    }
  });
});

// 6. Medir FPS em dev
let frames = 0;
let lastTime = performance.now();

function measureFPS() {
  frames++;
  const now = performance.now();
  if (now >= lastTime + 1000) {
    console.log(`FPS: ${Math.round((frames * 1000) / (now - lastTime))}`);
    frames = 0;
    lastTime = now;
  }
  requestAnimationFrame(measureFPS);
}
```

---

## 12. Acessibilidade em Animações

### prefers-reduced-motion — OBRIGATÓRIO

```css
/* Padrão: animações habilitadas */
.animated-element {
  animation: slideIn 0.6s ease forwards;
  transition: transform 0.3s ease;
}

/* Respeitar preferência do usuário */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}

/* Abordagem alternativa — manter apenas transições essenciais */
@media (prefers-reduced-motion: reduce) {
  .decorative-animation { animation: none; }
  .loading-spinner { animation: spin 1s linear infinite; } /* manter spinner */
}
```

```javascript
// JavaScript — verificar preferência
const prefersReducedMotion = window.matchMedia(
  '(prefers-reduced-motion: reduce)'
).matches;

// GSAP — desabilitar globalmente
if (prefersReducedMotion) {
  gsap.globalTimeline.timeScale(100); // super rápido = sem animação
  // ou
  gsap.config({ autoSleep: 1 });
}

// React / Framer Motion
import { useReducedMotion } from 'framer-motion';

function AnimatedComponent() {
  const shouldReduceMotion = useReducedMotion();

  return (
    <motion.div
      animate={{
        y: shouldReduceMotion ? 0 : -20,
        opacity: 1,
      }}
      transition={{
        duration: shouldReduceMotion ? 0 : 0.6,
      }}
    />
  );
}
```

### Boas Práticas de Acessibilidade

```jsx
// ─── Não usar animação como única fonte de informação ───
// ❌ Ruim: só pisca sem texto
<div className="pulsing-error" />

// ✅ Bom: comunica visualmente E textualmente
<div className="pulsing-error" role="alert" aria-live="polite">
  Campo obrigatório
</div>

// ─── Loading States ───
<button aria-busy={isLoading} disabled={isLoading}>
  {isLoading ? 'Carregando...' : 'Enviar'}
</button>

// ─── Carrossel/Slider ───
<div
  role="region"
  aria-label="Carrossel de depoimentos"
  aria-live="polite"
  aria-atomic="true"
>
  <div role="group" aria-roledescription="slide" aria-label={`${current + 1} de ${total}`}>
    {slides[current]}
  </div>
</div>
```

---

## 13. Design System com Motion Tokens

### Estrutura de Tokens

```javascript
// motion-tokens.js — seu design system de movimento
export const motionTokens = {
  // Duração
  duration: {
    instant:  50,
    fast:     100,
    normal:   200,
    moderate: 350,
    slow:     500,
    deliberate: 800,
  },

  // Easing
  easing: {
    // Padrões
    linear: 'linear',
    
    // Entradas (desacelerando)
    enterScreen: [0, 0, 0.2, 1],        // Material: decelerate
    
    // Saídas (acelerando)
    exitScreen: [0.4, 0, 1, 1],         // Material: accelerate
    
    // Permanência (ambos os lados)
    withinScreen: [0.4, 0, 0.2, 1],     // Material: standard
    
    // Spring/Bounce
    spring: [0.34, 1.56, 0.64, 1],
    bounce: [0.68, -0.55, 0.265, 1.55],
    
    // Dramáticos
    exponential: [0.19, 1, 0.22, 1],
  },

  // Stagger
  stagger: {
    tight: 60,
    normal: 100,
    loose: 150,
  },

  // Propriedades por tipo
  presets: {
    fadeIn: {
      from: { opacity: 0 },
      duration: 300,
      easing: 'enterScreen',
    },
    slideUp: {
      from: { opacity: 0, y: 30 },
      duration: 400,
      easing: 'enterScreen',
    },
    scaleIn: {
      from: { opacity: 0, scale: 0.85 },
      duration: 350,
      easing: 'spring',
    },
    pageTransition: {
      from: { opacity: 0, y: 20 },
      duration: 500,
      easing: 'exponential',
    },
  }
};
```

---

## 14. Workflow com Claude + VSCode

### Extensões VSCode Recomendadas

```json
// .vscode/extensions.json
{
  "recommendations": [
    "bradlc.vscode-tailwindcss",
    "csstools.postcss",
    "antfu.unocss",
    "styled-components.vscode-styled-components",
    "ms-vscode.live-server",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "formulahendry.auto-rename-tag",
    "naumovs.color-highlight",
    "wix.vscode-import-cost"
  ]
}
```

### Snippets para Animações

```json
// .vscode/snippets/motion.code-snippets
{
  "GSAP Timeline": {
    "prefix": "gsaptl",
    "body": [
      "const tl = gsap.timeline({",
      "  defaults: { duration: ${1:0.6}, ease: '${2:expo.out}' },",
      "});",
      "",
      "tl.from('${3:.element}', { y: ${4:40}, opacity: 0 })"
    ]
  },
  "Framer Motion Component": {
    "prefix": "fmotion",
    "body": [
      "<motion.${1:div}",
      "  initial={{ opacity: 0, y: ${2:30} }}",
      "  animate={{ opacity: 1, y: 0 }}",
      "  exit={{ opacity: 0 }}",
      "  transition={{ duration: ${3:0.5}, ease: [0.22, 1, 0.36, 1] }}",
      ">",
      "  $0",
      "</motion.${1:div}>"
    ]
  },
  "ScrollTrigger": {
    "prefix": "gsapst",
    "body": [
      "gsap.from('${1:.element}', {",
      "  scrollTrigger: {",
      "    trigger: '${1:.element}',",
      "    start: 'top ${2:80%}',",
      "    toggleActions: 'play none none reverse',",
      "  },",
      "  y: ${3:40},",
      "  opacity: 0,",
      "  duration: ${4:0.7},",
      "  ease: '${5:power3.out}',",
      "});"
    ]
  },
  "CSS Keyframe": {
    "prefix": "kf",
    "body": [
      "@keyframes ${1:animationName} {",
      "  from { ${2:opacity: 0; transform: translateY(20px);} }",
      "  to   { ${3:opacity: 1; transform: translateY(0);} }",
      "}"
    ]
  }
}
```

### Prompts para Claude

```markdown
## Prompts otimizados para pedir animações ao Claude

### 1. Página completa com Motion Design
"Crie uma landing page em HTML/CSS/JS com:
- Hero com texto revelado linha por linha (split text)
- Animações de entrada com stagger nos elementos
- Parallax sutil no scroll
- Cards com hover 3D tilt
- Contador animado quando entra na viewport
- Cursor customizado
- Paleta: [cores] / Fonte: [fonte]
Use GSAP + ScrollTrigger. Nada de frameworks CSS."

### 2. Componente React
"Crie um componente React com Framer Motion:
[descrever componente]
Deve ter: animação de entrada, hover state, exit animation
TypeScript, sem styled-components, use Tailwind"

### 3. Efeito específico
"Crie o efeito de [nome do efeito] em JavaScript puro.
Performance-first: use requestAnimationFrame.
Adicione controle de prefers-reduced-motion."

### 4. Revisão de código
"Analise essas animações e sugira melhorias de:
1. Performance (propriedades GPU vs CPU)
2. Acessibilidade (reduced-motion)
3. UX (timing e easing)
4. Código (refatoração, reutilização)
[colar código]"
```

---

## 15. Componentes Prontos para Copiar

### Text Scramble Effect

```javascript
class TextScramble {
  constructor(el) {
    this.el = el;
    this.chars = '!<>-_\\/[]{}—=+*^?#abcdefghijklmnopqrstuvwxyz';
    this.update = this.update.bind(this);
  }

  setText(newText) {
    const oldText = this.el.innerText;
    const length = Math.max(oldText.length, newText.length);
    const promise = new Promise(resolve => this.resolve = resolve);
    this.queue = [];

    for (let i = 0; i < length; i++) {
      const from = oldText[i] || '';
      const to = newText[i] || '';
      const start = Math.floor(Math.random() * 20);
      const end = start + Math.floor(Math.random() * 20);
      this.queue.push({ from, to, start, end });
    }

    cancelAnimationFrame(this.frameRequest);
    this.frame = 0;
    this.update();
    return promise;
  }

  update() {
    let output = '';
    let complete = 0;

    this.queue.forEach(({ from, to, start, end }, i) => {
      if (this.frame >= end) {
        complete++;
        output += to;
      } else if (this.frame >= start) {
        output += `<span style="color:#6366f1">${this.randomChar()}</span>`;
      } else {
        output += from;
      }
    });

    this.el.innerHTML = output;

    if (complete === this.queue.length) {
      this.resolve();
    } else {
      this.frameRequest = requestAnimationFrame(this.update);
      this.frame++;
    }
  }

  randomChar() {
    return this.chars[Math.floor(Math.random() * this.chars.length)];
  }
}

// Uso
const el = document.querySelector('.scramble-text');
const scrambler = new TextScramble(el);

const phrases = ['Motion Design', 'Animações Web', 'Experiências', 'Interfaces'];
let counter = 0;

const cycle = () => {
  scrambler.setText(phrases[counter]).then(() => {
    setTimeout(cycle, 2000);
  });
  counter = (counter + 1) % phrases.length;
};

cycle();
```

### Cursor Customizado

```javascript
class CustomCursor {
  constructor() {
    this.cursor = document.createElement('div');
    this.cursor.className = 'custom-cursor';
    this.follower = document.createElement('div');
    this.follower.className = 'custom-cursor-follower';

    document.body.appendChild(this.cursor);
    document.body.appendChild(this.follower);

    this.x = 0; this.y = 0;
    this.fx = 0; this.fy = 0;

    this.bindEvents();
    this.animate();
  }

  bindEvents() {
    document.addEventListener('mousemove', (e) => {
      this.x = e.clientX;
      this.y = e.clientY;
    });

    // Expandir em links e botões
    document.querySelectorAll('a, button').forEach(el => {
      el.addEventListener('mouseenter', () => this.cursor.classList.add('is-hover'));
      el.addEventListener('mouseleave', () => this.cursor.classList.remove('is-hover'));
    });
  }

  animate() {
    this.cursor.style.transform = `translate(${this.x}px, ${this.y}px)`;

    // Follower com delay (lerp)
    this.fx += (this.x - this.fx) * 0.1;
    this.fy += (this.y - this.fy) * 0.1;
    this.follower.style.transform = `translate(${this.fx}px, ${this.fy}px)`;

    requestAnimationFrame(() => this.animate());
  }
}
```

```css
/* CSS do cursor */
*, *::before, *::after { cursor: none !important; }

.custom-cursor {
  width: 8px; height: 8px;
  background: #6366f1;
  border-radius: 50%;
  position: fixed;
  top: -4px; left: -4px;
  pointer-events: none;
  z-index: 99999;
  transition: transform 0.1s ease, width 0.3s ease, height 0.3s ease;
}

.custom-cursor.is-hover {
  width: 40px; height: 40px;
  top: -20px; left: -20px;
  background: transparent;
  border: 2px solid #6366f1;
  mix-blend-mode: difference;
}

.custom-cursor-follower {
  width: 32px; height: 32px;
  border: 1px solid rgba(99, 102, 241, 0.4);
  border-radius: 50%;
  position: fixed;
  top: -16px; left: -16px;
  pointer-events: none;
  z-index: 99998;
}
```

### Card 3D Tilt

```javascript
class TiltCard {
  constructor(selector, options = {}) {
    this.elements = document.querySelectorAll(selector);
    this.options = {
      strength: 15,
      glare: true,
      ...options,
    };
    this.init();
  }

  init() {
    this.elements.forEach(el => {
      if (this.options.glare) {
        const glare = document.createElement('div');
        glare.className = 'tilt-glare';
        el.appendChild(glare);
        el.style.position = 'relative';
        el.style.overflow = 'hidden';
      }

      el.addEventListener('mousemove', (e) => this.onMove(e, el));
      el.addEventListener('mouseleave', (e) => this.onLeave(el));
    });
  }

  onMove(e, el) {
    const rect = el.getBoundingClientRect();
    const x = (e.clientX - rect.left) / rect.width;
    const y = (e.clientY - rect.top) / rect.height;

    const tiltX = (y - 0.5) * this.options.strength;
    const tiltY = (x - 0.5) * -this.options.strength;

    el.style.transform = `perspective(1000px) rotateX(${tiltX}deg) rotateY(${tiltY}deg) scale(1.02)`;
    el.style.transition = 'transform 0.1s ease';

    const glare = el.querySelector('.tilt-glare');
    if (glare) {
      const angle = Math.atan2(e.clientY - rect.top - rect.height / 2,
                               e.clientX - rect.left - rect.width / 2) * (180 / Math.PI);
      glare.style.transform = `rotate(${angle}deg)`;
      glare.style.opacity = '0.3';
    }
  }

  onLeave(el) {
    el.style.transform = 'perspective(1000px) rotateX(0) rotateY(0) scale(1)';
    el.style.transition = 'transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1)';

    const glare = el.querySelector('.tilt-glare');
    if (glare) glare.style.opacity = '0';
  }
}

new TiltCard('.card-3d', { strength: 12 });
```

---

## 16. Ferramentas & Recursos

### Geradores Online

| Ferramenta | Para quê | URL |
|---|---|---|
| **Cubic Bezier** | Criar curvas customizadas | cubic-bezier.com |
| **Easings.net** | Visualizar todas as easings | easings.net |
| **GSAP Ease Visualizer** | Easings do GSAP | gsap.com/ease-visualizer |
| **Animista** | Gerar @keyframes CSS | animista.net |
| **Haikei** | Backgrounds SVG animados | haikei.app |
| **SVG Path Editor** | Editar paths SVG | yqnn.github.io/svg-path-editor |
| **Rive** | Animações interativas | rive.app |
| **Spline** | 3D para web | spline.design |

### Bibliotecas Complementares

```bash
# Splitting de texto (alternativa free ao SplitText)
npm install splitting

# Anime.js (alternativa leve ao GSAP)
npm install animejs

# Motion One (baseado na Web Animations API)
npm install motion

# Auto Animate (animações automáticas de layout)
npm install @formkit/auto-animate

# Theatre.js (editor visual de animações)
npm install @theatre/core @theatre/studio

# Popmotion (física de animações)
npm install popmotion
```

### Checklist Final Antes de Publicar

```
PERFORMANCE
□ Animando apenas transform e opacity?
□ will-change removido após uso?
□ Animações pausadas fora do viewport?
□ FPS estável acima de 55fps em mobile?
□ Teste em hardware lento (throttle DevTools)?

ACESSIBILIDADE
□ prefers-reduced-motion implementado?
□ Animações não são única fonte de feedback?
□ Screen reader funciona sem animações?
□ Não há conteúdo piscando mais de 3x/s?

UX
□ Duração proporcional à distância/importância?
□ Easing adequado ao contexto?
□ Animações tem propósito (não são decorativas)?
□ Feedback imediato em interações (<100ms)?

CÓDIGO
□ Reutilizando variáveis/tokens de motion?
□ Animações encapsuladas em componentes?
□ Cleanup de event listeners e observers?
□ Nenhum reflow causado por animações?
```

---

## 17. Referências & Inspiração

### Sites para se Inspirar

- **[Awwwards](https://awwwards.com)** — Melhores sites do mundo
- **[Codrops](https://tympanus.net/codrops)** — Tutoriais e demos avançados
- **[CSS-Tricks](https://css-tricks.com)** — Referência CSS e animações
- **[Dribbble](https://dribbble.com)** — Conceitos de UI animados
- **[Lapa Ninja](https://lapa.ninja)** — Landing pages top

### Canais/Creators para Seguir

- **Fireship** (YouTube) — Animações web modernas
- **Kevin Powell** (YouTube) — CSS animations master
- **Jack Harrington** (YouTube) — React + Framer Motion
- **Bruno Simon** (Twitter/X) — Three.js criativo
- **Pika** (Twitter/X) — Motion design conceitos

### Documentação Oficial

- [GSAP Docs](https://gsap.com/docs/v3) — Completa e com playground
- [Framer Motion](https://www.framer.com/motion) — Com exemplos interativos
- [MDN Web Animations API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API)
- [Three.js Docs](https://threejs.org/docs)
- [React Three Fiber](https://docs.pmnd.rs/react-three-fiber)

---

## 🤝 Contribuindo

Este guia é open source e feito para a comunidade. Para contribuir:

1. Fork o repositório
2. Crie uma branch: `git checkout -b feature/nova-tecnica`
3. Adicione seu conteúdo seguindo o estilo do guia
4. Abra um Pull Request com descrição clara

**O que aceitar em PRs:**
- Novos componentes prontos para copiar
- Técnicas e padrões não cobertos
- Correções e melhorias
- Traduções (EN, ES, FR)

---

<div align="center">

**Feito com ❤️ para a comunidade de desenvolvedores**

*Licença MIT — Use, modifique e compartilhe livremente*

</div>
