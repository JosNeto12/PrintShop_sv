# 🚀 PrintShop_sv — Suite de Herramientas para Negocios

**PrintShop_sv** es una suite modular basada en la web, diseñada para la gestión de pequeños y medianos negocios. Su interfaz cuenta con un diseño **Liquid Glass** (inspirado en la estética de Apple TV / iOS), ofreciendo una experiencia fluida, 100% privada, funcional offline y sin costos de suscripción.

---

## 🗺 Plan de Ejecución

El proyecto está planteado en 4 fases de desarrollo:

| Fase | Objetivo | Resultado | Estado |
| :--- | :--- | :--- | :---: |
| **Fase 1** | Configuración base del repositorio y entorno Vite + React + Tailwind CSS | Proyecto ejecutable en GitHub Codespaces | ✅ Completado |
| **Fase 2** | Creación de componentes de arquitectura y estilos *Liquid Glass* | Sistema de temas, rutas e infraestructura base | ✅ Completado |
| **Fase 3** | Desarrollo del Hub principal y persistencia del modo claro/oscuro | Pantalla de inicio animada e interactiva con navegación | ✅ Completado |
| **Fase 4** | Reconstrucción modular del sistema Facturador | Módulo de impresión, cotizaciones e historial integrado | ⏳ Pendiente |

---

## 🛠 Tech Stack

- **Framework Frontend:** React (Vite)
- **Estilos:** Tailwind CSS (v3) + PostCSS + Autoprefixer
- **Iconografía:** Lucide React
- **Persistencia:** LocalStorage API
- **Entorno de Desarrollo:** GitHub Codespaces / Node.js

---

## 📂 Estructura del Proyecto

```text
PrintShop_sv/
├── index.html
├── package.json
├── postcss.config.js
├── tailwind.config.js
├── vite.config.js
└── src/
    ├── main.jsx
    ├── App.jsx
    ├── index.css
    ├── context/
    │   └── ThemeContext.jsx
    ├── hooks/
    │   └── useLocalStorage.js
    ├── data/
    │   └── modules.js
    ├── pages/
    │   └── Hub.jsx
    └── modules/
        └── facturador/
            └── FacturadorApp.jsx
```

---

## 🔧 Guía de Instalación y Configuración Inicial

### 1. Inicialización del Repositorio y Entorno

En GitHub, crea un repositorio público llamado `PrintShop_sv` (sin inicializar con README o `.gitignore`). Abre un **Codespace** sobre la rama principal (`main`) e ingresa las siguientes instrucciones en la terminal:

```bash
# Inicializar proyecto React con Vite
npm create vite@latest . -- --template react

# Instalar dependencias
npm install
npm install lucide-react
npm install -D tailwindcss@3 postcss autoprefixer
npx tailwindcss init -p
```

---

### 2. Archivos de Configuración Base

#### `vite.config.js`
```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
});
```

#### `tailwind.config.js`
```javascript
export default {
  darkMode: 'class',
  content: ['./index.html', './src/**/*.{js,jsx}'],
  theme: {
    extend: {
      fontFamily: {
        sans: ['-apple-system', 'BlinkMacSystemFont', 'SF Pro Display', 'Inter', 'Segoe UI', 'sans-serif'],
      },
      animation: {
        'float': 'float 8s ease-in-out infinite',
        'float-slow': 'float 14s ease-in-out infinite',
        'shimmer': 'shimmer 8s linear infinite',
      },
      keyframes: {
        float: {
          '0%, 100%': { transform: 'translateY(0px) translateX(0px)' },
          '50%': { transform: 'translateY(-30px) translateX(20px)' },
        },
        shimmer: {
          '0%': { backgroundPosition: '0% 50%' },
          '100%': { backgroundPosition: '200% 50%' },
        },
      },
    },
  },
  plugins: [],
};
```

#### `postcss.config.js`
```javascript
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
```

