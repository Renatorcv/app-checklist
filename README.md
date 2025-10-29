# app-checklist
aplicativo de check list 
import React, { useState, useEffect, useCallback } from 'react';
import { 
  LayoutDashboard, 
  FilePlus, 
  History, 
  Route,
  Truck,
  Moon,
  Sun,
  CheckCircle2,
  AlertTriangle,
  Clock,
  MapPin,
  User,
  Calendar,
  Trash2,
  Edit,
  Eye,
  Plus,
  Search,
  Filter,
  Download,
  Upload,
  Play,
  Pause,
  Flag,
  Navigation
} from "lucide-react";

// Simular dados para demonstração
const mockChecklists = [
  {
    id: 1,
    titulo: "Inspeção Veicular - Caminhão 001",
    veiculo: "Mercedes Actros 2546",
    motorista: "João Silva",
    data: "2024-09-05",
    status: "concluido",
    itens: 45,
    itensOk: 42,
    observacoes: "Pneu traseiro direito precisa ser verificado"
  },
  {
    id: 2,
    titulo: "Check-up Pré-Viagem - Rota SP-RJ",
    veiculo: "Volvo FH 540",
    motorista: "Maria Santos",
    data: "2024-09-04",
    status: "pendente",
    itens: 38,
    itensOk: 35,
    observacoes: "Aguardando aprovação do supervisor"
  },
  {
    id: 3,
    titulo: "Manutenção Preventiva - Frota 15",
    veiculo: "Scania R450",
    motorista: "Pedro Costa",
    data: "2024-09-03",
    status: "alerta",
    itens: 52,
    itensOk: 47,
    observacoes: "Problema no sistema de freios identificado"
  }
];

const mockViagens = [
  {
    id: 1,
    origem: "São Paulo - SP",
    destino: "Rio de Janeiro - RJ",
    motorista: "João Silva",
    veiculo: "Mercedes Actros 2546",
    saida: "2024-09-05 08:00",
    chegada: "2024-09-05 14:30",
    status: "em_andamento",
    km: 429
  },
  {
    id: 2,
    origem: "Belo Horizonte - MG",
    destino: "Salvador - BA",
    motorista: "Carlos Lima",
    veiculo: "Volvo FH 540",
    saida: "2024-09-05 06:00",
    chegada: "2024-09-06 18:00",
    status: "programada",
    km: 1247
  }
];

// Utilitário para criar URLs das páginas
const createPageUrl = (page) => `/${page.toLowerCase().replace(' ', '-')}`;

