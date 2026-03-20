  l<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Core AI - Matte Interface</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { background-color: #0A0A0A; color: #D1D1D1; overflow: hidden; }
        .matte-card { background-color: #121212; border: 1px solid rgba(255,255,255,0.05); }
        .fade-out { opacity: 0; transition: opacity 0.5s ease-in-out; pointer-events: none; }
        /* Voice Pulse Animation */
        @keyframes pulse-red {
            0% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(239, 68, 68, 0); }
            100% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0); }
        }
        .listening { animation: pulse-red 1.5s infinite; color: #ef4444 !important; }
    </style>
</head>
<body class="min-h-screen flex flex-col font-sans">
    <nav class="flex justify-between items-center px-8 py-6 border-b border-white/5">
        <div class="flex items-center gap-3">
            <div class="w-10 h-10 bg-blue-600 rounded-xl flex items-center justify-center font-bold text-white shadow-lg shadow-blue-900/40">A</div>
            <span class="text-xl font-bold tracking-tighter">CORE_AI</span>
        </div>
        <div class="text-gray-600">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"></circle><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path></svg>
        </div>
    </nav>
    <main class="flex-1 relative flex items-center justify-center">
        <div id="center-piece" class="text-center transition-all duration-700">
            <h1 class="text-7xl md:text-9xl font-black text-white/5 tracking-tighter">HELLO WORLD</h1>
            <p class="text-gray-700 mt-4 tracking-[0.5em] text-xs uppercase font-bold">Waiting for input...</p>
        </div
        <div id="chat-container" class="absolute inset-0 p-8 overflow-y-auto hidden">
            </div>
    </main>
    <footer class="p-8 pb-12 w-full max-w-4xl mx-auto">
        <div class="matte-card rounded-[30px] p-2 flex items-center shadow-2xl">
            <button id="mic-btn" class="p-4 text-gray-500 hover:text-white transition-all rounded-full">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2a3 3 0 0 0-3 3v7a3 3 0 0 0 6 0V5a3 3 0 0 0-3-3Z"></path><path d="M19 10v2a7 7 0 0 1-14 0v-2"></path><line x1="12" x2="12" y1="19" y2="22"></line></svg>
            </button
            <input type="text" id="user-input" placeholder="Describe your problem..." 
                class="flex-1 bg-transparent border-none outline-none px-4 py-2 text-lg text-white placeholder-gray-700">
            <button id="send-btn" class="bg-white text-black p-4 rounded-2xl hover:bg-blue-600 hover:text-white transition-all transform active:scale-95 shadow-lg">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="22" y1="2" x2="11" y2="13"></line><polygon points="22 2 15 22 11 13 2 9 22 2"></polygon></svg>
            </button>
        </div>
        <p class="text-center mt-4 text-[10px] uppercase tracking-widest text-gray-800 font-bold">Matte Finish Interface v1.0</p>
    </footer>
    <script>
        const input = document.getElementById('user-input');
        const centerPiece = document.getElementById('center-piece');
        const micBtn = document.getElementById('mic-btn');
        // Function to hide HELLO WORLD when user starts typing
        input.addEventListener('input', () => {
            if(input.value.length > 0) {
                centerPiece.classList.add('fade-out');
            } else {
                centerPiece.classList.remove('fade-out');
            }
        });
        // Voice Recognition (JavaScript)
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        if (SpeechRecognition) {
            const recognition = new SpeechRecognition();      
            micBtn.onclick = () => {
                recognition.start();
                micBtn.classList.add('listening');
            };
            recognition.onresult = (event) => {
                const transcript = event.results[0][0].transcript;
                input.value = transcript;
                centerPiece.classList.add('fade-out');
                micBtn.classList.remove('listening');
            };
            recognition.onerror = () => micBtn.classList.remove('listening');
            recognition.onend = () => micBtn.classList.remove('listening');
        } else {
            micBtn.style.display = 'none'; // Hide if browser doesn't support voice
        }
    </script>
</body>
</html>
