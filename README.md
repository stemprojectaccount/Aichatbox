<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Chatbox - Trợ lý Tin học</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary: #10a37f;
            --primary-dark: #0d8c6c;
            --primary-light: #e6f7f2;
            --secondary: #2563eb;
            --accent: #8b5cf6;
            --text: #1f2937;
            --text-light: #6b7280;
            --bg-light: #f9fafb;
            --bg-white: #ffffff;
            --border: #e5e7eb;
            --shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
            --radius: 12px;
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            background: linear-gradient(135deg, #f0f9ff 0%, #e0f2fe 100%);
            display: flex;
            flex-direction: column;
            height: 100vh;
            color: var(--text);
            overflow: hidden;
        }

        .header {
            padding: 16px 24px;
            background: var(--bg-white);
            border-bottom: 1px solid var(--border);
            display: flex;
            align-items: center;
            justify-content: space-between;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
            flex-shrink: 0;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .logo-icon {
            width: 36px;
            height: 36px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 18px;
        }

        .logo-text {
            font-size: 20px;
            font-weight: 700;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .header-actions {
            display: flex;
            gap: 12px;
        }

        .main-container {
            display: flex;
            flex: 1;
            padding: 20px;
            gap: 20px;
            min-height: 0;
            max-width: 1400px;
            margin: 0 auto;
            width: 100%;
        }

        .sidebar {
            width: 260px;
            background: var(--bg-white);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 20px;
            flex-shrink: 0;
        }

        .new-chat-btn {
            background: var(--primary);
            color: white;
            border: none;
            border-radius: var(--radius);
            padding: 12px 16px;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            gap: 8px;
            justify-content: center;
        }

        .new-chat-btn:hover {
            background: var(--primary-dark);
            transform: translateY(-2px);
        }

        .history-section {
            flex: 1;
            overflow-y: auto;
        }

        .history-title {
            font-size: 14px;
            font-weight: 600;
            color: var(--text-light);
            margin-bottom: 12px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .history-item {
            padding: 12px;
            border-radius: 8px;
            cursor: pointer;
            transition: var(--transition);
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .history-item:hover {
            background: var(--primary-light);
        }

        .history-item.active {
            background: var(--primary-light);
            color: var(--primary);
            font-weight: 600;
        }

        .chat-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            background: var(--bg-white);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            transition: var(--transition);
            position: relative;
        }

        .chat-header {
            padding: 16px 24px;
            background: var(--bg-white);
            border-bottom: 1px solid var(--border);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .chat-title {
            font-size: 18px;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .status-indicator {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 14px;
            color: var(--text-light);
        }

        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: #10b981;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        #chatbox {
            flex: 1;
            overflow-y: auto;
            padding: 24px;
            display: flex;
            flex-direction: column;
            gap: 24px;
            scroll-behavior: smooth;
            background: var(--bg-light);
        }

        .msg {
            padding: 20px 24px;
            border-radius: var(--radius);
            max-width: 85%;
            word-wrap: break-word;
            white-space: pre-wrap;
            font-size: 15px;
            line-height: 1.6;
            animation: fadeIn 0.4s ease;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
            transition: var(--transition);
            position: relative;
        }

        .msg:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
        }

        .user {
            background: var(--bg-white);
            border: 1px solid var(--border);
            align-self: flex-end;
            border-bottom-right-radius: 4px;
        }

        .bot {
            background: var(--bg-white);
            border: 1px solid var(--border);
            align-self: flex-start;
            border-bottom-left-radius: 4px;
        }

        .msg-avatar {
            position: absolute;
            top: -12px;
            width: 28px;
            height: 28px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            color: white;
        }

        .user .msg-avatar {
            right: -12px;
            background: var(--secondary);
        }

        .bot .msg-avatar {
            left: -12px;
            background: var(--primary);
        }

        .msg-time {
            font-size: 12px;
            opacity: 0.6;
            margin-top: 8px;
            text-align: right;
        }

        .img-preview {
            max-width: 300px;
            border-radius: 8px;
            margin-top: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        #inputArea {
            display: flex;
            padding: 20px;
            background: var(--bg-white);
            align-items: flex-end;
            gap: 12px;
            border-top: 1px solid var(--border);
            flex-shrink: 0;
        }

        .input-container {
            flex: 1;
            position: relative;
        }

        #userInput {
            width: 100%;
            padding: 16px 120px 16px 20px;
            border: 1px solid var(--border);
            border-radius: 24px;
            outline: none;
            font-size: 15px;
            transition: var(--transition);
            background: var(--bg-light);
            resize: none;
            min-height: 56px;
            max-height: 120px;
            line-height: 1.5;
        }

        #userInput:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(16, 163, 127, 0.1);
            background: var(--bg-white);
        }

        .input-actions {
            position: absolute;
            right: 16px;
            bottom: 16px;
            display: flex;
            gap: 8px;
        }

        .btn {
            border: none;
            border-radius: 50%;
            background: var(--primary);
            color: white;
            cursor: pointer;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: var(--transition);
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        .btn:hover {
            background: var(--primary-dark);
            transform: scale(1.05);
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        }

        .btn:active {
            transform: scale(0.98);
        }

        .btn-secondary {
            background: var(--secondary);
        }

        .btn-secondary:hover {
            background: #1d4ed8;
        }

        .btn-accent {
            background: var(--accent);
        }

        .btn-accent:hover {
            background: #7c3aed;
        }

        .btn-danger {
            background: #ef4444;
        }

        .btn-danger:hover {
            background: #dc2626;
        }

        /* TTS Panel */
        #ttsPanel {
            position: fixed;
            bottom: 100px;
            right: 20px;
            background: var(--bg-white);
            border-radius: var(--radius);
            padding: 24px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
            width: 380px;
            z-index: 100;
            display: none;
            border: 1px solid var(--border);
        }

        #ttsPanel.visible {
            display: block;
            animation: slideIn 0.3s ease;
        }

        .tts-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            border-bottom: 1px solid var(--border);
            padding-bottom: 16px;
        }

        .tts-header h3 {
            margin: 0;
            color: var(--text);
            font-size: 18px;
            font-weight: 700;
        }

        #closeTtsPanel {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: var(--text-light);
            padding: 4px;
            transition: var(--transition);
        }

        #closeTtsPanel:hover {
            color: var(--text);
            transform: rotate(90deg);
        }

        .tts-control {
            margin: 16px 0;
        }

        .tts-control label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--text);
            font-size: 14px;
        }

        .tts-control select, .tts-control input {
            width: 100%;
            padding: 12px 16px;
            border-radius: 8px;
            border: 1px solid var(--border);
            background: var(--bg-light);
            transition: var(--transition);
            font-size: 14px;
        }

        .tts-control select:focus, .tts-control input:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 2px rgba(16, 163, 127, 0.1);
            background: var(--bg-white);
        }

        .tts-control input[type="range"] {
            padding: 0;
            height: 20px;
        }

        .range-value {
            display: inline-block;
            width: 36px;
            text-align: center;
            font-weight: 600;
            color: var(--primary);
            font-size: 14px;
        }

        .language-section {
            background: var(--primary-light);
            padding: 16px;
            border-radius: 10px;
            margin-bottom: 20px;
            border-left: 4px solid var(--primary);
        }

        .language-section h4 {
            margin: 0 0 12px;
            color: var(--primary);
            font-size: 16px;
        }

        .language-options {
            display: flex;
            gap: 12px;
        }

        .language-option {
            flex: 1;
            text-align: center;
            padding: 12px;
            border-radius: 8px;
            background: var(--bg-white);
            cursor: pointer;
            transition: var(--transition);
            border: 2px solid transparent;
        }

        .language-option:hover {
            background: #d1fae5;
        }

        .language-option.active {
            border-color: var(--primary);
            background: #d1fae5;
        }

        .language-option input {
            display: none;
        }

        .voice-preview {
            margin-top: 20px;
            padding: 16px;
            background: var(--bg-light);
            border-radius: 10px;
            border: 1px solid var(--border);
        }

        .voice-preview-text {
            font-size: 14px;
            color: var(--text-light);
            margin-bottom: 12px;
            font-weight: 600;
        }

        .voice-preview-controls {
            display: flex;
            gap: 10px;
        }

        .voice-preview-btn {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 8px;
            background: var(--primary);
            color: white;
            cursor: pointer;
            font-size: 13px;
            transition: var(--transition);
            font-weight: 500;
        }

        .voice-preview-btn:hover {
            background: var(--primary-dark);
        }

        .voice-info {
            font-size: 12px;
            color: var(--text-light);
            margin-top: 8px;
            font-style: italic;
        }

        .typing-indicator {
            display: inline-flex;
            align-items: center;
            color: var(--text-light);
            font-style: italic;
        }

        .typing-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background-color: var(--text-light);
            margin-left: 4px;
            animation: typingAnimation 1.4s infinite ease-in-out;
        }

        .typing-dot:nth-child(1) { animation-delay: 0s; }
        .typing-dot:nth-child(2) { animation-delay: 0.2s; }
        .typing-dot:nth-child(3) { animation-delay: 0.4s; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes typingAnimation {
            0%, 60%, 100% { transform: translateY(0); }
            30% { transform: translateY(-5px); }
        }

        .footer {
            text-align: center;
            padding: 16px;
            font-size: 13px;
            color: var(--text-light);
            background: var(--bg-white);
            border-top: 1px solid var(--border);
            flex-shrink: 0;
        }

        /* Responsive */
        @media (max-width: 1024px) {
            .sidebar {
                width: 220px;
            }
        }

        @media (max-width: 768px) {
            .main-container {
                padding: 12px;
                gap: 12px;
            }
            
            .sidebar {
                display: none;
            }
            
            .msg {
                max-width: 90%;
            }
            
            #ttsPanel {
                width: 90%;
                right: 5%;
                left: 5%;
            }
        }

        @media (max-width: 480px) {
            .btn {
                width: 36px;
                height: 36px;
            }
            
            #userInput {
                padding: 12px 100px 12px 16px;
            }
            
            .chat-header {
                padding: 12px 16px;
            }
            
            .header {
                padding: 12px 16px;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <div class="logo">
            <div class="logo-icon">
                <i class="fas fa-robot"></i>
            </div>
            <div class="logo-text">AI Chatbox</div>
        </div>
        <div class="header-actions">
            <button class="btn btn-secondary" id="imgBtn" title="Tải ảnh lên">
                <i class="fas fa-image"></i>
            </button>
            <button class="btn btn-accent" id="ttsBtn" title="Cài đặt giọng nói">
                <i class="fas fa-volume-up"></i>
            </button>
        </div>
    </div>

    <div class="main-container">
        <div class="sidebar">
            <button class="new-chat-btn" id="newChatBtn">
                <i class="fas fa-plus"></i>
                Cuộc trò chuyện mới
            </button>
            <div class="history-section">
                <div class="history-title">Lịch sử trò chuyện</div>
                <div class="history-item active">
                    <i class="fas fa-message"></i>
                    <span>Giới thiệu về AI</span>
                </div>
                <div class="history-item">
                    <i class="fas fa-message"></i>
                    <span>Hỏi về lập trình</span>
                </div>
                <div class="history-item">
                    <i class="fas fa-message"></i>
                    <span>Tìm hiểu Tin học</span>
                </div>
            </div>
        </div>

        <div class="chat-container">
            <div class="chat-header">
                <div class="chat-title">
                    <i class="fas fa-robot"></i>
                    Trợ lý AI Tin Học
                </div>
                <div class="status-indicator">
                    <div class="status-dot"></div>
                    <span>Đang trực tuyến</span>
                </div>
            </div>
            <div id="chatbox"></div>
            <div id="inputArea">
                <div class="input-container">
                    <textarea id="userInput" placeholder="Nhập câu hỏi của bạn..." rows="1"></textarea>
                    <div class="input-actions">
                        <button id="micBtn" class="btn btn-secondary" title="Nhận diện giọng nói">
                            <i class="fas fa-microphone"></i>
                        </button>
                        <button id="stopBtn" class="btn btn-danger" title="Dừng phát âm thanh" style="display:none">
                            <i class="fas fa-stop"></i>
                        </button>
                        <button id="sendBtn" class="btn" title="Gửi tin nhắn">
                            <i class="fas fa-paper-plane"></i>
                        </button>
                    </div>
                </div>
                <input type="file" id="fileInput" accept="image/*" style="display:none"/>
            </div>
        </div>
    </div>

    <div id="ttsPanel">
        <div class="tts-header">
            <h3><i class="fas fa-cog"></i> Cài đặt Giọng nói</h3>
            <button id="closeTtsPanel">×</button>
        </div>
        
        <div class="language-section">
            <h4>Lựa chọn ngôn ngữ</h4>
            <div class="language-options">
                <div class="language-option active" data-lang="vi">
                    <input type="radio" id="vietnamese" name="language" value="vi" checked>
                    <label for="vietnamese">Tiếng Việt</label>
                </div>
                <div class="language-option" data-lang="en">
                    <input type="radio" id="english" name="language" value="en">
                    <label for="english">Tiếng Anh</label>
                </div>
            </div>
        </div>
        
        <div class="tts-control">
            <label for="voiceSelect"><i class="fas fa-voice"></i> Giọng đọc:</label>
            <select id="voiceSelect"></select>
            <div class="voice-info" id="voiceInfo"></div>
        </div>
        
        <div class="tts-control">
            <label for="volume"><i class="fas fa-volume-up"></i> Âm lượng: <span class="range-value" id="volumeValue">1</span></label>
            <input type="range" id="volume" min="0" max="1" step="0.1" value="1">
        </div>
        
        <div class="tts-control">
            <label for="rate"><i class="fas fa-tachometer-alt"></i> Tốc độ: <span class="range-value" id="rateValue">1</span></label>
            <input type="range" id="rate" min="0.5" max="2" step="0.1" value="1">
        </div>
        
        <div class="tts-control">
            <label for="pitch"><i class="fas fa-music"></i> Độ cao: <span class="range-value" id="pitchValue">1</span></label>
            <input type="range" id="pitch" min="0" max="2" step="0.1" value="1">
        </div>
        
        <div class="voice-preview">
            <div class="voice-preview-text">Nghe thử giọng đọc:</div>
            <div class="voice-preview-controls">
                <button id="testVietnamese" class="voice-preview-btn">
                    <i class="fas fa-play"></i> Tiếng Việt
                </button>
                <button id="testEnglish" class="voice-preview-btn">
                    <i class="fas fa-play"></i> Tiếng Anh
                </button>
            </div>
        </div>
    </div>

    <div class="footer">
        <p>AI Chatbox - Ứng dụng trò chuyện với AI Tin Học | Hỗ trợ học tập hiệu quả</p>
    </div>

    <script>
        // DOM Elements
        const chatbox = document.getElementById('chatbox');
        const input = document.getElementById('userInput');
        const sendBtn = document.getElementById('sendBtn');
        const micBtn = document.getElementById('micBtn');
        const stopBtn = document.getElementById('stopBtn');
        const imgBtn = document.getElementById('imgBtn');
        const fileInput = document.getElementById('fileInput');
        const ttsBtn = document.getElementById('ttsBtn');
        const ttsPanel = document.getElementById('ttsPanel');
        const closeTtsPanel = document.getElementById('closeTtsPanel');
        const voiceSelect = document.getElementById('voiceSelect');
        const voiceInfo = document.getElementById('voiceInfo');
        const volumeControl = document.getElementById('volume');
        const rateControl = document.getElementById('rate');
        const pitchControl = document.getElementById('pitch');
        const testVietnameseBtn = document.getElementById('testVietnamese');
        const testEnglishBtn = document.getElementById('testEnglish');
        const volumeValue = document.getElementById('volumeValue');
        const rateValue = document.getElementById('rateValue');
        const pitchValue = document.getElementById('pitchValue');
        const languageOptions = document.querySelectorAll('.language-option');
        const newChatBtn = document.getElementById('newChatBtn');

        // State variables
        let ttsEnabled = true;
        let recognition;
        let voices = [];
        let currentUtterance = null;
        let selectedLanguage = 'vi';
        let isSpeaking = false;
        let isListening = false;

        // Initialize TTS with better voice selection
        function initTTS() {
            speechSynthesis.onvoiceschanged = function() {
                voices = speechSynthesis.getVoices();
                console.log('Available voices:', voices);
                populateVoiceList();
                
                // Try to find the best Vietnamese voice
                const vietnameseVoice = findBestVietnameseVoice();
                
                if (vietnameseVoice) {
                    voiceSelect.value = vietnameseVoice.voiceURI;
                    updateVoiceInfo(vietnameseVoice);
                    console.log('Selected Vietnamese voice:', vietnameseVoice);
                } else {
                    // Fallback to any available voice
                    if (voices.length > 0) {
                        voiceSelect.value = voices[0].voiceURI;
                        updateVoiceInfo(voices[0]);
                        console.log('Fallback to voice:', voices[0]);
                    }
                }
            };
            
            voices = speechSynthesis.getVoices();
            if (voices.length > 0) {
                populateVoiceList();
            }
        }

        // Find the best Vietnamese voice
        function findBestVietnameseVoice() {
            // Priority 1: Vietnamese voices
            const viVoices = voices.filter(voice => 
                voice.lang.includes('vi-VN') || 
                voice.lang === 'vi' ||
                voice.name.toLowerCase().includes('vietnamese')
            );
            
            if (viVoices.length > 0) {
                return viVoices[0];
            }
            
            // Priority 2: Any Asian language voices (might work better for Vietnamese)
            const asianVoices = voices.filter(voice => 
                voice.lang.includes('zh') || 
                voice.lang.includes('ja') ||
                voice.lang.includes('ko') ||
                voice.name.toLowerCase().includes('chinese') ||
                voice.name.toLowerCase().includes('japanese') ||
                voice.name.toLowerCase().includes('korean')
            );
            
            if (asianVoices.length > 0) {
                return asianVoices[0];
            }
            
            // Priority 3: Female voices (generally clearer for Vietnamese)
            const femaleVoices = voices.filter(voice => 
                voice.name.toLowerCase().includes('female') ||
                voice.name.toLowerCase().includes('woman') ||
                voice.name.toLowerCase().includes('zira') || // Windows female voice
                voice.name.toLowerCase().includes('karen') // macOS Australian female
            );
            
            if (femaleVoices.length > 0) {
                return femaleVoices[0];
            }
            
            return null;
        }

        // Populate voice list
        function populateVoiceList() {
            voiceSelect.innerHTML = '';
            
            // Show all available voices for debugging
            voices.forEach(voice => {
                const option = document.createElement('option');
                option.value = voice.voiceURI;
                option.textContent = `${voice.name} (${voice.lang})`;
                voiceSelect.appendChild(option);
            });
            
            // Try to select the best voice for the current language
            let selectedVoice = null;
            
            if (selectedLanguage === 'vi') {
                selectedVoice = findBestVietnameseVoice();
            } else {
                // For English, prefer female voices
                selectedVoice = voices.find(voice => 
                    voice.lang.includes('en') && 
                    (voice.name.toLowerCase().includes('female') || voice.name.toLowerCase().includes('zira'))
                ) || voices.find(voice => voice.lang.includes('en')) || voices[0];
            }
            
            if (selectedVoice) {
                voiceSelect.value = selectedVoice.voiceURI;
                updateVoiceInfo(selectedVoice);
            } else if (voices.length > 0) {
                voiceSelect.value = voices[0].voiceURI;
                updateVoiceInfo(voices[0]);
            }
        }

        // Update voice information
        function updateVoiceInfo(voice) {
            if (!voice) return;
            
            let info = `Ngôn ngữ: ${voice.lang}`;
            if (voice.localService) {
                info += ` | Giọng hệ thống`;
            }
            
            voiceInfo.textContent = info;
        }

        // Speak text with current settings
        function speak(text) {
            if (!ttsEnabled || !window.speechSynthesis) {
                console.log('TTS not available');
                return;
            }
            
            if (currentUtterance) {
                speechSynthesis.cancel();
            }
            
            const selectedVoice = voiceSelect.value;
            const voice = voices.find(v => v.voiceURI === selectedVoice);
            
            if (!voice) {
                console.log('No voice selected');
                return;
            }
            
            currentUtterance = new SpeechSynthesisUtterance(text);
            currentUtterance.voice = voice;
            currentUtterance.volume = parseFloat(volumeControl.value);
            currentUtterance.rate = parseFloat(rateControl.value);
            currentUtterance.pitch = parseFloat(pitchControl.value);
            
            // Optimize for Vietnamese pronunciation
            if (selectedLanguage === 'vi') {
                currentUtterance.rate = Math.max(0.8, currentUtterance.rate); // Don't speak too fast for Vietnamese
            }
            
            currentUtterance.onstart = function() {
                isSpeaking = true;
                stopBtn.style.display = 'flex';
                console.log('Started speaking:', text);
            };
            
            currentUtterance.onend = function() {
                isSpeaking = false;
                currentUtterance = null;
                stopBtn.style.display = 'none';
                console.log('Finished speaking');
            };
            
            currentUtterance.onerror = function(event) {
                isSpeaking = false;
                currentUtterance = null;
                stopBtn.style.display = 'none';
                console.error('Lỗi phát âm thanh:', event.error);
                
                // Fallback: Show notification about TTS issue
                if (event.error === 'not-allowed') {
                    addMessage('Lỗi: Trình duyệt không cho phép sử dụng Text-to-Speech. Vui lòng kiểm tra cài đặt quyền micro.', 'bot');
                }
            };
            
            speechSynthesis.speak(currentUtterance);
        }

        // Stop speaking
        function stopSpeaking() {
            if (window.speechSynthesis && (currentUtterance || isSpeaking)) {
                speechSynthesis.cancel();
                currentUtterance = null;
                isSpeaking = false;
                stopBtn.style.display = 'none';
                console.log('Speech stopped by user');
            }
        }

        // Add message to chat
        function addMessage(text, cls, imgSrc) {
            const div = document.createElement('div');
            div.className = `msg ${cls}`;
            
            // Add avatar
            const avatar = document.createElement('div');
            avatar.className = 'msg-avatar';
            avatar.innerHTML = cls === 'user' ? '<i class="fas fa-user"></i>' : '<i class="fas fa-robot"></i>';
            div.appendChild(avatar);
            
            if (text === 'Đang gõ...') {
                const typingIndicator = document.createElement('div');
                typingIndicator.className = 'typing-indicator';
                typingIndicator.innerHTML = 'AI đang soạn tin nhắn<span class="typing-dot"></span><span class="typing-dot"></span><span class="typing-dot"></span>';
                div.appendChild(typingIndicator);
            } else {
                div.appendChild(document.createTextNode(text));
                
                // Add timestamp
                const timeDiv = document.createElement('div');
                timeDiv.className = 'msg-time';
                const now = new Date();
                timeDiv.textContent = `${now.getHours()}:${now.getMinutes().toString().padStart(2, '0')}`;
                div.appendChild(timeDiv);
            }
            
            if (imgSrc) {
                const img = document.createElement('img');
                img.src = imgSrc;
                img.className = 'img-preview';
                div.appendChild(document.createElement('br'));
                div.appendChild(img);
            }
            
            chatbox.appendChild(div);
            chatbox.scrollTo({top: chatbox.scrollHeight, behavior: "smooth"});
            return div;
        }

        // Send message to API
        async function sendMessage(base64Img) {
            const question = input.value.trim();
            if (!question && !base64Img) return;
            
            addMessage(question || '[Đã gửi ảnh]', 'user', base64Img);
            input.value = '';

            const typing = addMessage('Đang gõ...', 'bot');

            // Simulate API call (replace with actual API)
            setTimeout(() => {
                const responses = [
                    "Xin chào! Tôi là trợ lý AI Tin Học. Tôi có thể giúp gì cho bạn?",
                    "Tôi hiểu câu hỏi của bạn. Để trả lời chính xác, tôi cần thêm thông tin chi tiết.",
                    "Dựa trên yêu cầu của bạn, tôi đề xuất các bước sau: 1) Xác định vấn đề, 2) Phân tích nguyên nhân, 3) Tìm giải pháp phù hợp.",
                    "Trong lĩnh vực tin học, đây là một chủ đề thú vị. Bạn có thể tìm hiểu thêm qua các tài liệu chuyên ngành.",
                    "Tôi đã phân tích yêu cầu của bạn và đưa ra giải pháp tối ưu nhất cho vấn đề này."
                ];
                const randomResponse = responses[Math.floor(Math.random() * responses.length)];
                typing.textContent = randomResponse;
                
                // Speak the reply only if TTS is enabled
                if (ttsEnabled) {
                    speak(randomResponse);
                }
            }, 2000);
        }

        // Auto-resize textarea
        input.addEventListener('input', function() {
            this.style.height = 'auto';
            this.style.height = (this.scrollHeight) + 'px';
        });

        // Event Listeners
        sendBtn.addEventListener('click', () => sendMessage());
        input.addEventListener('keydown', e => { 
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendMessage(); 
            }
        });

        // Image button
        imgBtn.addEventListener('click', () => fileInput.click());
        fileInput.addEventListener('change', () => {
            const file = fileInput.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = e => sendMessage(e.target.result);
            reader.readAsDataURL(file);
        });

        // TTS Panel
        ttsBtn.addEventListener('click', () => {
            ttsPanel.classList.toggle('visible');
        });

        closeTtsPanel.addEventListener('click', () => {
            ttsPanel.classList.remove('visible');
        });

        // Update range values
        volumeControl.addEventListener('input', () => {
            volumeValue.textContent = volumeControl.value;
        });

        rateControl.addEventListener('input', () => {
            rateValue.textContent = rateControl.value;
        });

        pitchControl.addEventListener('input', () => {
            pitchValue.textContent = pitchControl.value;
        });

        // Test voices
        testVietnameseBtn.addEventListener('click', () => {
            speak('Xin chào! Tôi là trợ lý AI Tin Học. Tôi có thể giúp gì cho bạn hôm nay?');
        });

        testEnglishBtn.addEventListener('click', () => {
            speak('Hello! I am your AI assistant. How can I help you today?');
        });

        // Language selection
        languageOptions.forEach(option => {
            option.addEventListener('click', () => {
                languageOptions.forEach(opt => opt.classList.remove('active'));
                option.classList.add('active');
                
                const lang = option.getAttribute('data-lang');
                selectedLanguage = lang;
                populateVoiceList();
            });
        });

        // Voice selection change
        voiceSelect.addEventListener('change', () => {
            const selectedVoice = voices.find(v => v.voiceURI === voiceSelect.value);
            if (selectedVoice) {
                updateVoiceInfo(selectedVoice);
            }
        });

        // Stop speaking
        stopBtn.addEventListener('click', stopSpeaking);

        // New chat button
        newChatBtn.addEventListener('click', () => {
            chatbox.innerHTML = '';
            addMessage("Xin chào! Tôi là trợ lý AI Tin Học. Tôi có thể giúp gì cho bạn?", 'bot');
        });

        // Voice Recognition
        if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
            const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
            recognition = new SR();
            recognition.lang = 'vi-VN';
            recognition.interimResults = false;
            recognition.continuous = false;

            recognition.onresult = (event) => {
                const transcript = event.results[0][0].transcript;
                input.value = transcript;
                // Auto-resize textarea
                input.style.height = 'auto';
                input.style.height = (input.scrollHeight) + 'px';
            };
            
            recognition.onerror = (e) => {
                console.log('Lỗi nhận diện giọng nói:', e);
                addMessage('Xin lỗi, tôi không nghe rõ. Bạn có thể nói lại được không?', 'bot');
                micBtn.innerHTML = '<i class="fas fa-microphone"></i>';
                isListening = false;
            };
            
            recognition.onend = () => {
                micBtn.innerHTML = '<i class="fas fa-microphone"></i>';
                isListening = false;
            };
        } else {
            micBtn.disabled = true;
            micBtn.title = "Trình duyệt không hỗ trợ nhận diện giọng nói";
        }

        // Microphone button
        micBtn.addEventListener('click', () => {
            if (!recognition) return;
            
            if (!isListening) {
                recognition.start();
                isListening = true;
                micBtn.innerHTML = '<i class="fas fa-microphone-slash"></i>';
            } else {
                recognition.stop();
                isListening = false;
                micBtn.innerHTML = '<i class="fas fa-microphone"></i>';
            }
        });

        // Initialize TTS when page loads
        window.addEventListener('DOMContentLoaded', function() {
            initTTS();
            
            // Add welcome message only (removed automatic speaking)
            setTimeout(() => {
                addMessage("Xin chào! Tôi là trợ lý AI Tin Học. Tôi có thể giúp gì cho bạn?", 'bot');
            }, 500);
        });

        // Display initial range values
        volumeValue.textContent = volumeControl.value;
        rateValue.textContent = rateControl.value;
        pitchValue.textContent = pitchControl.value;
    </script>
</body>
</html>
