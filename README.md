export const mockChecklists = [/* ... mesmos dados ... */];
export const mockViagens = [/* ... mesmos dados ... */];

export const STATUS_CONFIG = {
  checklist: {
    concluido: { color: 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300', label: 'Concluído' },
    pendente: { color: 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900 dark:text-yellow-300', label: 'Pendente' },
    alerta: { color: 'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-300', label: 'Com Alerta' },
  },
  viagem: {
    em_andamento: { color: 'bg-green-100 text-green-800', label: 'Em Andamento' },

import { getStatusConfig } from '../../utils/statusConfig';

import { useEffect, useState } from 'react';
import { useLocalStorage } from './useLocalStorage';

export function useDarkMode() {
  const [darkMode, setDarkMode] = useLocalStorage('darkMode', false);

  useEffect(() => {
    const root = document.documentElement;
    root.classList.toggle('dark', darkMode);
  }, [darkMode]);

  return [darkMode, setDarkMode];
}
export default function StatusBadge({ type = 'checklist', status }) {
  if (!status) return null;
  const config = getStatusConfig(type, status);
  return (
    <span className={`px-3 py-1 rounded-full text-xs font-medium ${config.color}`}>
      {config.label}
    </span>
  );
}


    programada: { color: 'bg-blue-100 text-blue-800', label: 'Programada' },
    pausada: { color: 'bg-yellow-100 text-yellow-800', label: 'Pausada' },
    finalizada: { color: 'bg-gray-100 text-gray-800', label: 'Finalizada' },
  }
};

export const getStatusConfig = (type, status) => {
  return STATUS_CONFIG[type]?.[status] || STATUS_CONFIG[type]?.finalizada || { color: '', label: 'Desconhecido' };
};

import { Truck, Sun, Moon } from 'lucide-react';
import NavigationItem from './NavigationItem';

const NAV_ITEMS = [
  { title: "Dashboard", url: "dashboard", icon: LayoutDashboard },
  { title: "Novo Checklist", url: "novo-checklist", icon: FilePlus },
  { title: "Histórico", url: "historico", icon: History },
  { title: "Viagens Ativas", url: "rotas", icon: Route },
];

export default function Sidebar({ currentPage, setCurrentPage, darkMode, setDarkMode }) {
  return (
    <aside className="hidden lg:flex flex-col w-64 bg-gradient-to-b from-slate-900 to-slate-900 text-white shadow-2xl">
      <div className="p-6 flex items-center gap-3 border-b border-slate-700/50">
        <div className="p-3 bg-gradient-to-r from-blue-600 to-purple-600 rounded-xl">
          <Truck className="w-6 h-6 text-white" />
        </div>
        <div>
          <h2 className="font-bold text-xl bg-gradient-to-r from-blue-400 to-purple-400 bg-clip-text text-transparent">
            CheckList Pro
          </h2>
          <p className="text-xs text-slate-400">Gestão Inteligente</p>
        </div>
      </div>

      <nav className="flex-1 p-4 space-y-2">
        {NAV_ITEMS.map(item => (
          <NavigationItem
            key={item.url}
            item={item}
            isActive={currentPage === item.url}
            onClick={setCurrentPage}
          />
        ))}
      </nav>

      <div className="p-4 border-t border-slate-700/50">
        <button
          onClick={() => setDarkMode(!darkMode)}
          className="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-slate-300 hover:bg-slate-700/50"
        >
          {darkMode ? <Sun className="w-5 h-5 text-yellow-400" /> : <Moon className="w-5 h-5 text-blue-400" />}
          <span className="font-medium">{darkMode ? 'Modo Claro' : 'Modo Escuro'}</span>
        </button>
      </div>
    </aside>
  );

import { CheckCircle2, AlertTriangle, Route, Truck } from 'lucide-react';
import { mockChecklists } from '../utils/mockData';
import StatusBadge from '../components/common/StatusBadge';
import Card from '../components/ui/Card';

const stats = [
  { title: "Checklists Hoje", value: "12", change: "+8%", icon: CheckCircle2, color: "text-green-600" },
  { title: "Alertas Pendentes", value: "3", change: "-2", icon: AlertTriangle, color: "text-orange-600" },
  { title: "Viagens Ativas", value: "8", change: "+12%", icon: Route, color: "text-blue-600" },
  { title: "Veículos em Operação", value: "45", change: "+5%", icon: Truck, color: "text-purple-600" },
];

export default function Dashboard() {
  return (
    <div className="p-4 lg:p-8">
      <header className="mb-8">
        <h1 className="text-3xl font-bold text-gray-900 dark:text-white mb-2">Dashboard</h1>
        <p className="text-gray-600 dark:text-gray-400">Visão geral das operações</p>
      </header>

      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        {stats.map((stat, i) => (
          <Card key={i} className="p-6">
            <div className="flex justify-between mb-4">
              <div className={`p-3 rounded-lg ${stat.color} bg-opacity-10`}>
                <stat.icon className={`w-6 h-6 ${stat.color}`} />
              </div>
              <span className="text-sm text-green-600 font-medium">{stat.change}</span>
            </div>
            <h3 className="text-2xl font-bold text-gray-900 dark:text-white">{stat.value}</h3>
            <p className="text-sm text-gray-600 dark:text-gray-400">{stat.title}</p>
          </Card>
        ))}
      </div>

      <Card>
        <div className="p-6 border-b border-gray-200 dark:border-gray-700">
          <h2 className="text-xl font-semibold">Checklists Recentes</h2>
        </div>
        <div className="p-6 space-y-4">
          {mockChecklists.slice(0, 3).map(c => (
            <div key={c.id} className="flex items-center justify-between p-4 bg-gray-50 dark:bg-gray-700 rounded-lg">
              <div className="flex items-center gap-4">
                <div className={`w-3 h-3 rounded-full ${c.status === 'concluido' ? 'bg-green-500' : c.status === 'pendente' ? 'bg-yellow-500' : 'bg-red-500'}`} />
                <div>
                  <h3 className="font-medium">{c.titulo}</h3>
                  <p className="text-sm text-gray-600 dark:text-gray-400">{c.veiculo} - {c.motorista}</p>
                </div>
              </div>
              <div className="text-right">
                <p className="font-medium">{c.itensOk}/{c.itens}</p>
                <p className="text-xs text-gray-600 dark:text-gray-400">{c.data}</p>
              </div>
            </div>
          ))}
        </div>
      </Card>
    </div>
  );
}

import { useState } from 'react';
import Layout from './components/layout/Layout';
import Dashboard from './pages/Dashboard';
import NovoChecklist from './pages/NovoChecklist';
import Historico from './pages/Historico';
import ViagensAtivas from './pages/ViagensAtivas';

const PAGES = {
  dashboard: <Dashboard />,
  'novo-checklist': <NovoChecklist />,
  historico: <Historico />,
  rotas: <ViagensAtivas />,
};

export default function App() {
  const [currentPage, setCurrentPage] = useState('dashboard');

  return (
    <Layout currentPage={currentPage} setCurrentPage={setCurrentPage}>
      {PAGES[currentPage] || PAGES.dashboard}
    </Layout>
  );
}

# Criar pastas
mkdir -p src/{components/{layout,ui,checklist,viagem,common},pages,hooks,utils,context}

# Criar arquivos base
touch src/components/ui/{Card,Button,Input,Select,Badge}.jsx
touch src/components/common/{StatusBadge,LoadingSpinner,EmptyState}.jsx
touch src/hooks/useLocalStorage.js
touch src/main.jsx


}
import Sidebar from './Sidebar';
import MobileNav from './MobileNav';
import { useDarkMode } from '../../hooks/useDarkMode';

export default function Layout({ children, currentPage, setCurrentPage }) {
  const [darkMode, setDarkMode] = useDarkMode();

  return (
    <div className={`min-h-screen flex ${darkMode ? 'dark bg-gray-900' : 'bg-gradient-to-br from-gray-50 to-blue-50'}`}>
      <Sidebar currentPage={currentPage} setCurrentPage={setCurrentPage} darkMode={darkMode} setDarkMode={setDarkMode} />
      <div className="flex-1 flex flex-col">
        <main className="flex-1 overflow-y-auto pb-20 lg:pb-0">
          {children}
        </main>
      </div>
      <MobileNav currentPage={currentPage} setCurrentPage={setCurrentPage} />
    </div>
  );
}