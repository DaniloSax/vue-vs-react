# Vue 3 vs React: Diferenças, Exemplos e Qual Escolher

Vue 3 e React são dois dos frameworks JavaScript mais populares para construção de interfaces modernas. Apesar de ambos serem eficientes, suas abordagens são diferentes. Neste artigo, vamos comparar:

- Sintaxe e estrutura
- Componentização
- Gerenciamento de estado (local e global)
- Ecossistema e curva de aprendizado
- Performance (benchmarking)
- Quando usar cada um

---

## 1. Sintaxe e Estrutura

**Vue 3 (Composition API):**

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

const increment = () => count.value++
</script>

<template>
  <button @click="increment">Count: {{ count }}</button>
</template>
```

**React (Hooks):**

```jsx
import { useState } from 'react'

function Counter() {
  const [count, setCount] = useState(0)
  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>
}
```

**Resumo:**  
Vue separa o template e o script, enquanto React usa JSX para unir lógica e marcação. Vue é mais próximo de HTML puro, enquanto React exige mais familiaridade com JavaScript moderno.

---

## 2. Componentização

**Vue 3:**

```vue
<!-- HelloWorld.vue -->
<template>
  <h1>Hello {{ name }}</h1>
</template>

<script setup>
defineProps(['name'])
</script>
```

**React:**

```jsx
function HelloWorld({ name }) {
  return <h1>Hello {name}</h1>
}
```

**Resumo:**  
Ambos são fortemente baseados em componentes, mas Vue oferece ferramentas integradas para passagem de props, slots e eventos com menor verbosidade.

---

## 3. Gerenciamento de Estado

### Estado Local:

Ambos usam `ref/useState` para estado local.

### Estado Global:

**Vue (Pinia):**

```js
// store/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({ count: 0 }),
  actions: {
    increment() {
      this.count++
    },
  },
})
```

**Uso do Pinia no Vue:**

```vue
<script setup>
import { useCounterStore } from '@/store/counter'

const counter = useCounterStore()
</script>

<template>
  <button @click="counter.increment">Count: {{ counter.count }}</button>
</template>
```

**Provide/Inject no Vue 3:**

```vue
// Parent.vue
import { provide } from 'vue'

export default {
  setup() {
    provide('counterKey', {
      count: ref(0),
      increment: () => count.value++,
    })
  },
}
```

**Usando o Provide/Inject no Vue 3 com Composition API:**

```vue
<!-- Parent.vue -->
<script setup>
import { ref, provide } from 'vue'

const count = ref(0)
const increment = () => count.value++

provide('counter', { count, increment })
</script>

<template>
  <div>
    <p>Parent count: {{ count }}</p>
    <button @click="increment">Increment</button>
    <ChildComponent />
  </div>
</template>
```

```vue
<!-- ChildComponent.vue -->
<script setup>
import { inject } from 'vue'

const { count, increment } = inject('counter')
</script>

<template>
  <div>
    <p>Child received count: {{ count }}</p>
    <button @click="increment">Increment from child</button>
  </div>
</template>
```

**React (Context API):**

```jsx
// CounterContext.js
const CounterContext = createContext()

function CounterProvider({ children }) {
  const [count, setCount] = useState(0)
  return <CounterContext.Provider value={{ count, setCount }}>{children}</CounterContext.Provider>
}
```

**Uso do Context API no React:**

```jsx
function Counter() {
  const { count, setCount } = useContext(CounterContext)
  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>
}
```

**React (Redux):**

```jsx
// counterSlice.js
import { createSlice } from '@reduxjs/toolkit'

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1
    },
  },
})

export const { increment } = counterSlice.actions
export default counterSlice.reducer
```

**Uso do Redux no React:**

```jsx
import { useSelector, useDispatch } from 'react-redux'
import { increment } from './counterSlice'

function Counter() {
  const count = useSelector((state) => state.counter.value)
  const dispatch = useDispatch()

  return <button onClick={() => dispatch(increment())}>Count: {count}</button>
}
```

**Resumo:**  
Vue + Pinia oferece uma sintaxe mais direta e menos boilerplate para gerenciamento de estado. Em React, o Context API é nativo, mas pode ser verboso para aplicações complexas. Redux é mais estruturado e poderoso para grandes aplicações React, mas exige mais código inicial. Para projetos React modernos de médio a grande porte, o Redux Toolkit tem simplificado significativamente a experiência.

---

## 4. Ecossistema e Ferramentas

| Critério            | Vue 3                           | React                             |
| ------------------- | ------------------------------- | --------------------------------- |
| CLI oficial         | `Vite` (também usado por React) | `Create React App` (CRA)          |
| DevTools            | Vue Devtools                    | React DevTools                    |
| Framework fullstack | Nuxt                            | Next.js                           |
| Comunidade          | Menor que React                 | Muito ampla                       |
| Documentação        | Muito clara e didática          | Boa, mas exige mais background JS |

---

## 5. Performance (Benchmarking)

Ambos têm performances similares em aplicações reais. Em benchmarks sintéticos (como [js-framework-benchmark](https://krausest.github.io/js-framework-benchmark/current.html)), Vue geralmente é levemente mais rápido na criação e destruição de DOM, mas React é mais otimizado com re-renderizações complexas via `memo`, `useCallback`, etc.

**Resumo:**  
Ambos são rápidos o suficiente. A performance dificilmente será um critério decisivo para apps comuns.

---

## 6. Quando Usar Cada Um?

| Critério                     | Use Vue 3                            | Use React                                              |
| ---------------------------- | ------------------------------------ | ------------------------------------------------------ |
| Curva de aprendizado         | Mais suave para quem vem do HTML/CSS | Melhor se você já domina JS moderno                    |
| Projetos pequenos a médios   | Mais produtivo com menos boilerplate | Pode ser verboso, mas flexível                         |
| Ecossistema / job market     | Menor, mas crescente                 | Gigante, com muitas oportunidades                      |
| Fullstack com SSR            | Nuxt 3                               | Next.js                                                |
| Ferramentas de UI prontas    | Quasar, Vuetify                      | Material UI, Chakra, Tailwind + UI libs                |
| Customização profunda com JS | Possível, mas menos "raw JS"         | Flexibilidade máxima, ideal pra arquiteturas complexas |

---

## Conclusão

- **Vue 3** é ideal para produtividade rápida, curva de aprendizado suave e simplicidade.
- **React** brilha em projetos complexos, grandes times e ecossistema massivo.

---
