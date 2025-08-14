# Candidato-1.5-TS
 ATS (Applicant Tracking System) 100% frontend com TypeScript, React e Tailwind CSS
🌟 Prompt Detalhado para Criar Candidato-1.5 Pro (ATS Local Avançado) 🌟

Objetivo:
Criar um sistema ATS (Applicant Tracking System) 100% frontend com TypeScript, React e Tailwind CSS que:
✅ Gerencia candidatos localmente (LocalStorage)
✅ Oferece filtros/triagem avançada
✅ Inclui dashboard analítico
✅ É deployável no Vercel gratuitamente

Requisitos Técnicos:

markdown
1. **Stack Principal**  
   - React 18 + TypeScript  
   - Tailwind CSS (com configuração customizada)  
   - Vite (build tool)  
   - LocalStorage como "banco de dados"  

2. **Funcionalidades-Chave**  
   - [ ] CRUD completo de candidatos  
   - [ ] Sistema de status (Pending/Approved/Rejected)  
   - [ ] Favoritos e tags de skills  
   - [ ] Filtros combináveis (status + skills + texto)  
   - [ ] Dashboard com gráficos (status, skills, timeline)  
   - [ ] Simulador de upload/download de CV (PDF mock)  

4. **Dependências Essenciais**  
```json
"react-icons": "^4"        # Ícones  
"uuid": "^9"              # IDs únicos  
"date-fns": "^2"          # Manipulação de datas  
"react-to-print": "^2"    # Exportar PDF  
Configurações Obrigatórias

TypeScript strict mode

Tailwind com cores para status:

js
pending: '#FBBF24',  
approved: '#34D399',  
rejected: '#F87171'  
Vite configurado como SPA (single-page app)

Entrega Esperada:

Código fonte organizado conforme estrutura

README.md com:

Instruções de setup

Lista de funcionalidades

Screenshots do sistema

Configuração pronta para deploy no Vercel

Diferenciais (Opcionais):

Animais com Framer Motion

Dark mode toggle

Exportação de dados para JSON

Exemplo de Código Inicial:

tsx
// src/core/types/candidate.ts
export interface Candidate {
  id: string;
  name: string;
  skills: string[];
  status: 'pending' | 'approved' | 'rejected';
  isFavorite: boolean;
}
Prompt para GPT/Assistente:
_"Crie um projeto React com TypeScript usando a estrutura acima. Comece gerando:

O arquivo useCandidates.ts (hook completo)

Componente <CandidateCard /> com Tailwind

Configuração do vite.config.ts para SPA"_

💡 Dica: Para implementação gradual, comece pela funcionalidade de adicionar candidatos e depois evolua para filtros/dashboard.


/src
├── /assets
│   ├── /icons
│   └── /images
├── /components
│   ├── /charts
│   │   ├── StatusPieChart.tsx
│   │   ├── SkillsBarChart.tsx
│   │   └── TimelineChart.tsx
│   ├── /common
│   │   ├── DarkModeToggle.tsx
│   │   ├── Pagination.tsx
│   │   └── FileUploader.tsx
│   ├── /sections
│   │   ├── CandidateForm.tsx
│   │   ├── CandidateCard.tsx
│   │   ├── FiltersSection.tsx
│   │   └── StatsDashboard.tsx
├── /context
│   ├── AppContext.tsx
│   └── ThemeContext.tsx
├── /hooks
│   ├── useCandidates.ts
│   ├── usePagination.ts
│   └── useLocalStorage.ts
├── /pages
│   ├── CandidatesPage.tsx
│   └── DashboardPage.tsx
├── /services
│   ├── candidateService.ts
│   └── fileService.ts
├── /styles
│   ├── tailwind.config.js
│   └── theme.css
├── /types
│   └── candidate.d.ts
├── App.tsx
└── main.tsx