#### `index.html`
```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="https://api.iconify.design/lucide/sparkles.svg?color=%236366f1" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover" />
    <meta name="theme-color" content="#000000" media="(prefers-color-scheme: dark)" />
    <meta name="theme-color" content="#f5f5f7" media="(prefers-color-scheme: light)" />
    <title>PrintShop SV — Suite de Herramientas</title>
    <meta name="description" content="Suite modular para negocios: facturación, inventario y más. 100% privado, offline y sin suscripciones." />
    <script>
      (function () {
        try {
          var t = localStorage.getItem('printshop_theme');
          var sys = window.matchMedia('(prefers-color-scheme: dark)').matches;
          if (t === 'dark' || (!t && sys)) document.documentElement.classList.add('dark');
        } catch (e) {}
      })();
    </script>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

---

### 3. Estilos Globales Liquid Glass

#### `src/index.css`
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    -webkit-tap-highlight-color: transparent;
    scroll-behavior: smooth;
  }
  body {
    font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'Inter', 'Segoe UI', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-rendering: optimizeLegibility;
  }
  ::selection {
    background: rgba(99, 102, 241, 0.3);
  }
}

@layer components {
  /* Liquid Glass base */
  .glass {
    background: rgba(255, 255, 255, 0.55);
    backdrop-filter: blur(40px) saturate(180%);
    -webkit-backdrop-filter: blur(40px) saturate(180%);
    border: 1px solid rgba(255, 255, 255, 0.5);
    box-shadow:
      0 8px 32px rgba(0, 0, 0, 0.06),
      inset 0 1px 1px rgba(255, 255, 255, 0.7);
  }
  .dark .glass {
    background: rgba(255, 255, 255, 0.055);
    border: 1px solid rgba(255, 255, 255, 0.1);
    box-shadow:
      0 8px 32px rgba(0, 0, 0, 0.4),
      inset 0 1px 1px rgba(255, 255, 255, 0.1);
  }

  .glass-strong {
    background: rgba(255, 255, 255, 0.75);
    backdrop-filter: blur(60px) saturate(200%);
    -webkit-backdrop-filter: blur(60px) saturate(200%);
    border: 1px solid rgba(255, 255, 255, 0.6);
    box-shadow:
      0 12px 40px rgba(0, 0, 0, 0.08),
      inset 0 1px 1px rgba(255, 255, 255, 0.8);
  }
  .dark .glass-strong {
    background: rgba(255, 255, 255, 0.08);
    border: 1px solid rgba(255, 255, 255, 0.14);
    box-shadow:
      0 12px 40px rgba(0, 0, 0, 0.5),
      inset 0 1px 1px rgba(255, 255, 255, 0.14);
  }
}
```

---

### 4. Código Fuente de la Aplicación

Crea la estructura de carpetas necesarias ejecutando:

```bash
mkdir -p src/context src/hooks src/pages src/data src/components src/modules/facturador
```

#### `src/context/ThemeContext.jsx`
```jsx
import { createContext, useContext, useEffect, useState } from 'react';

const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState(() => {
    try {
      const stored = localStorage.getItem('printshop_theme');
      if (stored === 'dark' || stored === 'light') return stored;
      return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
    } catch {
      return 'light';
    }
  });

  useEffect(() => {
    const root = document.documentElement;
    if (theme === 'dark') root.classList.add('dark');
    else root.classList.remove('dark');
    try { localStorage.setItem('printshop_theme', theme); } catch {}
  }, [theme]);

  const toggleTheme = () => setTheme((t) => (t === 'dark' ? 'light' : 'dark'));

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export const useTheme = () => useContext(ThemeContext);
```

#### `src/hooks/useLocalStorage.js`
```jsx
import { useEffect, useState } from 'react';

export function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    try {
      const stored = localStorage.getItem(key);
      return stored ? JSON.parse(stored) : initialValue;
    } catch {
      return initialValue;
    }
  });

  useEffect(() => {
    try { localStorage.setItem(key, JSON.stringify(value)); } catch {}
  }, [key, value]);

  return [value, setValue];
}
```

#### `src/data/modules.js`
```jsx
import { Receipt, Package, Users, BarChart3 } from 'lucide-react';

export const modules = [
  {
    id: 'facturador',
    name: 'Facturador',
    description: 'Recibos y cotizaciones profesionales con impresión A4 y ticket 80 mm.',
    icon: Receipt,
    color: 'from-blue-500 via-indigo-500 to-purple-500',
    glow: 'shadow-blue-500/40',
    accent: 'text-blue-500 dark:text-blue-400',
    available: true,
  },
  {
    id: 'inventario',
    name: 'Inventario',
    description: 'Control de productos, stock y movimientos.',
    icon: Package,
    color: 'from-emerald-500 via-teal-500 to-cyan-500',
    glow: 'shadow-emerald-500/40',
    accent: 'text-emerald-500 dark:text-emerald-400',
    available: false,
  },
  {
    id: 'clientes',
    name: 'Clientes',
    description: 'Gestión de contactos y seguimiento comercial.',
    icon: Users,
    color: 'from-orange-500 via-amber-500 to-yellow-500',
    glow: 'shadow-orange-500/40',
    accent: 'text-orange-500 dark:text-orange-400',
    available: false,
  },
  {
    id: 'reportes',
    name: 'Reportes',
    description: 'Análisis de ventas y estadísticas del negocio.',
    icon: BarChart3,
    color: 'from-rose-500 via-pink-500 to-fuchsia-500',
    glow: 'shadow-rose-500/40',
    accent: 'text-rose-500 dark:text-rose-400',
    available: false,
  },
];
```

