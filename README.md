🌟 Prompt Detalhado para Candidato-1.5 Pro (ATS Local Avançado) 🌟

Objetivo Final:
Criar um sistema ATS (Applicant Tracking System) 100% frontend com TypeScript que gerencia candidatos localmente com dashboard analítico completo.

📁 Estrutura de Arquivos Detalhada:

markdown
src/
├── core/                  # Lógica central
│   ├── hooks/             # Hooks customizados
│   │   ├── useCandidates.ts   # Gestão de estado dos candidatos
│   │   ├── usePagination.ts   # Lógica de paginação
│   │   └── useStorage.ts      # Abstraction do LocalStorage
│   │
│   ├── utils/             # Utilitários
│   │   ├── filters.ts     # Funções de filtragem
│   │   ├── stats.ts       # Cálculos estatísticos
│   │   └── fileUtils.ts   # Manipulação de arquivos mock
│   │
│   └── types/             # Tipos TypeScript
│       └── candidate.ts   # Tipos e interfaces
│
├── modules/               # Funcionalidades
│   ├── candidates/        # Módulo de candidatos
│   │   ├── components/    # Componentes UI
│   │   │   ├── CandidateForm.tsx  # Formulário CRUD
│   │   │   ├── CandidateCard.tsx  # Card de visualização
│   │   │   └── CandidateList.tsx  # Lista com filtros
│   │   │
│   │   └── services/      # Lógica de negócio
│   │       └── candidateService.ts  # Operações CRUD
│   │
│   └── dashboard/         # Módulo analítico
│       ├── components/
│       │   ├── StatusChart.tsx     # Gráfico de status
│       │   ├── SkillsChart.tsx     # Gráfico de skills
│       │   └── TimelineChart.tsx   # Linha do tempo
│       │
│       └── utils/
│           └── chartData.ts   # Transformação de dados
│
├── shared/                # Componentes compartilhados
│   ├── ui/
│   │   ├── FileUploader.tsx  # Componente de upload
│   │   └── Pagination.tsx    # Controle de paginação
│   │
│   └── layout/
│       ├── AppLayout.tsx     # Layout principal
│       └── Navbar.tsx        # Barra de navegação
│
├── contexts/              # Contextos globais
│   ├── AppContext.tsx     # Estado global
│   └── ThemeContext.tsx   # Tema (dark/light)
│
└── pages/                 # Páginas principais
    ├── CandidatesPage.tsx # Página de listagem
    └── DashboardPage.tsx  # Página analítica
📝 Arquivos Críticos com Implementação:

src/core/types/candidate.ts (Tipagem central)

typescript
export type CandidateStatus = 'pending' | 'approved' | 'rejected';

export interface Candidate {
  id: string;
  name: string;
  email: string;
  phone?: string;
  skills: string[];
  status: CandidateStatus;
  isFavorite: boolean;
  appliedDate: Date;
  notes?: string;
  resume?: {  // Mock de arquivo
    name: string;
    size: string;
    lastModified: number;
  };
}

export interface CandidateStats {
  total: number;
  byStatus: Record<CandidateStatus, number>;
  skillsDistribution: Record<string, number>;
  monthlyApplications: { month: string; count: number }[];
}
src/core/hooks/useCandidates.ts (Lógica principal)

typescript
import { useState, useEffect, useCallback } from 'react';
import { v4 as uuidv4 } from 'uuid';
import { Candidate } from '../types/candidate';

const STORAGE_KEY = 'candidates-data';

export const useCandidates = () => {
  const [candidates, setCandidates] = useState<Candidate[]>([]);
  const [loading, setLoading] = useState(true);

  // Persistência no LocalStorage
  useEffect(() => {
    const saved = localStorage.getItem(STORAGE_KEY);
    setCandidates(saved ? JSON.parse(saved) : []);
    setLoading(false);
  }, []);

  useEffect(() => {
    if (!loading) {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(candidates));
    }
  }, [candidates, loading]);

  const addCandidate = (candidate: Omit<Candidate, 'id' | 'appliedDate'>) => {
    const newCandidate: Candidate = {
      ...candidate,
      id: uuidv4(),
      appliedDate: new Date(),
      isFavorite: false,
      status: 'pending'
    };
    setCandidates(prev => [...prev, newCandidate]);
  };

  // ... outras operações CRUD

  const calculateStats = useCallback((): CandidateStats => {
    // Lógica para gerar estatísticas
  }, [candidates]);

  return {
    candidates,
    loading,
    addCandidate,
    calculateStats,
    // ... outras funções
  };
};
src/modules/candidates/components/CandidateForm.tsx (Formulário completo)