// Componente de Layout Principal
function Layout({ children, currentPage, setCurrentPage }) {
  const [darkMode, setDarkMode] = useState(false);

  const navigationItems = [
    { title: "Dashboard", url: "dashboard", icon: LayoutDashboard },
    { title: "Novo Checklist", url: "novo-checklist", icon: FilePlus },
    { title: "Histórico", url: "historico", icon: History },
    { title: "Viagens Ativas", url: "rotas", icon: Route },
  ];

  useEffect(() => {
    const savedMode = JSON.parse(localStorage.getItem('darkMode') || 'false');
    setDarkMode(savedMode);
  }, []);

  useEffect(() => {
    if (darkMode) {
      document.documentElement.classList.add('dark');
    } else {
      document.documentElement.classList.remove('dark');
    }
  }, [darkMode]);

  const toggleDarkMode = () => {
    const newMode = !darkMode;
    setDarkMode(newMode);
    localStorage.setItem('darkMode', JSON.stringify(newMode));
  };

  return (
    <div className={`min-h-screen flex transition-all duration-300 ${darkMode ? 'dark bg-gray-900' : 'bg-gradient-to-br from-gray-50 to-blue-50'}`}>
      {/* Desktop Sidebar */}
      <aside className="hidden lg:block w-64 bg-gradient-to-b from-slate-900 via-slate-800 to-slate-900 dark:from-black dark:via-gray-900 dark:to-black text-white flex-col shadow-2xl border-r border-slate-700">
        <div className="p-6 flex items-center gap-3 border-b border-slate-700/50">
          <div className="p-3 bg-gradient-to-r from-blue-600 via-indigo-600 to-purple-600 rounded-xl shadow-lg transform hover:scale-110 transition-all duration-300">
            <Truck className="text-white w-6 h-6"/>
          </div>
          <div>
            <h2 className="font-bold text-xl bg-gradient-to-r from-blue-400 via-indigo-400 to-purple-400 bg-clip-text text-transparent">
              CheckList Pro
            </h2>
            <p className="text-xs text-slate-400">Gestão Inteligente</p>
          </div>
        </div>
        
        <nav className="flex-1 p-4 space-y-2">
          {navigationItems.map((item) => {
            const isActive = currentPage === item.url;
            return (
              <button 
                key={item.title}
                onClick={() => setCurrentPage(item.url)}
                className={`group flex items-center gap-4 px-4 py-3 rounded-xl transition-all duration-300 transform hover:scale-105 w-full text-left ${
                  isActive 
                    ? "bg-gradient-to-r from-blue-600 via-indigo-600 to-purple-600 text-white shadow-lg shadow-blue-500/25" 
                    : "text-slate-300 hover:bg-slate-700/50 hover:text-white"
                }`}
              >
                <item.icon className={`w-5 h-5 transition-transform duration-300 ${isActive ? 'scale-110' : 'group-hover:scale-110'}`}/>
                <span className="font-medium">{item.title}</span>
                {isActive && (
                  <div className="w-2 h-2 bg-white rounded-full ml-auto animate-pulse"></div>
                )}
              </button>
            );
          })}
        </nav>

        <div className="p-4 border-t border-slate-700/50">
          <button
            onClick={toggleDarkMode}
            className="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-slate-300 hover:bg-slate-700/50 hover:text-white transition-all duration-300 transform hover:scale-105"
          >
            {darkMode ? (
              <Sun className="w-5 h-5 text-yellow-400" />
            ) : (
              <Moon className="w-5 h-5 text-blue-400" />
            )}
            <span className="font-medium">{darkMode ? 'Modo Claro' : 'Modo Escuro'}</span>
          </button>
        </div>
      </aside>

      <div className="flex-1 flex flex-col">
        <main className={`flex-1 overflow-y-auto transition-colors duration-300 ${darkMode ? 'bg-gray-900' : 'bg-gradient-to-br from-gray-50 to-blue-50'} pb-20 lg:pb-0`}>
          {children}
        </main>
      </div>

      {/* Mobile Bottom Navigation */}
      <nav className="lg:hidden fixed bottom-0 left-0 right-0 bg-white/95 dark:bg-gray-900/95 backdrop-blur-lg border-t border-gray-200/50 dark:border-gray-700/50 z-50 shadow-2xl">
        <div className="grid grid-cols-4 py-2">
          {navigationItems.map((item) => {
            const isActive = currentPage === item.url;
            return (
              <button 
                key={item.title} 
                onClick={() => setCurrentPage(item.url)}
                className={`group flex flex-col items-center p-3 transition-all duration-300 transform hover:scale-110 ${
                  isActive 
                    ? "text-blue-600 dark:text-blue-400" 
                    : "text-gray-500 dark:text-gray-400 hover:text-blue-600 dark:hover:text-blue-400"
                }`}
              >
                <div className={`relative p-2 rounded-lg transition-all duration-300 ${
                  isActive 
                    ? 'bg-gradient-to-r from-blue-100 to-indigo-100 dark:from-blue-900/50 dark:to-indigo-900/50 shadow-lg' 
                    : 'group-hover:bg-blue-50 dark:group-hover:bg-blue-900/30'
                }`}>
                  <item.icon className="w-5 h-5"/>
                  {isActive && (
                    <div className="absolute -top-1 -right-1 w-3 h-3 bg-gradient-to-r from-blue-500 to-indigo-500 rounded-full animate-pulse"></div>
                  )}
                </div>
                <span className="text-xs font-medium mt-1 text-center leading-tight">
                  {item.title.split(' ').map((word, i) => (
                    <div key={i}>{word}</div>
                  ))}
                </span>
              </button>
            );
          })}
        </div>
        <div className="h-1 bg-gradient-to-r from-transparent via-gray-300 to-transparent dark:via-gray-600"></div>
      </nav>
    </div>
  );
}

