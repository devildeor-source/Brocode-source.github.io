import React, { useState, useEffect, useRef } from 'react';
import { Mic, Send, Bot, User, Settings, Shield, Sparkles } from 'lucide-react';

const PremiumAISolver = () => {
  const [input, setInput] = useState("");
  const [isListening, setIsListening] = useState(false);
  const [hasStarted, setHasStarted] = useState(false);
  const [messages, setMessages] = useState([]);
  
  const recognitionRef = useRef(null);

  // Initialize Voice Recognition
  useEffect(() => {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (SpeechRecognition) {
      recognitionRef.current = new SpeechRecognition();
      recognitionRef.current.onresult = (event) => {
        const text = event.results[0][0].transcript;
        setInput(text);
        handleStartInteraction();
      };
    }
  }, []);

  const handleStartInteraction = () => {
    if (!hasStarted) setHasStarted(true);
  };

  const onInputChange = (e) => {
    setInput(e.target.value);
    if (e.target.value.length > 0) handleStartInteraction();
  };

  const triggerVoice = () => {
    setIsListening(true);
    recognitionRef.current.start();
    handleStartInteraction();
  };

  return (
    <div className="min-h-screen bg-[#0A0A0A] text-[#D1D1D1] font-sans selection:bg-blue-500/30 flex flex-col">
      
      {/* 1. TOP NAVIGATION */}
      <nav className="flex justify-between items-center px-8 py-6 backdrop-blur-md bg-[#0A0A0A]/80 sticky top-0 z-50 border-b border-white/5">
        <div className="flex items-center gap-3">
          <div className="w-10 h-10 bg-gradient-to-br from-blue-600 to-blue-800 rounded-xl flex items-center justify-center shadow-[0_0_20px_rgba(37,99,235,0.3)]">
            <Shield className="text-white" size={20} />
          </div>
          <span className="text-xl font-bold tracking-tighter text-white">CORE_AI</span>
        </div>
        <div className="flex gap-6 items-center text-gray-500">
          <Settings size={20} className="hover:text-white cursor-pointer transition-colors" />
          <div className="w-8 h-8 rounded-full bg-[#1A1A1A] border border-white/10 flex items-center justify-center">
            <User size={16} />
          </div>
        </div>
      </nav>

      {/* 2. MAIN CONTENT AREA */}
      <main className="flex-1 flex flex-col relative max-w-5xl w-full mx-auto px-6">
        
        {/* "HELLO WORLD" CENTERPIECE (Fades out when typing starts) */}
        {!hasStarted && (
          <div className="absolute inset-0 flex flex-col items-center justify-center transition-all duration-700 ease-in-out pointer-events-none">
            <h1 className="text-7xl font-black tracking-tighter text-white/10 animate-pulse">
              HELLO WORLD
            </h1>
            <p className="text-gray-600 mt-4 tracking-[0.4em] text-[10px] uppercase">
              System Ready • Voice & Text Enabled
            </p>
          </div>
        )}

        {/* CHAT MESSAGES (Visible after interaction) */}
        <div className={`flex-1 py-10 space-y-8 transition-opacity duration-500 ${hasStarted ? 'opacity-100' : 'opacity-0'}`}>
          {messages.map((m, i) => (
            <div key={i} className={`flex gap-4 ${m.role === 'user' ? 'justify-end' : 'justify-start'} animate-in fade-in slide-in-from-bottom-2`}>
              {m.role === 'ai' && <Bot size={20} className="text-blue-500 mt-2" />}
              <div className={`p-5 rounded-3xl max-w-[70%] ${
                m.role === 'user' 
                ? 'bg-blue-600 text-white rounded-br-none shadow-xl' 
                : 'bg-[#151515] border border-white/5 rounded-bl-none text-gray-200'
              }`}>
                {m.content}
              </div>
            </div>
          ))}
        </div>
      </main>

      {/* 3. THE MATTE BLACK INPUT SYSTEM */}
      <div className="p-8 pb-12 w-full max-w-4xl mx-auto">
        <div className="relative group">
          {/* Main Input Container */}
          <div className="bg-[#121212] border border-white/10 rounded-[30px] p-3 flex items-center shadow-[0_20px_50px_rgba(0,0,0,0.5)] transition-all focus-within:border-blue-500/50">
            
            {/* Mic Option */}
            <button 
              onClick={triggerVoice}
              className={`p-4 rounded-2xl transition-all ${isListening ? 'bg-red-500/10 text-red-500 animate-pulse' : 'hover:bg-white/5 text-gray-500'}`}
            >
              <Mic size={24} />
            </button>

            {/* Input Field */}
            <input 
              type="text"
              value={input}
              onChange={onInputChange}
              placeholder="Ask anything..."
              className="flex-1 bg-transparent border-none outline-none px-4 text-lg text-white placeholder-gray-700"
            />

            {/* Action Buttons */}
            <div className="flex gap-2 pr-2">
              <button className="p-4 text-gray-600 hover:text-white transition-colors">
                <Sparkles size={22} />
              </button>
              <button 
                className="bg-white text-black p-4 rounded-2xl hover:bg-blue-500 hover:text-white transition-all transform active:scale-90 shadow-lg"
                onClick={() => {
                  setMessages([...messages, {role: 'user', content: input}]);
                  setInput("");
                }}
              >
                <Send size={22} />
              </button>
            </div>
          </div>
          
          {/* Subtle Branding Bottom */}
          <p className="text-center mt-4 text-[9px] uppercase tracking-[0.3em] text-gray-700 font-bold">
            Powered by Core_AI Engine v1.0
          </p>
        </div>
      </div>
    </div>
  );
};

export default PremiumAISolver;