typescript
import { useForm } from 'react-hook-form';
import { FileUploader } from '../../../shared/ui/FileUploader';
import { Candidate } from '../../../core/types/candidate';

type Props = {
  initialData?: Candidate;
  onSubmit: (data: Omit<Candidate, 'id' | 'appliedDate'>) => void;
};

export const CandidateForm = ({ initialData, onSubmit }: Props) => {
  const { register, handleSubmit, setValue } = useForm();
  const [skills, setSkills] = useState<string[]>(initialData?.skills || []);

  const handleAddSkill = (skill: string) => {
    if (skill && !skills.includes(skill)) {
      setSkills(prev => [...prev, skill]);
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Campos do formulário */}
      <FileUploader 
        onFileUpload={(file) => setValue('resume', file)}
      />
    </form>
  );
};
src/modules/dashboard/components/StatusChart.tsx (Gráfico analítico)

typescript
import { PieChart, Pie, Cell, ResponsiveContainer } from 'recharts';

const COLORS = ['#FBBF24', '#34D399', '#F87171'];

export const StatusChart = ({ data }: { data: { status: string; count: number }[] }) => (
  <ResponsiveContainer width="100%" height={300}>
    <PieChart>
      <Pie
        data={data}
        cx="50%"
        cy="50%"
        labelLine={false}
        outerRadius={80}
        fill="#8884d8"
        dataKey="count"
      >
        {data.map((_, index) => (
          <Cell key={`cell-${index}`} fill={COLORS[index % COLORS.length]} />
        ))}
      </Pie>
    </PieChart>
  </ResponsiveContainer>
);
src/shared/ui/FileUploader.tsx (Upload simulado)

typescript
export const FileUploader = ({ onFileUpload }: { onFileUpload: (file: File) => void }) => {
  const handleMockUpload = () => {
    const mockFile = new File([''], 'curriculo.pdf', {
      type: 'application/pdf',
      lastModified: Date.now()
    });
    onFileUpload(mockFile);
  };

  return (
    <div onClick={handleMockUpload}>
      {/* UI do uploader */}
    </div>
  );
};
🔧 Configurações Essenciais:

tailwind.config.js (Estilos customizados)

javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        pending: '#FBBF24',
        approved: '#34D399',
        rejected: '#F87171'
      }
    }
  }
};
vite.config.ts (Build optimizado)

typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  build: {
    outDir: 'dist',
    emptyOutDir: true,
    rollupOptions: {
      output: {
        manualChunks: {
          charts: ['recharts'],
          icons: ['lucide-react']
        }
      }
    }
  }
});
🎯 Diferenciais a Implementar:

Dark Mode

Criar ThemeContext

Adicionar toggle na navbar

Configurar classes dark do Tailwind

Animações

Usar Framer Motion para:

Transição entre páginas

Animar cards de candidatos

Efeitos no dashboard

Testes

Configurar Vitest

Testes para:

Hooks customizados

Cálculo de estatísticas

Persistência no LocalStorage

📌 Prompt para Implementação:
"Crie o sistema completo baseado nesta estrutura, começando pela implementação dos arquivos core (types, hooks) em TypeScript, depois os componentes principais do módulo de candidatos (form, card, list), seguido pelo dashboard analítico. Use Tailwind para estilização e garanta que todos os componentes sejam tipados corretamente. Adicione documentação JSDoc para as funções principais."

✅ Critérios de Qualidade:

Tipagem TypeScript rigorosa

Componentes puros sempre que possível

Responsividade mobile-first

Documentação clara dos componentes

Tratamento de erros robusto

Performance optimizada (memoization, etc)

🔗 Recursos Recomendados:

Lucide React para ícones

Recharts para visualizações

date-fns para manipulação de datas

react-hook-form para formulários

zod para validação

Este prompt fornece uma base completa para desenvolver o sistema com arquitetura limpa e boas práticas. Posso detalhar qualquer parte específica conforme necessário!