// Componente Dashboard
function Dashboard() {
  const stats = [
    { title: "Checklists Hoje", value: "12", change: "+8%", icon: CheckCircle2, color: "text-green-600" },
    { title: "Alertas Pendentes", value: "3", change: "-2", icon: AlertTriangle, color: "text-orange-600" },
    { title: "Viagens Ativas", value: "8", change: "+12%", icon: Route, color: "text-blue-600" },
    { title: "Veículos em Operação", value: "45", change: "+5%", icon: Truck, color: "text-purple-600" },
  ];

  return (
    <div className="p-4 lg:p-8">
      <div className="mb-8">
        <h1 className="text-3xl font-bold text-gray-900 dark:text-white mb-2">Dashboard</h1>
        <p className="text-gray-600 dark:text-gray-400">Visão geral das operações</p>
      </div>

      {/* Stats Cards */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        {stats.map((stat, index) => (
          <div key={index} className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg hover:shadow-xl transition-all duration-300 border border-gray-200 dark:border-gray-700">
            <div className="flex items-center justify-between mb-4">
              <div className={`p-3 rounded-lg ${stat.color} bg-opacity-10`}>
                <stat.icon className={`w-6 h-6 ${stat.color}`} />
              </div>
              <span className="text-sm text-green-600 font-medium">{stat.change}</span>
            </div>
            <h3 className="text-2xl font-bold text-gray-900 dark:text-white mb-1">{stat.value}</h3>
            <p className="text-gray-600 dark:text-gray-400 text-sm">{stat.title}</p>
          </div>
        ))}
      </div>

      {/* Recent Checklists */}
      <div className="bg-white dark:bg-gray-800 rounded-xl shadow-lg border border-gray-200 dark:border-gray-700">
        <div className="p-6 border-b border-gray-200 dark:border-gray-700">
          <h2 className="text-xl font-semibold text-gray-900 dark:text-white">Checklists Recentes</h2>
        </div>
        <div className="p-6">
          <div className="space-y-4">
            {mockChecklists.slice(0, 3).map((checklist) => (
              <div key={checklist.id} className="flex items-center justify-between p-4 bg-gray-50 dark:bg-gray-700 rounded-lg">
                <div className="flex items-center gap-4">
                  <div className={`w-3 h-3 rounded-full ${
                    checklist.status === 'concluido' ? 'bg-green-500' :
                    checklist.status === 'pendente' ? 'bg-yellow-500' : 'bg-red-500'
                  }`}></div>
                  <div>
                    <h3 className="font-medium text-gray-900 dark:text-white">{checklist.titulo}</h3>
                    <p className="text-sm text-gray-600 dark:text-gray-400">{checklist.veiculo} - {checklist.motorista}</p>
                  </div>
                </div>
                <div className="text-right">
                  <p className="text-sm font-medium text-gray-900 dark:text-white">{checklist.itensOk}/{checklist.itens}</p>
                  <p className="text-xs text-gray-600 dark:text-gray-400">{checklist.data}</p>
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );
}

// Componente Novo Checklist
function NovoChecklist() {
  const [formData, setFormData] = useState({
    titulo: '',
    veiculo: '',
    motorista: '',
    tipoChecklist: 'pre-viagem'
  });

  const checklistItems = [
    { id: 1, item: "Verificar nível de óleo do motor", categoria: "Motor" },
    { id: 2, item: "Verificar pressão dos pneus", categoria: "Pneus" },
    { id: 3, item: "Testar funcionamento dos freios", categoria: "Freios" },
    { id: 4, item: "Verificar luzes e sinalização", categoria: "Elétrica" },
    { id: 5, item: "Verificar documentação do veículo", categoria: "Documentos" },
    { id: 6, item: "Verificar nível de combustível", categoria: "Combustível" },
    { id: 7, item: "Verificar estado dos espelhos", categoria: "Carroceria" },
    { id: 8, item: "Testar buzina", categoria: "Elétrica" }
  ];

  const [checkedItems, setCheckedItems] = useState({});

  const handleSubmit = (e) => {
    e.preventDefault();
    alert('Checklist criado com sucesso!');
  };

  return (
    <div className="p-4 lg:p-8">
      <div className="mb-8">
        <h1 className="text-3xl font-bold text-gray-900 dark:text-white mb-2">Novo Checklist</h1>
        <p className="text-gray-600 dark:text-gray-400">Criar nova inspeção veicular</p>
      </div>

      <form onSubmit={handleSubmit} className="space-y-6">
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700">
          <h2 className="text-xl font-semibold text-gray-900 dark:text-white mb-6">Informações Gerais</h2>
          
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div>
              <label className="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
                Título do Checklist
              </label>
              <input
                type="text"
                value={formData.titulo}
                onChange={(e) => setFormData({...formData, titulo: e.target.value})}
                className="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent bg-white dark:bg-gray-700 text-gray-900 dark:text-white"
                placeholder="Ex: Inspeção Pré-Viagem"
                required
              />
            </div>
            
            <div>
              <label className="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
                Veículo
              </label>
              <input
                type="text"
                value={formData.veiculo}
                onChange={(e) => setFormData({...formData, veiculo: e.target.value})}
                className="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent bg-white dark:bg-gray-700 text-gray-900 dark:text-white"
                placeholder="Ex: Mercedes Actros 2546"
                required
              />
            </div>
            
            <div>
              <label className="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
                Motorista
              </label>
              <input
                type="text"
                value={formData.motorista}
                onChange={(e) => setFormData({...formData, motorista: e.target.value})}
                className="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent bg-white dark:bg-gray-700 text-gray-900 dark:text-white"
                placeholder="Nome do motorista"
                required
              />
            </div>
            
            <div>
              <label className="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
                Tipo de Checklist
              </label>
              <select
                value={formData.tipoChecklist}
                onChange={(e) => setFormData({...formData, tipoChecklist: e.target.value})}
                className="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent bg-white dark:bg-gray-700 text-gray-900 dark:text-white"
              >
                <option value="pre-viagem">Pré-Viagem</option>
                <option value="pos-viagem">Pós-Viagem</option>
                <option value="manutencao">Manutenção</option>
                <option value="inspecao">Inspeção Geral</option>
              </select>
            </div>
          </div>
        </div>

        {/* Itens do Checklist */}
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700">
          <h2 className="text-xl font-semibold text-gray-900 dark:text-white mb-6">Itens de Verificação</h2>
          
          <div className="space-y-4">
            {checklistItems.map((item) => (
              <div key={item.id} className="flex items-center gap-4 p-4 bg-gray-50 dark:bg-gray-700 rounded-lg">
                <input
                  type="checkbox"
                  id={`item-${item.id}`}
                  checked={checkedItems[item.id] || false}
                  onChange={(e) => setCheckedItems({...checkedItems, [item.id]: e.target.checked})}
                  className="w-5 h-5 text-blue-600 bg-gray-100 border-gray-300 rounded focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-600 dark:border-gray-500"
                />
                <div className="flex-1">
                  <label htmlFor={`item-${item.id}`} className="text-gray-900 dark:text-white font-medium cursor-pointer">
                    {item.item}
                  </label>
                  <p className="text-sm text-gray-600 dark:text-gray-400">{item.categoria}</p>
                </div>
              </div>
            ))}
          </div>
        </div>

        <div className="flex gap-4">
          <button
            type="submit"
            className="px-6 py-3 bg-gradient-to-r from-blue-600 to-indigo-600 text-white rounded-lg font-medium hover:from-blue-700 hover:to-indigo-700 transition-all duration-300 transform hover:scale-105 shadow-lg"
          >
            Salvar Checklist
          </button>
          <button
            type="button"
            className="px-6 py-3 bg-gray-300 dark:bg-gray-600 text-gray-700 dark:text-gray-300 rounded-lg font-medium hover:bg-gray-400 dark:hover:bg-gray-500 transition-all duration-300"
          >
            Cancelar
          </button>
        </div>
      </form>
    </div>
  );
}

// Componente Histórico
function Historico() {
  const [searchTerm, setSearchTerm] = useState('');
  const [filterStatus, setFilterStatus] = useState('todos');

  const filteredChecklists = mockChecklists.filter(checklist => {
    const matchesSearch = checklist.titulo.toLowerCase().includes(searchTerm.toLowerCase()) ||
                         checklist.motorista.toLowerCase().includes(searchTerm.toLowerCase()) ||
                         checklist.veiculo.toLowerCase().includes(searchTerm.toLowerCase());
    const matchesStatus = filterStatus === 'todos' || checklist.status === filterStatus;
    return matchesSearch && matchesStatus;
  });

  const getStatusColor = (status) => {
    switch (status) {
      case 'concluido': return 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300';
      case 'pendente': return 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900 dark:text-yellow-300';
      case 'alerta': return 'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-300';
      default: return 'bg-gray-100 text-gray-800 dark:bg-gray-900 dark:text-gray-300';
    }
  };

  const getStatusText = (status) => {
    switch (status) {
      case 'concluido': return 'Concluído';
      case 'pendente': return 'Pendente';
      case 'alerta': return 'Com Alerta';
      default: return 'Desconhecido';
    }
  };

  return (
    <div className="p-4 lg:p-8">
      <div className="mb-8">
        <h1 className="text-3xl font-bold text-gray-900 dark:text-white mb-2">Histórico</h1>
        <p className="text-gray-600 dark:text-gray-400">Consultar checklists anteriores</p>
      </div>

      {/* Filtros */}
      <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700 mb-6">
        <div className="flex flex-col lg:flex-row gap-4">
          <div className="flex-1">
            <div className="relative">
              <Search className="w-5 h-5 absolute left-3 top-3 text-gray-400" />
              <input
                type="text"
                placeholder="Buscar por título, motorista ou veículo..."
                value={searchTerm}
                onChange={(e) => setSearchTerm(e.target.value)}
                className="w-full pl-10 pr-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent bg-white dark:bg-gray-700 text-gray-900 dark:text-white"
              />
            </div>
          </div>
          
          <div className="flex gap-4">
            <select
              value={filterStatus}
              onChange={(e) => setFilterStatus(e.target.value)}
              className="px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent bg-white dark:bg-gray-700 text-gray-900 dark:text-white"
            >
              <option value="todos">Todos os Status</option>
              <option value="concluido">Concluído</option>
              <option value="pendente">Pendente</option>
              <option value="alerta">Com Alerta</option>
            </select>
            
            <button className="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors duration-300 flex items-center gap-2">
              <Download className="w-4 h-4" />
              Exportar
            </button>
          </div>
        </div>
      </div>

      {/* Lista de Checklists */}
      <div className="space-y-4">
        {filteredChecklists.map((checklist) => (
          <div key={checklist.id} className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700 hover:shadow-xl transition-all duration-300">
            <div className="flex flex-col lg:flex-row lg:items-center justify-between gap-4">
              <div className="flex-1">
                <div className="flex items-center gap-3 mb-3">
                  <h3 className="text-xl font-semibold text-gray-900 dark:text-white">{checklist.titulo}</h3>
                  <span className={`px-3 py-1 rounded-full text-xs font-medium ${getStatusColor(checklist.status)}`}>
                    {getStatusText(checklist.status)}
                  </span>
                </div>
                
                <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 text-sm text-gray-600 dark:text-gray-400">
                  <div className="flex items-center gap-2">
                    <Truck className="w-4 h-4" />
                    {checklist.veiculo}
                  </div>
                  <div className="flex items-center gap-2">
                    <User className="w-4 h-4" />
                    {checklist.motorista}
                  </div>
                  <div className="flex items-center gap-2">
                    <Calendar className="w-4 h-4" />
                    {checklist.data}
                  </div>
                  <div className="flex items-center gap-2">
                    <CheckCircle2 className="w-4 h-4" />
                    {checklist.itensOk}/{checklist.itens} itens
                  </div>
                </div>
                
                {checklist.observacoes && (
                  <div className="mt-3 p-3 bg-gray-50 dark:bg-gray-700 rounded-lg">
                    <p className="text-sm text-gray-700 dark:text-gray-300">{checklist.observacoes}</p>
                  </div>
                )}
              </div>
              
              <div className="flex gap-2">
                <button className="p-2 text-blue-600 hover:bg-blue-50 dark:hover:bg-blue-900/20 rounded-lg transition-colors duration-300">
                  <Eye className="w-4 h-4" />
                </button>
                <button className="p-2 text-green-600 hover:bg-green-50 dark:hover:bg-green-900/20 rounded-lg transition-colors duration-300">
                  <Edit className="w-4 h-4" />
                </button>
                <button className="p-2 text-red-600 hover:bg-red-50 dark:hover:bg-red-900/20 rounded-lg transition-colors duration-300">
                  <Trash2 className="w-4 h-4" />
                </button>
              </div>
            </div>
          </div>
        ))}
      </div>

      {filteredChecklists.length === 0 && (
        <div className="text-center py-12">
          <p className="text-gray-500 dark:text-gray-400">Nenhum checklist encontrado com os filtros aplicados.</p>
        </div>
      )}
    </div>
  );
}

// Componente de Mapa Simulado
function MapaSimulado({ viagem, onPositionUpdate }) {
  const [currentPosition, setCurrentPosition] = useState(null);
  const [route, setRoute] = useState([]);

  useEffect(() => {
    // Simular movimento do veículo
    if (viagem && viagem.status === 'em_andamento') {
      const interval = setInterval(() => {
        const newLat = -23.5505 + (Math.random() - 0.5) * 0.01;
        const newLng = -46.6333 + (Math.random() - 0.5) * 0.01;
        const newPoint = { lat: newLat, lng: newLng, timestamp: new Date().toISOString() };
        
        setCurrentPosition(newPoint);
        setRoute(prev => [...prev, newPoint]);
        onPositionUpdate && onPositionUpdate(newPoint);
      }, 3000);

      return () => clearInterval(interval);
    }
  }, [viagem, onPositionUpdate]);

  return (
    <div className="w-full h-full bg-gradient-to-br from-blue-50 to-green-50 dark:from-gray-800 dark:to-gray-900 rounded-lg flex items-center justify-center relative overflow-hidden">
      {/* Simulação de mapa com grid */}
      <div className="absolute inset-0 opacity-20">
        <div className="grid grid-cols-20 grid-rows-20 h-full w-full">
          {Array.from({ length: 400 }).map((_, i) => (
            <div key={i} className="border border-gray-300 dark:border-gray-600"></div>
          ))}
        </div>
      </div>
      
      {/* Marcador do veículo */}
      {currentPosition && (
        <div className="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-10">
          <div className="flex flex-col items-center animate-bounce">
            <div className="bg-red-600 text-white p-3 rounded-full shadow-lg">
              <Truck className="w-6 h-6" />
            </div>
            <div className="bg-white dark:bg-gray-800 px-3 py-1 rounded-lg shadow-md mt-2 text-sm font-medium">
              Posição Atual
            </div>
          </div>
        </div>
      )}
      
      {/* Rota simulada */}
      <div className="absolute inset-0 z-5">
        <svg className="w-full h-full">
          <path
            d="M 100 300 Q 200 100 300 200 T 500 250"
            stroke="#3b82f6"
            strokeWidth="4"
            fill="none"
            strokeDasharray="10,5"
            className="animate-pulse"
          />
        </svg>
      </div>
      
      {/* Informações do mapa */}
      <div className="absolute top-4 left-4 bg-white dark:bg-gray-800 p-4 rounded-lg shadow-lg">
        <div className="flex items-center gap-2 text-sm">
          <div className="w-3 h-3 bg-green-500 rounded-full animate-pulse"></div>
          <span className="text-gray-700 dark:text-gray-300">GPS Ativo</span>
        </div>
        <div className="text-xs text-gray-500 dark:text-gray-400 mt-1">
          Atualização a cada 3s
        </div>
      </div>
    </div>
  );
}

// Componente de Rastreamento de Viagem
function RastreamentoViagem({ viagemId, onVoltar }) {
  const [viagem, setViagem] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [elapsedTime, setElapsedTime] = useState(0);
  const [distanciaPercorrida, setDistanciaPercorrida] = useState(0);

  useEffect(() => {
    // Simular carregamento de dados da viagem
    setTimeout(() => {
      const viagemMock = {
        id: viagemId,
        placa_veiculo: "ABC-1234",
        nome_motorista: "João Silva",
        origem: "São Paulo - SP",
        destino: "Rio de Janeiro - RJ",
        status: "em_andamento",
        data_inicio: new Date(Date.now() - 3600000).toISOString(), // 1 hora atrás
        distancia_total: 429,
        trajeto: []
      };
      setViagem(viagemMock);
      setIsLoading(false);
    }, 1000);
  }, [viagemId]);

  useEffect(() => {
    let timer;
    if (viagem?.status === 'em_andamento') {
      const startTime = new Date(viagem.data_inicio).getTime();
      timer = setInterval(() => {
        setElapsedTime(Date.now() - startTime);
      }, 1000);
    }
    return () => clearInterval(timer);
  }, [viagem]);

  const handlePositionUpdate = useCallback((position) => {
    setDistanciaPercorrida(prev => prev + 0.5); // Incrementar distância simulada
  }, []);

  const handleTogglePause = () => {
    const newStatus = viagem.status === 'em_andamento' ? 'pausada' : 'em_andamento';
    setViagem(prev => ({ ...prev, status: newStatus }));
    
    if ('speechSynthesis' in window) {
      const text = newStatus === 'pausada' ? 'Viagem pausada' : 'Viagem retomada';
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = 'pt-BR';
      window.speechSynthesis.speak(utterance);
    }
  };

  const handleFinishTrip = () => {
    setViagem(prev => ({ 
      ...prev, 
      status: 'finalizada',
      data_fim: new Date().toISOString() 
    }));
    
    if ('speechSynthesis' in window) {
      const utterance = new SpeechSynthesisUtterance("Viagem concluída");
      utterance.lang = 'pt-BR';
      window.speechSynthesis.speak(utterance);
    }
  };

  const formatElapsedTime = (ms) => {
    const totalSeconds = Math.floor(ms / 1000);
    const hours = Math.floor(totalSeconds / 3600);
    const minutes = Math.floor((totalSeconds % 3600) / 60);
    const seconds = totalSeconds % 60;
    return `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
  };

  if (isLoading) {
    return (
      <div className="flex justify-center items-center h-full">
        <div className="text-center">
          <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-600 mx-auto mb-4"></div>
          <p className="text-gray-600 dark:text-gray-400">Carregando dados da viagem...</p>
        </div>
      </div>
    );
  }

  if (!viagem) {
    return (
      <div className="p-6 text-center">
        <h2 className="text-xl font-semibold text-red-600">Viagem não encontrada</h2>
        <button 
          onClick={onVoltar}
          className="mt-4 px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors duration-300"
        >
          Voltar
        </button>
      </div>
    );
  }

  const getStatusColor = (status) => {
    switch (status) {
      case 'em_andamento': return 'text-green-600 bg-green-100 dark:bg-green-900/20';
      case 'pausada': return 'text-yellow-600 bg-yellow-100 dark:bg-yellow-900/20';
      case 'finalizada': return 'text-gray-600 bg-gray-100 dark:bg-gray-900/20';
      default: return 'text-gray-600 bg-gray-100 dark:bg-gray-900/20';
    }
  };

  const getStatusText = (status) => {
    switch (status) {
      case 'em_andamento': return 'Em Andamento';
      case 'pausada': return 'Pausada';
      case 'finalizada': return 'Finalizada';
      default: return 'Desconhecido';
    }
  };

  return (
    <div className="p-4 lg:p-8">
      {/* Header */}
      <div className="mb-8">
        <div className="flex items-center gap-4 mb-4">
          <button 
            onClick={onVoltar}
            className="p-2 bg-gray-200 dark:bg-gray-700 rounded-lg hover:bg-gray-300 dark:hover:bg-gray-600 transition-colors duration-300"
          >
            ←
          </button>
          <div>
            <h1 className="text-3xl font-bold text-gray-900 dark:text-white">Rastreamento de Viagem</h1>
            <p className="text-gray-600 dark:text-gray-400">
              {viagem.placa_veiculo} | {viagem.nome_motorista}
            </p>
          </div>
        </div>
        
        <div className="flex items-center gap-3">
          <span className={`px-4 py-2 rounded-full text-sm font-medium ${getStatusColor(viagem.status)}`}>
            {getStatusText(viagem.status)}
          </span>
          <div className="text-sm text-gray-600 dark:text-gray-400">
            {viagem.origem} → {viagem.destino}
          </div>
        </div>
      </div>

      <div className="grid grid-cols-1 xl:grid-cols-3 gap-8">
        {/* Mapa */}
        <div className="xl:col-span-2">
          <div className="bg-white dark:bg-gray-800 rounded-xl shadow-lg border border-gray-200 dark:border-gray-700 overflow-hidden">
            <div className="h-96 lg:h-[500px]">
              <MapaSimulado viagem={viagem} onPositionUpdate={handlePositionUpdate} />
            </div>
          </div>
        </div>

        {/* Painel de Controle */}
        <div className="space-y-6">
          {/* Estatísticas */}
          <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700">
            <h2 className="text-xl font-semibold text-gray-900 dark:text-white mb-6">Estatísticas da Viagem</h2>
            
            <div className="space-y-4">
              <div className="flex items-center justify-between">
                <div className="flex items-center gap-3">
                  <Clock className="w-5 h-5 text-blue-600" />
                  <span className="text-gray-700 dark:text-gray-300">Tempo Decorrido</span>
                </div>
                <span className="font-bold text-gray-900 dark:text-white">
                  {formatElapsedTime(elapsedTime)}
                </span>
              </div>
              
              <div className="flex items-center justify-between">
                <div className="flex items-center gap-3">
                  <Route className="w-5 h-5 text-green-600" />
                  <span className="text-gray-700 dark:text-gray-300">Distância Percorrida</span>
                </div>
                <span className="font-bold text-gray-900 dark:text-white">
                  {distanciaPercorrida.toFixed(1)} km
                </span>
              </div>
              
              <div className="flex items-center justify-between">
                <div className="flex items-center gap-3">
                  <MapPin className="w-5 h-5 text-purple-600" />
                  <span className="text-gray-700 dark:text-gray-300">Distância Total</span>
                </div>
                <span className="font-bold text-gray-900 dark:text-white">
                  {viagem.distancia_total} km
                </span>
              </div>
              
              <div className="mt-4">
                <div className="flex justify-between text-sm text-gray-600 dark:text-gray-400 mb-2">
                  <span>Progresso</span>
                  <span>{Math.min(100, (distanciaPercorrida / viagem.distancia_total * 100)).toFixed(1)}%</span>
                </div>
                <div className="w-full bg-gray-200 dark:bg-gray-700 rounded-full h-2">
                  <div 
                    className="bg-gradient-to-r from-blue-600 to-green-600 h-2 rounded-full transition-all duration-300"
                    style={{ width: `${Math.min(100, distanciaPercorrida / viagem.distancia_total * 100)}%` }}
                  ></div>
                </div>
              </div>
            </div>
          </div>

          {/* Controles */}
          {viagem.status !== 'finalizada' && (
            <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700">
              <h2 className="text-xl font-semibold text-gray-900 dark:text-white mb-6">Controles da Viagem</h2>
              
              <div className="space-y-4">
                <button
                  onClick={handleTogglePause}
                  className={`w-full flex items-center justify-center gap-3 px-6 py-3 rounded-lg font-medium transition-all duration-300 ${
                    viagem.status === 'em_andamento' 
                      ? 'bg-yellow-600 hover:bg-yellow-700 text-white' 
                      : 'bg-green-600 hover:bg-green-700 text-white'
                  }`}
                >
                  {viagem.status === 'em_andamento' ? (
                    <>
                      <Pause className="w-5 h-5" />
                      Pausar Viagem
                    </>
                  ) : (
                    <>
                      <Play className="w-5 h-5" />
                      Retomar Viagem
                    </>
                  )}
                </button>
                
                <button
                  onClick={handleFinishTrip}
                  className="w-full flex items-center justify-center gap-3 px-6 py-3 bg-red-600 hover:bg-red-700 text-white rounded-lg font-medium transition-all duration-300"
                >
                  <Flag className="w-5 h-5" />
                  Finalizar Viagem
                </button>
              </div>
            </div>
          )}

          {/* Informações Adicionais */}
          <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700">
            <h2 className="text-xl font-semibold text-gray-900 dark:text-white mb-4">Informações</h2>
            
            <div className="space-y-3 text-sm">
              <div className="flex justify-between">
                <span className="text-gray-600 dark:text-gray-400">Início da Viagem:</span>
                <span className="font-medium text-gray-900 dark:text-white">
                  {new Date(viagem.data_inicio).toLocaleString('pt-BR')}
                </span>
              </div>
              
              {viagem.data_fim && (
                <div className="flex justify-between">
                  <span className="text-gray-600 dark:text-gray-400">Fim da Viagem:</span>
                  <span className="font-medium text-gray-900 dark:text-white">
                    {new Date(viagem.data_fim).toLocaleString('pt-BR')}
                  </span>
                </div>
              )}
              
              <div className="flex justify-between">
                <span className="text-gray-600 dark:text-gray-400">Velocidade Média:</span>
                <span className="font-medium text-gray-900 dark:text-white">
                  {elapsedTime > 0 ? (distanciaPercorrida / (elapsedTime / 3600000)).toFixed(1) : '0'} km/h
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

// Componente Viagens Ativas
function ViagensAtivas() {
  const [viagemSelecionada, setViagemSelecionada] = useState(null);

  if (viagemSelecionada) {
    return (
      <RastreamentoViagem 
        viagemId={viagemSelecionada} 
        onVoltar={() => setViagemSelecionada(null)} 
      />
    );
  }

  const getStatusColor = (status) => {
    switch (status) {
      case 'em_andamento': return 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300';
      case 'programada': return 'bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-300';
      case 'finalizada': return 'bg-gray-100 text-gray-800 dark:bg-gray-900 dark:text-gray-300';
      default: return 'bg-gray-100 text-gray-800 dark:bg-gray-900 dark:text-gray-300';
    }
  };

  const getStatusText = (status) => {
    switch (status) {
      case 'em_andamento': return 'Em Andamento';
      case 'programada': return 'Programada';
      case 'finalizada': return 'Finalizada';
      default: return 'Desconhecido';
    }
  };

  return (
    <div className="p-4 lg:p-8">
      <div className="mb-8">
        <h1 className="text-3xl font-bold text-gray-900 dark:text-white mb-2">Viagens Ativas</h1>
        <p className="text-gray-600 dark:text-gray-400">Monitorar rotas em andamento</p>
      </div>

      {/* Stats Cards */}
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700">
          <div className="flex items-center gap-3 mb-4">
            <div className="p-3 bg-green-100 dark:bg-green-900/20 rounded-lg">
              <Route className="w-6 h-6 text-green-600" />
            </div>
            <div>
              <h3 className="text-2xl font-bold text-gray-900 dark:text-white">8</h3>
              <p className="text-gray-600 dark:text-gray-400 text-sm">Em Andamento</p>
            </div>
          </div>
        </div>
        
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700">
          <div className="flex items-center gap-3 mb-4">
            <div className="p-3 bg-blue-100 dark:bg-blue-900/20 rounded-lg">
              <Clock className="w-6 h-6 text-blue-600" />
            </div>
            <div>
              <h3 className="text-2xl font-bold text-gray-900 dark:text-white">12</h3>
              <p className="text-gray-600 dark:text-gray-400 text-sm">Programadas</p>
            </div>
          </div>
        </div>
        
        <div className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700">
          <div className="flex items-center gap-3 mb-4">
            <div className="p-3 bg-purple-100 dark:bg-purple-900/20 rounded-lg">
              <MapPin className="w-6 h-6 text-purple-600" />
            </div>
            <div>
              <h3 className="text-2xl font-bold text-gray-900 dark:text-white">2.847</h3>
              <p className="text-gray-600 dark:text-gray-400 text-sm">KM Hoje</p>
            </div>
          </div>
        </div>
      </div>

      {/* Lista de Viagens */}
      <div className="space-y-4">
        {mockViagens.map((viagem) => (
          <div key={viagem.id} className="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-lg border border-gray-200 dark:border-gray-700 hover:shadow-xl transition-all duration-300">
            <div className="flex flex-col lg:flex-row lg:items-center justify-between gap-4">
              <div className="flex-1">
                <div className="flex items-center gap-3 mb-4">
                  <span className={`px-3 py-1 rounded-full text-xs font-medium ${getStatusColor(viagem.status)}`}>
                    {getStatusText(viagem.status)}
                  </span>
                </div>
                
                <div className="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-4">
                  <div className="space-y-3">
                    <div className="flex items-center gap-3">
                      <MapPin className="w-4 h-4 text-green-600" />
                      <div>
                        <p className="text-xs text-gray-500 dark:text-gray-400">ORIGEM</p>
                        <p className="font-semibold text-gray-900 dark:text-white">{viagem.origem}</p>
                      </div>
                    </div>
                    
                    <div className="flex items-center gap-3">
                      <MapPin className="w-4 h-4 text-red-600" />
                      <div>
                        <p className="text-xs text-gray-500 dark:text-gray-400">DESTINO</p>
                        <p className="font-semibold text-gray-900 dark:text-white">{viagem.destino}</p>
                      </div>
                    </div>
                  </div>
                  
                  <div className="space-y-3">
                    <div className="flex items-center gap-3">
                      <User className="w-4 h-4 text-blue-600" />
                      <div>
                        <p className="text-xs text-gray-500 dark:text-gray-400">MOTORISTA</p>
                        <p className="font-semibold text-gray-900 dark:text-white">{viagem.motorista}</p>
                      </div>
                    </div>
                    
                    <div className="flex items-center gap-3">
                      <Truck className="w-4 h-4 text-purple-600" />
                      <div>
                        <p className="text-xs text-gray-500 dark:text-gray-400">VEÍCULO</p>
                        <p className="font-semibold text-gray-900 dark:text-white">{viagem.veiculo}</p>
                      </div>
                    </div>
                  </div>
                </div>
                
                <div className="flex flex-wrap gap-4 text-sm text-gray-600 dark:text-gray-400">
                  <div className="flex items-center gap-2">
                    <Clock className="w-4 h-4" />
                    Saída: {viagem.saida}
                  </div>
                  <div className="flex items-center gap-2">
                    <Clock className="w-4 h-4" />
                    Chegada: {viagem.chegada}
                  </div>
                  <div className="flex items-center gap-2">
                    <Route className="w-4 h-4" />
                    {viagem.km} km
                  </div>
                </div>
              </div>
              
              <div className="flex gap-2">
                <button 
                  onClick={() => setViagemSelecionada(viagem.id)}
                  className="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors duration-300 text-sm"
                >
                  Acompanhar
                </button>
              </div>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}

// Componente Principal da Aplicação
function CheckListProApp() {
  const [currentPage, setCurrentPage] = useState('dashboard');

  const renderCurrentPage = () => {
    switch (currentPage) {
      case 'dashboard':
        return <Dashboard />;
      case 'novo-checklist':
        return <NovoChecklist />;
      case 'historico':
        return <Historico />;
      case 'rotas':
        return <ViagensAtivas />;
      default:
        return <Dashboard />;
    }
  };

  return (
    <Layout currentPage={currentPage} setCurrentPage={setCurrentPage}>
      {renderCurrentPage()}
    </Layout>
  );
}

export default CheckListProApp

