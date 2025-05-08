# eco-smart-lixeira
import React, { useState, useEffect } from 'react';

const EcoSmartApp = () => {
  const [capacidade, setCapacidade] = useState(0);
  const [mensagem, setMensagem] = useState('');
  const [historico, setHistorico] = useState([]);
  const [mostrarHistorico, setMostrarHistorico] = useState(false);
  
  // Atualiza a mensagem com base na capacidade
  useEffect(() => {
    if (capacidade === 100) {
      setMensagem('Atenção: Lixeira cheia! Favor esvaziar.');
    } else if (capacidade >= 75) {
      setMensagem('Lixeira quase cheia.');
    } else if (capacidade >= 50) {
      setMensagem('Lixeira na metade da capacidade.');
    } else if (capacidade >= 25) {
      setMensagem('Lixeira com capacidade disponível.');
    } else {
      setMensagem('Lixeira vazia.');
    }
  }, [capacidade]);
  
  // Simula o descarte de uma bateria
  const descartarBateria = () => {
    if (capacidade < 100) {
      const novaCapacidade = Math.min(100, capacidade + 5);
      setCapacidade(novaCapacidade);
      
      // Adiciona ao histórico
      const agora = new Date();
      const registroHora = agora.toLocaleTimeString();
      const novoRegistro = {
        id: Date.now(),
        hora: registroHora,
        acao: 'Bateria descartada',
        capacidade: `${novaCapacidade}%`
      };
      
      setHistorico([novoRegistro, ...historico]);
    }
  };
  
  // Esvazia a lixeira
  const esvaziarLixeira = () => {
    setCapacidade(0);
    
    // Adiciona ao histórico
    const agora = new Date();
    const registroHora = agora.toLocaleTimeString();
    const novoRegistro = {
      id: Date.now(),
      hora: registroHora,
      acao: 'Lixeira esvaziada',
      capacidade: '0%'
    };
    
    setHistorico([novoRegistro, ...historico]);
  };
  
  // Calcula a cor de fundo baseada na capacidade
  const getBarColor = () => {
    if (capacidade >= 75) return 'bg-red-500';
    if (capacidade >= 50) return 'bg-orange-500';
    if (capacidade >= 25) return 'bg-yellow-500';
    return 'bg-green-500';
  };
  
  return (
    <div className="flex flex-col items-center justify-center min-h-screen bg-gray-100 p-4">
      <div className="w-full max-w-md bg-white rounded-lg shadow-md p-6">
        <div className="text-center mb-6">
          <h1 className="text-3xl font-bold text-green-600">EcoSmart</h1>
          <p className="text-gray-600">Lixeira Inteligente para Baterias</p>
        </div>
        
        <div className="mb-6">
          <div className="text-center mb-2">
            <span className="text-2xl font-bold">{capacidade}% Cheia</span>
          </div>
          
          <div className="w-full bg-gray-200 rounded-full h-6 mb-2">
            <div 
              className={`${getBarColor()} h-full rounded-full transition-all duration-500 ease-in-out`} 
              style={{ width: `${capacidade}%` }}
            ></div>
          </div>
          
          <div className="text-center text-sm font-medium">
            <p className={capacidade >= 75 ? "text-red-600" : "text-gray-600"}>
              {mensagem}
            </p>
          </div>
        </div>
        
        <div className="flex justify-center gap-4 mb-6">
          <button
            onClick={descartarBateria}
            disabled={capacidade >= 100}
            className={`px-4 py-2 rounded-md ${
              capacidade >= 100
                ? "bg-gray-400 cursor-not-allowed"
                : "bg-blue-500 hover:bg-blue-600"
            } text-white font-medium transition`}
          >
            Descartar Bateria
          </button>
          
          <button
            onClick={esvaziarLixeira}
            disabled={capacidade === 0}
            className={`px-4 py-2 rounded-md ${
              capacidade === 0
                ? "bg-gray-400 cursor-not-allowed"
                : "bg-red-500 hover:bg-red-600"
            } text-white font-medium transition`}
          >
            Esvaziar Lixeira
          </button>
        </div>
        
        <div className="border-t pt-4">
          <button
            onClick={() => setMostrarHistorico(!mostrarHistorico)}
            className="w-full py-2 bg-gray-200 hover:bg-gray-300 rounded-md text-gray-700 font-medium transition"
          >
            {mostrarHistorico ? "Ocultar Histórico" : "Mostrar Histórico"}
          </button>
          
          {mostrarHistorico && (
            <div className="mt-4 max-h-64 overflow-y-auto">
              <table className="w-full text-sm">
                <thead>
                  <tr className="bg-gray-100">
                    <th className="px-2 py-1 text-left">Hora</th>
                    <th className="px-2 py-1 text-left">Ação</th>
                    <th className="px-2 py-1 text-right">Capacidade</th>
                  </tr>
                </thead>
                <tbody>
                  {historico.map((item) => (
                    <tr key={item.id} className="border-b">
                      <td className="px-2 py-1">{item.hora}</td>
                      <td className="px-2 py-1">{item.acao}</td>
                      <td className="px-2 py-1 text-right">{item.capacidade}</td>
                    </tr>
                  ))}
                  {historico.length === 0 && (
                    <tr>
                      <td colSpan="3" className="px-2 py-4 text-center text-gray-500">
                        Nenhum registro no histórico
                      </td>
                    </tr>
                  )}
                </tbody>
              </table>
            </div>
          )}
        </div>
      </div>
      
      <div className="mt-6 text-center text-sm text-gray-500">
        <p>© 2025 EcoSmart - Gerenciamento Inteligente de Resíduos</p>
      </div>
    </div>
  );
};

export default EcoSmartApp;