#### `src/main.jsx`
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App.jsx';
import './index.css';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

#### `src/App.jsx`
```jsx
import { useState } from 'react';
import { ThemeProvider } from './context/ThemeContext';
import Hub from './pages/Hub';
import FacturadorApp from './modules/facturador/FacturadorApp';

export default function App() {
  const [route, setRoute] = useState({ view: 'hub' });

  return (
    <ThemeProvider>
      {route.view === 'hub' && <Hub onNavigate={setRoute} />}
      {route.view === 'facturador' && (
        <FacturadorApp onBack={() => setRoute({ view: 'hub' })} />
      )}
    </ThemeProvider>
  );
}
```

#### `src/pages/Hub.jsx`
```jsx
import { useEffect, useState } from 'react';
import { Moon, Sun, Sparkles, ChevronRight, Lock } from 'lucide-react';
import { useTheme } from '../context/ThemeContext';
import { modules } from '../data/modules';

export default function Hub({ onNavigate }) {
  const { theme, toggleTheme } = useTheme();
  const [time, setTime] = useState(new Date());

  useEffect(() => {
    const timer = setInterval(() => setTime(new Date()), 60_000);
    return () => clearInterval(timer);
  }, []);

  const greeting = (() => {
    const h = time.getHours();
    if (h < 12) return 'Buenos días';
    if (h < 19) return 'Buenas tardes';
    return 'Buenas noches';
  })();

  return (
    <div className="min-h-screen relative overflow-hidden bg-gradient-to-br from-slate-50 via-white to-slate-100 dark:from-black dark:via-zinc-950 dark:to-black transition-colors duration-700">
      {/* Ambient gradient orbs */}
      <div className="pointer-events-none fixed inset-0 overflow-hidden">
        <div className="absolute -top-40 -left-40 w-[600px] h-[600px] rounded-full bg-blue-500/25 dark:bg-blue-600/30 blur-[120px] animate-float" />
        <div
          className="absolute top-1/3 -right-40 w-[500px] h-[500px] rounded-full bg-purple-500/25 dark:bg-purple-600/30 blur-[120px] animate-float"
          style={{ animationDelay: '3s' }}
        />
        <div
          className="absolute -bottom-40 left-1/3 w-[500px] h-[500px] rounded-full bg-pink-500/20 dark:bg-pink-600/25 blur-[120px] animate-float-slow"
          style={{ animationDelay: '6s' }}
        />
      </div>

      <div className="relative z-10 max-w-6xl mx-auto px-6 md:px-10 py-10 md:py-14">
        {/* Header */}
        <header className="flex items-center justify-between mb-16 md:mb-24">
          <div className="flex items-center gap-3">
            <div className="w-11 h-11 rounded-2xl bg-gradient-to-br from-blue-500 to-purple-600 flex items-center justify-center shadow-lg shadow-blue-500/40">
              <Sparkles className="w-5 h-5 text-white" />
            </div>
            <div>
              <h1 className="text-lg font-semibold tracking-tight text-slate-900 dark:text-white">
                PrintShop SV
              </h1>
              <p className="text-xs text-slate-500 dark:text-zinc-400 font-medium">
                Suite de herramientas
              </p>
            </div>
          </div>
          <button
            onClick={toggleTheme}
            className="glass p-3 rounded-full transition-all duration-300 hover:scale-105 active:scale-95"
            aria-label="Cambiar tema"
          >
            {theme === 'dark' ? (
              <Sun className="w-5 h-5 text-amber-400" />
            ) : (
              <Moon className="w-5 h-5 text-slate-700" />
            )}
          </button>
        </header>

        {/* Greeting hero */}
        <section className="mb-16 md:mb-20">
          <h2 className="text-5xl md:text-7xl font-bold tracking-tight text-slate-900 dark:text-white leading-[1.05] mb-3">
            {greeting}.
          </h2>
          <p className="text-lg md:text-xl text-slate-600 dark:text-zinc-400 font-medium capitalize">
            {time.toLocaleDateString('es-SV', {
              weekday: 'long',
              day: 'numeric',
              month: 'long',
            })}
          </p>
        </section>

        {/* Module grid */}
        <section>
          <h3 className="text-sm font-bold uppercase tracking-widest text-slate-500 dark:text-zinc-500 mb-6 pl-1">
            Módulos
          </h3>
          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-5">
            {modules.map((m) => {
              const Icon = m.icon;
              const disabled = !m.available;
              return (
                <button
                  key={m.id}
                  onClick={() => m.available && onNavigate({ view: m.id })}
                  disabled={disabled}
                  className={`group relative text-left glass rounded-3xl p-6 overflow-hidden transition-all duration-300 ${
                    disabled
                      ? 'opacity-60 cursor-not-allowed'
                      : 'hover:scale-[1.02] hover:shadow-2xl active:scale-[0.98]'
                  }`}
                >
                  <div
                    className={`absolute inset-0 bg-gradient-to-br ${m.color} opacity-0 group-hover:opacity-10 transition-opacity duration-500 pointer-events-none`}
                  />
                  <div className="relative z-10">
                    <div
                      className={`w-14 h-14 rounded-2xl bg-gradient-to-br ${m.color} flex items-center justify-center mb-5 shadow-lg ${m.glow}`}
                    >
                      <Icon className="w-7 h-7 text-white" strokeWidth={2.2} />
                    </div>
                    <h4 className="text-xl font-bold text-slate-900 dark:text-white mb-1.5 tracking-tight">
                      {m.name}
                    </h4>
                    <p className="text-sm text-slate-600 dark:text-zinc-400 mb-6 leading-relaxed">
                      {m.description}
                    </p>
                    <div
                      className={`flex items-center gap-1.5 text-sm font-semibold ${
                        disabled ? 'text-slate-400 dark:text-zinc-500' : m.accent
                      }`}
                    >
                      {disabled ? (
                        <>
                          <Lock className="w-3.5 h-3.5" /> Próximamente
                        </>
                      ) : (
                        <span className="flex items-center gap-1 group-hover:gap-2 transition-all">
                          Abrir <ChevronRight className="w-4 h-4" />
                        </span>
                      )}
                    </div>
                  </div>
                </button>
              );
            })}
          </div>
        </section>

        <footer className="mt-24 text-center text-xs text-slate-500 dark:text-zinc-600 font-medium">
          PrintShop SV · 100% privado · Funciona offline · Sin suscripciones
        </footer>
      </div>
    </div>
  );
}
```

#### `src/modules/facturador/FacturadorApp.jsx`
```jsx
import { ArrowLeft } from 'lucide-react';

export default function FacturadorApp({ onBack }) {
  return (
    <div className="min-h-screen bg-slate-50 dark:bg-zinc-950 transition-colors duration-500">
      <div className="max-w-5xl mx-auto px-6 py-10">
        <button
          onClick={onBack}
          className="glass flex items-center gap-2 px-4 py-2.5 rounded-full mb-8 text-sm font-semibold text-slate-700 dark:text-zinc-200 hover:scale-105 active:scale-95 transition-all"
        >
          <ArrowLeft className="w-4 h-4" /> Volver al inicio
        </button>
        <div className="glass rounded-3xl p-12 text-center">
          <h2 className="text-2xl font-bold text-slate-900 dark:text-white mb-2">
            Módulo Facturador
          </h2>
          <p className="text-slate-600 dark:text-zinc-400">
            Listo para reconstruir. Aquí integraremos dashboard, editor, historial, settings e impresión.
          </p>
        </div>
      </div>
    </div>
  );
}
```

---

## 🚀 Ejecución y Despliegue

### Modo Desarrollo
Para iniciar el servidor local de Vite:

```bash
npm run dev
```

### Despliegue en Netlify
1. Realiza el commit de los cambios y publica la rama en GitHub.
2. En [Netlify](https://app.netlify.com/), conecta tu cuenta de GitHub y selecciona el repositorio `PrintShop_sv`.
3. Configura los parámetros de compilación:
   - **Build Command:** `npm run build`
   - **Publish Directory:** `dist`
4. Despliega la aplicación.
