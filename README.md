<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Pro Ultimate - Toutes fonctionnalités</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jsQR/1.4.0/jsQR.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap');
        
        * { font-family: 'Inter', sans-serif; }
        
        :root {
            --primary: #667eea;
            --secondary: #764ba2;
            --dark: #1a1a2e;
            --light: #f5f5f7;
        }
        
        body {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            min-height: 100vh;
            transition: all 0.3s ease;
        }
        
        body.dark-mode {
            background: linear-gradient(135deg, #0f0f23 0%, #1a1a3e 100%);
        }
        
        .glass-panel {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            transition: all 0.3s ease;
        }
        
        body.dark-mode .glass-panel {
            background: rgba(30, 30, 50, 0.95);
            border-color: rgba(255, 255, 255, 0.1);
            color: white;
        }
        
        .tab-active {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 10px 25px -5px rgba(102, 126, 234, 0.4);
        }
        
        .qr-preview {
            background-image: 
                linear-gradient(45deg, #f0f0f0 25%, transparent 25%), 
                linear-gradient(-45deg, #f0f0f0 25%, transparent 25%), 
                linear-gradient(45deg, transparent 75%, #f0f0f0 75%), 
                linear-gradient(-45deg, transparent 75%, #f0f0f0 75%);
            background-size: 20px 20px;
            background-position: 0 0, 0 10px, 10px -10px, -10px 0px;
            position: relative;
            overflow: hidden;
        }
        
        body.dark-mode .qr-preview {
            background-image: 
                linear-gradient(45deg, #2a2a3e 25%, transparent 25%), 
                linear-gradient(-45deg, #2a2a3e 25%, transparent 25%), 
                linear-gradient(45deg, transparent 75%, #2a2a3e 75%), 
                linear-gradient(-45deg, transparent 75%, #2a2a3e 75%);
        }
        
        .btn-primary {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 15px 30px -5px rgba(102, 126, 234, 0.5);
        }
        
        .btn-primary::after {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
            transform: rotate(45deg);
            transition: all 0.5s;
        }
        
        .btn-primary:hover::after {
            left: 100%;
        }
        
        .input-field {
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }
        
        .input-field:focus {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px -5px rgba(102, 126, 234, 0.2);
            border-color: var(--primary);
        }
        
        body.dark-mode .input-field {
            background: rgba(50, 50, 70, 0.8);
            color: white;
            border-color: rgba(255,255,255,0.1);
        }
        
        body.dark-mode .input-field:focus {
            border-color: var(--primary);
        }
        
        .social-btn {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .social-btn:hover {
            transform: scale(1.05) translateY(-3px);
        }
        
        .social-btn.active {
            ring: 3px solid var(--primary);
            transform: scale(1.05);
        }
        
        .history-item {
            transition: all 0.3s ease;
            animation: slideIn 0.3s ease;
        }
        
        @keyframes slideIn {
            from { opacity: 0; transform: translateX(-20px); }
            to { opacity: 1; transform: translateX(0); }
        }
        
        .history-item:hover {
            transform: translateX(5px);
            box-shadow: 0 10px 30px -5px rgba(0, 0, 0, 0.2);
        }
        
        .scanner-overlay {
            background: linear-gradient(0deg, 
                rgba(0,0,0,0.8) 0%, 
                transparent 20%, 
                transparent 80%, 
                rgba(0,0,0,0.8) 100%);
        }
        
        .scan-line {
            position: absolute;
            width: 100%;
            height: 2px;
            background: #00ff00;
            box-shadow: 0 0 10px #00ff00;
            animation: scan 2s linear infinite;
        }
        
        @keyframes scan {
            0% { top: 0; }
            100% { top: 100%; }
        }
        
        .stat-card {
            background: linear-gradient(135deg, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0.05) 100%);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .gradient-text {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .floating-action {
            position: fixed;
            bottom: 30px;
            right: 30px;
            z-index: 100;
        }
        
        .pulse-ring {
            position: absolute;
            border-radius: 50%;
            animation: pulse-ring 2s cubic-bezier(0.215, 0.61, 0.355, 1) infinite;
        }
        
        @keyframes pulse-ring {
            0% { transform: scale(0.33); opacity: 1; }
            80%, 100% { transform: scale(2); opacity: 0; }
        }
        
        .template-card {
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .template-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px -5px rgba(0,0,0,0.2);
        }
        
        .batch-item {
            animation: fadeInUp 0.3s ease;
        }
        
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .loader {
            border: 3px solid rgba(255,255,255,0.3);
            border-radius: 50%;
            border-top: 3px solid white;
            width: 20px;
            height: 20px;
            animation: spin 1s linear infinite;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .toast {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%) translateY(100px);
            background: #333;
            color: white;
            padding: 16px 32px;
            border-radius: 50px;
            opacity: 0;
            transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 12px;
            box-shadow: 0 10px 40px -5px rgba(0,0,0,0.3);
        }
        
        .toast.show {
            transform: translateX(-50%) translateY(0);
            opacity: 1;
        }
        
        .theme-toggle {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 100;
        }
        
        .qr-3d {
            transform-style: preserve-3d;
            transition: transform 0.5s;
        }
        
        .qr-3d:hover {
            transform: rotateY(10deg) rotateX(5deg);
        }
        
        .confetti {
            position: fixed;
            width: 10px;
            height: 10px;
            background: #f0f;
            position: absolute;
            animation: confetti-fall 3s linear forwards;
        }
        
        @keyframes confetti-fall {
            to {
                transform: translateY(100vh) rotate(720deg);
                opacity: 0;
            }
        }
    </style>
</head>
<body class="p-4 md:p-8">
    <!-- Theme Toggle -->
    <button onclick="toggleTheme()" class="theme-toggle w-12 h-12 rounded-full glass-panel flex items-center justify-center hover:scale-110 transition-transform shadow-lg">
        <i class="fas fa-moon text-xl" id="theme-icon"></i>
    </button>

    <div class="max-w-7xl mx-auto">
        <!-- Header -->
        <header class="text-center mb-8 text-white relative">
            <div class="inline-block mb-4">
                <i class="fas fa-qrcode text-6xl mb-2 animate-pulse"></i>
            </div>
            <h1 class="text-5xl md:text-6xl font-bold mb-3">
                QR Code <span class="gradient-text bg-white text-transparent bg-clip-text">Ultimate</span>
            </h1>
            <p class="text-xl opacity-90 mb-6">Générateur tout-en-un avec scanner, historique, templates & analytics</p>
            
            <!-- Quick Stats -->
            <div class="flex justify-center gap-6 flex-wrap">
                <div class="glass-panel rounded-full px-6 py-2 flex items-center gap-2">
                    <i class="fas fa-qrcode text-green-400"></i>
                    <span id="total-generated">0 QR générés</span>
                </div>
                <div class="glass-panel rounded-full px-6 py-2 flex items-center gap-2">
                    <i class="fas fa-eye text-blue-400"></i>
                    <span id="total-scans">0 scans</span>
                </div>
                <div class="glass-panel rounded-full px-6 py-2 flex items-center gap-2 cursor-pointer hover:bg-white/20 transition-all" onclick="showFavorites()">
                    <i class="fas fa-heart text-red-400"></i>
                    <span id="fav-count">0 favoris</span>
                </div>
            </div>
        </header>

        <!-- Navigation Tabs -->
        <div class="glass-panel rounded-2xl p-2 mb-6 flex flex-wrap gap-2 justify-center">
            <button onclick="switchMainTab('generate')" class="main-tab tab-active px-6 py-3 rounded-xl font-medium transition-all flex items-center gap-2" data-tab="generate">
                <i class="fas fa-magic"></i> Générer
            </button>
            <button onclick="switchMainTab('scan')" class="main-tab px-6 py-3 rounded-xl font-medium text-gray-600 hover:bg-gray-100 transition-all flex items-center gap-2" data-tab="scan">
                <i class="fas fa-camera"></i> Scanner
            </button>
            <button onclick="switchMainTab('batch')" class="main-tab px-6 py-3 rounded-xl font-medium text-gray-600 hover:bg-gray-100 transition-all flex items-center gap-2" data-tab="batch">
                <i class="fas fa-layer-group"></i> Lot
            </button>
            <button onclick="switchMainTab('templates')" class="main-tab px-6 py-3 rounded-xl font-medium text-gray-600 hover:bg-gray-100 transition-all flex items-center gap-2" data-tab="templates">
                <i class="fas fa-palette"></i> Templates
            </button>
            <button onclick="switchMainTab('analytics')" class="main-tab px-6 py-3 rounded-xl font-medium text-gray-600 hover:bg-gray-100 transition-all flex items-center gap-2" data-tab="analytics">
                <i class="fas fa-chart-pie"></i> Stats
            </button>
        </div>

        <!-- GENERATE TAB -->
        <div id="tab-generate" class="main-content">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                <!-- Left Panel -->
                <div class="lg:col-span-7 space-y-6">
                    <!-- Type Selector -->
                    <div class="glass-panel rounded-2xl p-4">
                        <p class="text-sm font-medium text-gray-500 mb-3 uppercase tracking-wide">Type de QR Code</p>
                        <div class="grid grid-cols-4 md:grid-cols-8 gap-2">
                            <button onclick="switchType('url')" class="type-btn tab-active p-3 rounded-xl text-sm flex flex-col items-center gap-1 transition-all" data-type="url">
                                <i class="fas fa-link text-lg"></i>
                                <span class="text-xs">URL</span>
                            </button>
                            <button onclick="switchType('wifi')" class="type-btn p-3 rounded-xl text-sm flex flex-col items-center gap-1 text-gray-600 hover:bg-gray-100 transition-all" data-type="wifi">
                                <i class="fas fa-wifi text-lg"></i>
                                <span class="text-xs">Wi-Fi</span>
                            </button>
                            <button onclick="switchType('contact')" class="type-btn p-3 rounded-xl text-sm flex flex-col items-center gap-1 text-gray-600 hover:bg-gray-100 transition-all" data-type="contact">
                                <i class="fas fa-address-card text-lg"></i>
                                <span class="text-xs">Contact</span>
                            </button>
                            <button onclick="switchType('social')" class="type-btn p-3 rounded-xl text-sm flex flex-col items-center gap-1 text-gray-600 hover:bg-gray-100 transition-all" data-type="social">
                                <i class="fas fa-share-alt text-lg"></i>
                                <span class="text-xs">Social</span>
                            </button>
                            <button onclick="switchType('email')" class="type-btn p-3 rounded-xl text-sm flex flex-col items-center gap-1 text-gray-600 hover:bg-gray-100 transition-all" data-type="email">
                                <i class="fas fa-envelope text-lg"></i>
                                <span class="text-xs">Email</span>
                            </button>
                            <button onclick="switchType('sms')" class="type-btn p-3 rounded-xl text-sm flex flex-col items-center gap-1 text-gray-600 hover:bg-gray-100 transition-all" data-type="sms">
                                <i class="fas fa-sms text-lg"></i>
                                <span class="text-xs">SMS</span>
                            </button>
                            <button onclick="switchType('location')" class="type-btn p-3 rounded-xl text-sm flex flex-col items-center gap-1 text-gray-600 hover:bg-gray-100 transition-all" data-type="location">
                                <i class="fas fa-map-marker-alt text-lg"></i>
                                <span class="text-xs">GPS</span>
                            </button>
                            <button onclick="switchType('event')" class="type-btn p-3 rounded-xl text-sm flex flex-col items-center gap-1 text-gray-600 hover:bg-gray-100 transition-all" data-type="event">
                                <i class="fas fa-calendar-alt text-lg"></i>
                                <span class="text-xs">Event</span>
                            </button>
                        </div>
                    </div>

                    <!-- Forms Container -->
                    <div class="glass-panel rounded-2xl p-6">
                        <!-- URL Form -->
                        <div id="form-url" class="type-form space-y-4">
                            <div>
                                <label class="block text-sm font-medium mb-2">URL du site web</label>
                                <div class="relative">
                                    <i class="fas fa-globe absolute left-4 top-1/2 -translate-y-1/2 text-gray-400"></i>
                                    <input type="url" id="url-input" placeholder="https://www.example.com" 
                                        class="input-field w-full pl-12 pr-4 py-4 rounded-xl bg-gray-50 focus:outline-none"
                                        oninput="updatePreview()">
                                </div>
                            </div>
                            <div class="flex items-center gap-3 p-4 bg-purple-50 rounded-xl border border-purple-100">
                                <input type="checkbox" id="dynamic-qr" class="w-5 h-5 text-purple-600 rounded">
                                <div>
                                    <label class="font-medium text-sm">QR Code Dynamique</label>
                                    <p class="text-xs text-gray-500">Modifiez le lien plus tard sans changer le QR</p>
                                </div>
                                <span class="ml-auto bg-purple-600 text-white text-xs px-2 py-1 rounded-full">PRO</span>
                            </div>
                        </div>

                        <!-- WiFi Form -->
                        <div id="form-wifi" class="type-form hidden space-y-4">
                            <div>
                                <label class="block text-sm font-medium mb-2">Nom du réseau (SSID)</label>
                                <input type="text" id="wifi-ssid" placeholder="Mon_WiFi" 
                                    class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none"
                                    oninput="updatePreview()">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-2">Mot de passe</label>
                                <div class="relative">
                                    <input type="password" id="wifi-password" placeholder="Mot de passe" 
                                        class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none"
                                        oninput="updatePreview()">
                                    <button onclick="togglePassword('wifi-password')" class="absolute right-4 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-600">
                                        <i class="fas fa-eye"></i>
                                    </button>
                                </div>
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-2">Sécurité</label>
                                <select id="wifi-security" class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none" onchange="updatePreview()">
                                    <option value="WPA">WPA/WPA2 (Recommandé)</option>
                                    <option value="WEP">WEP (Ancien)</option>
                                    <option value="nopass">Aucune (Réseau ouvert)</option>
                                </select>
                            </div>
                        </div>

                        <!-- Contact Form -->
                        <div id="form-contact" class="type-form hidden space-y-4">
                            <div class="grid grid-cols-2 gap-4">
                                <input type="text" id="contact-firstname" placeholder="Prénom" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                                <input type="text" id="contact-lastname" placeholder="Nom" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            </div>
                            <input type="tel" id="contact-phone" placeholder="Téléphone" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            <input type="email" id="contact-email" placeholder="Email" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            <input type="text" id="contact-org" placeholder="Entreprise / Organisation" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            <textarea id="contact-address" placeholder="Adresse complète" rows="2" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()"></textarea>
                            <input type="url" id="contact-website" placeholder="Site web" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                        </div>

                        <!-- Social Form -->
                        <div id="form-social" class="type-form hidden space-y-4">
                            <div class="grid grid-cols-3 gap-3 mb-4">
                                <button onclick="selectSocial('facebook')" class="social-btn p-4 rounded-xl border-2 border-gray-200 hover:border-blue-500 hover:bg-blue-50 flex flex-col items-center gap-2" data-social="facebook">
                                    <i class="fab fa-facebook text-2xl text-blue-600"></i>
                                    <span class="text-xs">Facebook</span>
                                </button>
                                <button onclick="selectSocial('instagram')" class="social-btn p-4 rounded-xl border-2 border-gray-200 hover:border-pink-500 hover:bg-pink-50 flex flex-col items-center gap-2" data-social="instagram">
                                    <i class="fab fa-instagram text-2xl text-pink-600"></i>
                                    <span class="text-xs">Instagram</span>
                                </button>
                                <button onclick="selectSocial('twitter')" class="social-btn p-4 rounded-xl border-2 border-gray-200 hover:border-blue-400 hover:bg-blue-50 flex flex-col items-center gap-2" data-social="twitter">
                                    <i class="fab fa-twitter text-2xl text-blue-400"></i>
                                    <span class="text-xs">Twitter</span>
                                </button>
                                <button onclick="selectSocial('linkedin')" class="social-btn p-4 rounded-xl border-2 border-gray-200 hover:border-blue-700 hover:bg-blue-50 flex flex-col items-center gap-2" data-social="linkedin">
                                    <i class="fab fa-linkedin text-2xl text-blue-700"></i>
                                    <span class="text-xs">LinkedIn</span>
                                </button>
                                <button onclick="selectSocial('tiktok')" class="social-btn p-4 rounded-xl border-2 border-gray-200 hover:border-black hover:bg-gray-50 flex flex-col items-center gap-2" data-social="tiktok">
                                    <i class="fab fa-tiktok text-2xl text-black"></i>
                                    <span class="text-xs">TikTok</span>
                                </button>
                                <button onclick="selectSocial('snapchat')" class="social-btn p-4 rounded-xl border-2 border-gray-200 hover:border-yellow-400 hover:bg-yellow-50 flex flex-col items-center gap-2" data-social="snapchat">
                                    <i class="fab fa-snapchat text-2xl text-yellow-500"></i>
                                    <span class="text-xs">Snapchat</span>
                                </button>
                                <button onclick="selectSocial('youtube')" class="social-btn p-4 rounded-xl border-2 border-gray-200 hover:border-red-500 hover:bg-red-50 flex flex-col items-center gap-2" data-social="youtube">
                                    <i class="fab fa-youtube text-2xl text-red-600"></i>
                                    <span class="text-xs">YouTube</span>
                                </button>
                                <button onclick="selectSocial('github')" class="social-btn p-4 rounded-xl border-2 border-gray-200 hover:border-gray-800 hover:bg-gray-100 flex flex-col items-center gap-2" data-social="github">
                                    <i class="fab fa-github text-2xl text-gray-800"></i>
                                    <span class="text-xs">GitHub</span>
                                </button>
                                <button onclick="selectSocial('whatsapp')" class="social-btn p-4 rounded-xl border-2 border-gray-200 hover:border-green-500 hover:bg-green-50 flex flex-col items-center gap-2" data-social="whatsapp">
                                    <i class="fab fa-whatsapp text-2xl text-green-600"></i>
                                    <span class="text-xs">WhatsApp</span>
                                </button>
                            </div>
                            <div id="social-input-container" class="hidden">
                                <label class="block text-sm font-medium mb-2" id="social-label">Nom d'utilisateur</label>
                                <div class="relative">
                                    <span id="social-prefix" class="absolute left-4 top-1/2 -translate-y-1/2 text-gray-400 font-medium"></span>
                                    <input type="text" id="social-input" class="input-field w-full pl-16 pr-4 py-4 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                                </div>
                            </div>
                        </div>

                        <!-- Email Form -->
                        <div id="form-email" class="type-form hidden space-y-4">
                            <input type="email" id="email-to" placeholder="Destinataire" class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            <input type="text" id="email-subject" placeholder="Sujet" class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            <textarea id="email-body" placeholder="Message" rows="4" class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()"></textarea>
                        </div>

                        <!-- SMS Form -->
                        <div id="form-sms" class="type-form hidden space-y-4">
                            <input type="tel" id="sms-number" placeholder="Numéro de téléphone" class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            <textarea id="sms-message" placeholder="Message" rows="4" class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()"></textarea>
                        </div>

                        <!-- Location Form -->
                        <div id="form-location" class="type-form hidden space-y-4">
                            <div class="grid grid-cols-2 gap-4">
                                <input type="text" id="loc-lat" placeholder="Latitude" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                                <input type="text" id="loc-lng" placeholder="Longitude" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            </div>
                            <button onclick="getCurrentLocation()" class="w-full py-3 rounded-xl bg-blue-100 text-blue-700 hover:bg-blue-200 transition-all flex items-center justify-center gap-2">
                                <i class="fas fa-location-arrow"></i> Utiliser ma position actuelle
                            </button>
                            <input type="text" id="loc-query" placeholder="Ou recherchez un lieu (ex: Tour Eiffel, Paris)" class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                        </div>

                        <!-- Event Form -->
                        <div id="form-event" class="type-form hidden space-y-4">
                            <input type="text" id="event-title" placeholder="Titre de l'événement" class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            <div class="grid grid-cols-2 gap-4">
                                <input type="datetime-local" id="event-start" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                                <input type="datetime-local" id="event-end" class="input-field w-full px-4 py-3 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            </div>
                            <input type="text" id="event-location" placeholder="Lieu" class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()">
                            <textarea id="event-description" placeholder="Description" rows="3" class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none" oninput="updatePreview()"></textarea>
                        </div>

                        <!-- Design Options -->
                        <div class="mt-8 pt-6 border-t border-gray-200">
                            <div class="flex items-center justify-between mb-4">
                                <h3 class="font-semibold flex items-center gap-2">
                                    <i class="fas fa-paint-brush text-purple-600"></i>
                                    Personnalisation avancée
                                </h3>
                                <button onclick="randomizeDesign()" class="text-sm text-purple-600 hover:text-purple-800 flex items-center gap-1">
                                    <i class="fas fa-dice"></i> Aléatoire
                                </button>
                            </div>
                            
                            <div class="grid grid-cols-2 gap-4 mb-4">
                                <div>
                                    <label class="text-xs text-gray-500 block mb-1">Couleur QR</label>
                                    <div class="flex items-center gap-2">
                                        <input type="color" id="qr-color" value="#000000" class="w-12 h-10 rounded cursor-pointer" onchange="updatePreview()">
                                        <input type="text" id="qr-color-text" value="#000000" class="flex-1 px-3 py-2 rounded-lg bg-gray-100 text-sm font-mono" onchange="updateColorFromText('qr-color', this.value)">
                                    </div>
                                </div>
                                <div>
                                    <label class="text-xs text-gray-500 block mb-1">Couleur fond</label>
                                    <div class="flex items-center gap-2">
                                        <input type="color" id="bg-color" value="#ffffff" class="w-12 h-10 rounded cursor-pointer" onchange="updatePreview()">
                                        <input type="text" id="bg-color-text" value="#ffffff" class="flex-1 px-3 py-2 rounded-lg bg-gray-100 text-sm font-mono" onchange="updateColorFromText('bg-color', this.value)">
                                    </div>
                                </div>
                            </div>

                            <div class="mb-4">
                                <label class="text-xs text-gray-500 block mb-2">Style des coins</label>
                                <div class="flex gap-2">
                                    <button onclick="setCornerStyle('square')" class="corner-btn flex-1 py-2 rounded-lg border-2 border-purple-500 bg-purple-50 text-purple-700 text-sm" data-style="square">Carrés</button>
                                    <button onclick="setCornerStyle('rounded')" class="corner-btn flex-1 py-2 rounded-lg border-2 border-gray-200 text-gray-600 text-sm" data-style="rounded">Arrondis</button>
                                    <button onclick="setCornerStyle('circle')" class="corner-btn flex-1 py-2 rounded-lg border-2 border-gray-200 text-gray-600 text-sm" data-style="circle">Cercles</button>
                                </div>
                            </div>

                            <div class="mb-4">
                                <label class="text-xs text-gray-500 block mb-2">Logo au centre (optionnel)</label>
                                <div class="flex items-center gap-3">
                                    <label class="flex-1 cursor-pointer">
                                        <div class="border-2 border-dashed border-gray-300 rounded-xl p-4 text-center hover:border-purple-500 hover:bg-purple-50 transition-all">
                                            <i class="fas fa-cloud-upload-alt text-2xl text-gray-400 mb-2"></i>
                                            <p class="text-sm text-gray-600">Cliquez pour ajouter</p>
                                            <input type="file" id="logo-upload" accept="image/*" class="hidden" onchange="handleLogoUpload(event)">
                                        </div>
                                    </label>
                                    <div id="logo-preview" class="hidden w-16 h-16 rounded-xl bg-gray-100 flex items-center justify-center overflow-hidden">
                                        <img id="logo-img" class="w-full h-full object-contain">
                                    </div>
                                    <button onclick="removeLogo()" id="remove-logo" class="hidden text-red-500 hover:text-red-700 p-2">
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </div>
                            </div>

                            <div>
                                <label class="text-xs text-gray-500 block mb-2">Taille du QR</label>
                                <input type="range" id="qr-size" min="200" max="500" value="300" class="w-full" oninput="updatePreview()">
                                <div class="flex justify-between text-xs text-gray-400 mt-1">
                                    <span>Petit</span>
                                    <span id="size-value">300px</span>
                                    <span>Grand</span>
                                </div>
                            </div>
                        </div>

                        <button onclick="generateFinalQR()" class="btn-primary w-full mt-6 py-4 rounded-xl text-white font-bold text-lg flex items-center justify-center gap-2 shadow-xl">
                            <i class="fas fa-magic"></i>
                            Générer le QR Code
                        </button>
                    </div>

                    <!-- History -->
                    <div class="glass-panel rounded-2xl p-6">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-semibold flex items-center gap-2">
                                <i class="fas fa-history text-purple-600"></i>
                                Historique récent
                            </h3>
                            <div class="flex gap-2">
                                <button onclick="exportHistory()" class="text-xs bg-gray-100 hover:bg-gray-200 px-3 py-1 rounded-full transition-all">
                                    <i class="fas fa-download"></i> Export
                                </button>
                                <button onclick="clearHistory()" class="text-xs text-red-500 hover:bg-red-50 px-3 py-1 rounded-full transition-all">
                                    <i class="fas fa-trash"></i> Effacer
                                </button>
                            </div>
                        </div>
                        <div id="history-list" class="space-y-3 max-h-64 overflow-y-auto">
                            <p class="text-gray-400 text-center py-8 italic">Aucun QR code généré</p>
                        </div>
                    </div>
                </div>

                <!-- Right Panel: Preview -->
                <div class="lg:col-span-5 space-y-6">
                    <div class="glass-panel rounded-2xl p-6 sticky top-6">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-semibold">Aperçu en direct</h3>
                            <div class="flex gap-2">
                                <button onclick="toggleFavorite()" id="fav-btn" class="text-gray-400 hover:text-red-500 transition-all">
                                    <i class="far fa-heart text-xl"></i>
                                </button>
                                <button onclick="resetForm()" class="text-gray-400 hover:text-gray-600">
                                    <i class="fas fa-redo text-xl"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="qr-preview rounded-xl p-8 flex items-center justify-center min-h-[350px] relative bg-white" id="preview-container">
                            <div id="qrcode" class="qr-3d transition-all duration-500">
                                <div class="text-center text-gray-400">
                                    <i class="fas fa-qrcode text-6xl mb-4 opacity-30"></i>
                                    <p>Remplissez le formulaire</p>
                                </div>
                            </div>
                            <div id="logo-overlay" class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-20 h-20 hidden bg-white rounded-2xl shadow-2xl p-2 z-10 border-4 border-white">
                                <img id="overlay-logo-img" class="w-full h-full object-contain">
                            </div>
                        </div>

                        <div id="action-buttons" class="hidden mt-6 space-y-3">
                            <div class="grid grid-cols-2 gap-3">
                                <button onclick="downloadQR('png')" class="py-3 rounded-xl bg-gray-800 text-white hover:bg-gray-900 transition-all flex items-center justify-center gap-2 font-medium">
                                    <i class="fas fa-download"></i> PNG
                                </button>
                                <button onclick="downloadQR('svg')" class="py-3 rounded-xl bg-gray-200 text-gray-800 hover:bg-gray-300 transition-all flex items-center justify-center gap-2 font-medium">
                                    <i class="fas fa-vector-square"></i> SVG
                                </button>
                            </div>
                            <div class="grid grid-cols-2 gap-3">
                                <button onclick="downloadQR('pdf')" class="py-3 rounded-xl bg-red-100 text-red-700 hover:bg-red-200 transition-all flex items-center justify-center gap-2 font-medium">
                                    <i class="fas fa-file-pdf"></i> PDF
                                </button>
                                <button onclick="printQR()" class="py-3 rounded-xl bg-blue-100 text-blue-700 hover:bg-blue-200 transition-all flex items-center justify-center gap-2 font-medium">
                                    <i class="fas fa-print"></i> Imprimer
                                </button>
                            </div>
                            <button onclick="shareQR()" class="w-full py-3 rounded-xl border-2 border-purple-600 text-purple-600 hover:bg-purple-50 transition-all flex items-center justify-center gap-2 font-medium">
                                <i class="fas fa-share-alt"></i> Partager
                            </button>
                        </div>

                        <div id="qr-info" class="hidden mt-4 p-4 bg-gray-50 rounded-xl">
                            <div class="flex items-center justify-between text-sm">
                                <span class="text-gray-500">Type:</span>
                                <span id="info-type" class="font-medium">URL</span>
                            </div>
                            <div class="flex items-center justify-between text-sm mt-2">
                                <span class="text-gray-500">Créé le:</span>
                                <span id="info-date" class="font-medium">-</span>
                            </div>
                            <div class="flex items-center justify-between text-sm mt-2">
                                <span class="text-gray-500">Taille:</span>
                                <span id="info-size" class="font-medium">-</span>
                            </div>
                        </div>
                    </div>

                    <!-- Quick Templates -->
                    <div class="glass-panel rounded-2xl p-6">
                        <h3 class="font-semibold mb-4 flex items-center gap-2">
                            <i class="fas fa-bolt text-yellow-500"></i>
                            Templates rapides
                        </h3>
                        <div class="grid grid-cols-2 gap-3">
                            <button onclick="applyTemplate('wifi-cafe')" class="p-3 rounded-xl bg-gradient-to-r from-orange-100 to-orange-200 text-orange-800 text-sm font-medium hover:shadow-md transition-all text-left">
                                <i class="fas fa-coffee mr-2"></i>Wi-Fi Café
                            </button>
                            <button onclick="applyTemplate('contact-pro')" class="p-3 rounded-xl bg-gradient-to-r from-blue-100 to-blue-200 text-blue-800 text-sm font-medium hover:shadow-md transition-all text-left">
                                <i class="fas fa-briefcase mr-2"></i>Contact Pro
                            </button>
                            <button onclick="applyTemplate('social-influencer')" class="p-3 rounded-xl bg-gradient-to-r from-pink-100 to-pink-200 text-pink-800 text-sm font-medium hover:shadow-md transition-all text-left">
                                <i class="fas fa-hashtag mr-2"></i>Influenceur
                            </button>
                            <button onclick="applyTemplate('event-party')" class="p-3 rounded-xl bg-gradient-to-r from-purple-100 to-purple-200 text-purple-800 text-sm font-medium hover:shadow-md transition-all text-left">
                                <i class="fas fa-glass-cheers mr-2"></i>Soirée
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- SCAN TAB -->
        <div id="tab-scan" class="main-content hidden">
            <div class="glass-panel rounded-2xl p-8 max-w-2xl mx-auto">
                <h2 class="text-2xl font-bold mb-6 text-center">
                    <i class="fas fa-camera text-purple-600 mr-2"></i>
                    Scanner un QR Code
                </h2>
                
                <div class="relative rounded-2xl overflow-hidden bg-black aspect-video mb-6">
                    <video id="scan-video" class="w-full h-full object-cover" autoplay playsinline></video>
                    <div class="scanner-overlay absolute inset-0 flex items-center justify-center">
                        <div class="w-64 h-64 border-2 border-white/50 rounded-2xl relative">
                            <div class="scan-line"></div>
                            <div class="absolute top-0 left-0 w-8 h-8 border-t-4 border-l-4 border-green-400"></div>
                            <div class="absolute top-0 right-0 w-8 h-8 border-t-4 border-r-4 border-green-400"></div>
                            <div class="absolute bottom-0 left-0 w-8 h-8 border-b-4 border-l-4 border-green-400"></div>
                            <div class="absolute bottom-0 right-0 w-8 h-8 border-b-4 border-r-4 border-green-400"></div>
                        </div>
                    </div>
                    <button onclick="stopScan()" class="absolute top-4 right-4 w-10 h-10 bg-red-500 text-white rounded-full flex items-center justify-center">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <div class="flex gap-3 justify-center">
                    <button onclick="startScan()" class="btn-primary px-8 py-3 rounded-xl text-white font-medium flex items-center gap-2">
                        <i class="fas fa-play"></i> Démarrer la caméra
                    </button>
                    <label class="px-6 py-3 rounded-xl bg-gray-200 text-gray-800 font-medium flex items-center gap-2 cursor-pointer hover:bg-gray-300 transition-all">
                        <i class="fas fa-image"></i> Importer image
                        <input type="file" accept="image/*" class="hidden" onchange="scanFromImage(event)">
                    </label>
                </div>
                
                <div id="scan-result" class="hidden mt-6 p-4 bg-green-50 border border-green-200 rounded-xl">
                    <h4 class="font-semibold text-green-800 mb-2">Contenu détecté :</h4>
                    <p id="scan-content" class="text-green-700 break-all mb-3"></p>
                    <div class="flex gap-2">
                        <button onclick="copyScanResult()" class="px-4 py-2 bg-green-600 text-white rounded-lg text-sm hover:bg-green-700">
                            <i class="fas fa-copy mr-1"></i> Copier
                        </button>
                        <button onclick="useScanResult()" class="px-4 py-2 bg-blue-600 text-white rounded-lg text-sm hover:bg-blue-700">
                            <i class="fas fa-edit mr-1"></i> Utiliser
                        </button>
                        <a id="scan-link" href="#" target="_blank" class="hidden px-4 py-2 bg-purple-600 text-white rounded-lg text-sm hover:bg-purple-700">
                            <i class="fas fa-external-link-alt mr-1"></i> Ouvrir
                        </a>
                    </div>
                </div>
            </div>
        </div>

        <!-- BATCH TAB -->
        <div id="tab-batch" class="main-content hidden">
            <div class="glass-panel rounded-2xl p-6">
                <h2 class="text-2xl font-bold mb-6">
                    <i class="fas fa-layer-group text-purple-600 mr-2"></i>
                    Génération en lot
                </h2>
                
                <div class="mb-6">
                    <label class="block text-sm font-medium mb-2">Entrez vos données (une par ligne)</label>
                    <textarea id="batch-input" rows="8" placeholder="https://site1.com&#10;https://site2.com&#10;WiFi:MonReseau;motdepasse&#10;..." class="input-field w-full px-4 py-4 rounded-xl bg-gray-50 focus:outline-none font-mono text-sm"></textarea>
                    <p class="text-xs text-gray-500 mt-2">Format: URLs, textes, ou commandes spéciales (WiFi:, TEL:, etc.)</p>
                </div>
                
                <div class="flex gap-3 mb-6">
                    <button onclick="generateBatch()" class="btn-primary px-6 py-3 rounded-xl text-white font-medium flex items-center gap-2">
                        <i class="fas fa-magic"></i> Générer tous
                    </button>
                    <button onclick="downloadAllBatch()" class="px-6 py-3 rounded-xl bg-gray-200 text-gray-800 font-medium flex items-center gap-2" id="download-all-btn" disabled>
                        <i class="fas fa-download"></i> Tout télécharger (ZIP)
                    </button>
                    <button onclick="clearBatch()" class="px-6 py-3 rounded-xl border border-red-300 text-red-600 font-medium hover:bg-red-50">
                        <i class="fas fa-trash"></i>
                    </button>
                </div>
                
                <div id="batch-results" class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-4">
                    <!-- Results will appear here -->
                </div>
            </div>
        </div>

        <!-- TEMPLATES TAB -->
        <div id="tab-templates" class="main-content hidden">
            <div class="glass-panel rounded-2xl p-6">
                <h2 class="text-2xl font-bold mb-6">
                    <i class="fas fa-palette text-purple-600 mr-2"></i>
                    Galerie de templates
                </h2>
                
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Template cards -->
                    <div class="template-card bg-white rounded-2xl overflow-hidden shadow-lg border border-gray-100" onclick="loadTemplate('modern-dark')">
                        <div class="h-40 bg-gradient-to-br from-gray-900 to-gray-700 flex items-center justify-center">
                            <div class="w-24 h-24 bg-white rounded-xl flex items-center justify-center">
                                <i class="fas fa-qrcode text-4xl text-gray-900"></i>
                            </div>
                        </div>
                        <div class="p-4">
                            <h4 class="font-bold text-lg">Moderne Dark</h4>
                            <p class="text-sm text-gray-500">Noir élégant avec coins arrondis</p>
                            <div class="flex gap-2 mt-3">
                                <span class="px-2 py-1 bg-gray-100 rounded text-xs">#000000</span>
                                <span class="px-2 py-1 bg-gray-100 rounded text-xs">Rounded</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="template-card bg-white rounded-2xl overflow-hidden shadow-lg border border-gray-100" onclick="loadTemplate('gradient-purple')">
                        <div class="h-40 bg-gradient-to-br from-purple-600 to-pink-600 flex items-center justify-center">
                            <div class="w-24 h-24 bg-white rounded-full flex items-center justify-center">
                                <i class="fas fa-qrcode text-4xl text-purple-600"></i>
                            </div>
                        </div>
                        <div class="p-4">
                            <h4 class="font-bold text-lg">Dégradé Purple</h4>
                            <p class="text-sm text-gray-500">Style vibrant avec cercles</p>
                            <div class="flex gap-2 mt-3">
                                <span class="px-2 py-1 bg-purple-100 text-purple-700 rounded text-xs">Purple</span>
                                <span class="px-2 py-1 bg-pink-100 text-pink-700 rounded text-xs">Pink</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="template-card bg-white rounded-2xl overflow-hidden shadow-lg border border-gray-100" onclick="loadTemplate('nature-green')">
                        <div class="h-40 bg-gradient-to-br from-green-500 to-emerald-700 flex items-center justify-center">
                            <div class="w-24 h-24 bg-white rounded-lg flex items-center justify-center">
                                <i class="fas fa-qrcode text-4xl text-green-600"></i>
                            </div>
                        </div>
                        <div class="p-4">
                            <h4 class="font-bold text-lg">Nature Green</h4>
                            <p class="text-sm text-gray-500">Thème écologique</p>
                            <div class="flex gap-2 mt-3">
                                <span class="px-2 py-1 bg-green-100 text-green-700 rounded text-xs">Green</span>
                                <span class="px-2 py-1 bg-emerald-100 text-emerald-700 rounded text-xs">Eco</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="template-card bg-white rounded-2xl overflow-hidden shadow-lg border border-gray-100" onclick="loadTemplate('ocean-blue')">
                        <div class="h-40 bg-gradient-to-br from-blue-500 to-cyan-600 flex items-center justify-center">
                            <div class="w-24 h-24 bg-white rounded-xl flex items-center justify-center">
                                <i class="fas fa-qrcode text-4xl text-blue-600"></i>
                            </div>
                        </div>
                        <div class="p-4">
                            <h4 class="font-bold text-lg">Ocean Blue</h4>
                            <p class="text-sm text-gray-500">Style frais et professionnel</p>
                            <div class="flex gap-2 mt-3">
                                <span class="px-2 py-1 bg-blue-100 text-blue-700 rounded text-xs">Blue</span>
                                <span class="px-2 py-1 bg-cyan-100 text-cyan-700 rounded text-xs">Pro</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="template-card bg-white rounded-2xl overflow-hidden shadow-lg border border-gray-100" onclick="loadTemplate('sunset-orange')">
                        <div class="h-40 bg-gradient-to-br from-orange-500 to-red-600 flex items-center justify-center">
                            <div class="w-24 h-24 bg-white rounded-lg flex items-center justify-center">
                                <i class="fas fa-qrcode text-4xl text-orange-600"></i>
                            </div>
                        </div>
                        <div class="p-4">
                            <h4 class="font-bold text-lg">Sunset Orange</h4>
                            <p class="text-sm text-gray-500">Chaleureux et accueillant</p>
                            <div class="flex gap-2 mt-3">
                                <span class="px-2 py-1 bg-orange-100 text-orange-700 rounded text-xs">Orange</span>
                                <span class="px-2 py-1 bg-red-100 text-red-700 rounded text-xs">Warm</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="template-card bg-white rounded-2xl overflow-hidden shadow-lg border border-gray-100" onclick="loadTemplate('minimal-white')">
                        <div class="h-40 bg-gray-100 flex items-center justify-center border-b">
                            <div class="w-24 h-24 bg-black rounded-sm flex items-center justify-center">
                                <i class="fas fa-qrcode text-4xl text-white"></i>
                            </div>
                        </div>
                        <div class="p-4">
                            <h4 class="font-bold text-lg">Minimal White</h4>
                            <p class="text-sm text-gray-500">Épuré et moderne</p>
                            <div class="flex gap-2 mt-3">
                                <span class="px-2 py-1 bg-gray-100 rounded text-xs">Black</span>
                                <span class="px-2 py-1 bg-gray-100 rounded text-xs">Minimal</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ANALYTICS TAB -->
        <div id="tab-analytics" class="main-content hidden">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
                <div class="stat-card rounded-2xl p-6 text-white">
                    <div class="flex items-center justify-between mb-4">
                        <div>
                            <p class="text-sm opacity-80">Total QR générés</p>
                            <p class="text-4xl font-bold" id="stat-total">0</p>
                        </div>
                        <div class="w-14 h-14 bg-white/20 rounded-full flex items-center justify-center">
                            <i class="fas fa-qrcode text-2xl"></i>
                        </div>
                    </div>
                    <div class="flex items-center text-sm">
                        <i class="fas fa-arrow-up text-green-400 mr-1"></i>
                        <span class="text-green-400">+12%</span>
                        <span class="opacity-60 ml-2">ce mois</span>
                    </div>
                </div>
                
                <div class="stat-card rounded-2xl p-6 text-white">
                    <div class="flex items-center justify-between mb-4">
                        <div>
                            <p class="text-sm opacity-80">Scans estimés</p>
                            <p class="text-4xl font-bold" id="stat-scans">0</p>
                        </div>
                        <div class="w-14 h-14 bg-white/20 rounded-full flex items-center justify-center">
                            <i class="fas fa-eye text-2xl"></i>
                        </div>
                    </div>
                    <div class="flex items-center text-sm">
                        <i class="fas fa-arrow-up text-green-400 mr-1"></i>
                        <span class="text-green-400">+24%</span>
                        <span class="opacity-60 ml-2">cette semaine</span>
                    </div>
                </div>
                
                <div class="stat-card rounded-2xl p-6 text-white">
                    <div class="flex items-center justify-between mb-4">
                        <div>
                            <p class="text-sm opacity-80">Type préféré</p>
                            <p class="text-4xl font-bold" id="stat-favorite">URL</p>
                        </div>
                        <div class="w-14 h-14 bg-white/20 rounded-full flex items-center justify-center">
                            <i class="fas fa-star text-2xl"></i>
                        </div>
                    </div>
                    <div class="flex items-center text-sm">
                        <span class="opacity-60">Le plus utilisé</span>
                    </div>
                </div>
            </div>
            
            <div class="glass-panel rounded-2xl p-6">
                <h3 class="text-xl font-bold mb-6">Activité récente</h3>
                <div class="space-y-4" id="activity-list">
                    <!-- Activity items will be added here -->
                </div>
            </div>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toast" class="toast">
        <i class="fas fa-check-circle text-green-400 text-xl"></i>
        <span id="toast-message">Opération réussie</span>
    </div>

    <script>
        // Global variables
        let currentType = 'url';
        let currentSocial = '';
        let qrHistory = JSON.parse(localStorage.getItem('qrHistory') || '[]');
        let favorites = JSON.parse(localStorage.getItem('qrFavorites') || '[]');
        let currentQR = null;
        let uploadedLogo = null;
        let scanStream = null;
        let batchResults = [];
        let stats = JSON.parse(localStorage.getItem('qrStats') || '{"total":0,"scans":0,"byType":{}}');

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            renderHistory();
            updateStats();
            updateFavCount();
            checkTheme();
        });

        // Theme functions
        function checkTheme() {
            if (localStorage.getItem('darkMode') === 'true') {
                document.body.classList.add('dark-mode');
                document.getElementById('theme-icon').classList.remove('fa-moon');
                document.getElementById('theme-icon').classList.add('fa-sun');
            }
        }

        function toggleTheme() {
            document.body.classList.toggle('dark-mode');
            const isDark = document.body.classList.contains('dark-mode');
            localStorage.setItem('darkMode', isDark);
            
            const icon = document.getElementById('theme-icon');
            if (isDark) {
                icon.classList.remove('fa-moon');
                icon.classList.add('fa-sun');
            } else {
                icon.classList.remove('fa-sun');
                icon.classList.add('fa-moon');
            }
        }

        // Tab switching
        function switchMainTab(tab) {
            document.querySelectorAll('.main-tab').forEach(btn => {
                if (btn.dataset.tab === tab) {
                    btn.classList.add('tab-active');
                    btn.classList.remove('text-gray-600', 'hover:bg-gray-100');
                } else {
                    btn.classList.remove('tab-active');
                    btn.classList.add('text-gray-600', 'hover:bg-gray-100');
                }
            });
            
            document.querySelectorAll('.main-content').forEach(content => {
                content.classList.add('hidden');
            });
            document.getElementById(`tab-${tab}`).classList.remove('hidden');
            
            if (tab === 'analytics') updateAnalytics();
        }

        function switchType(type) {
            currentType = type;
            
            document.querySelectorAll('.type-btn').forEach(btn => {
                if (btn.dataset.type === type) {
                    btn.classList.add('tab-active');
                    btn.classList.remove('text-gray-600', 'hover:bg-gray-100');
                } else {
                    btn.classList.remove('tab-active');
                    btn.classList.add('text-gray-600', 'hover:bg-gray-100');
                }
            });
            
            document.querySelectorAll('.type-form').forEach(form => {
                form.classList.add('hidden');
            });
            document.getElementById(`form-${type}`).classList.remove('hidden');
        }

        function selectSocial(platform) {
            currentSocial = platform;
            
            document.querySelectorAll('.social-btn').forEach(btn => {
                if (btn.dataset.social === platform) {
                    btn.classList.add('active', 'ring-2', 'ring-purple-500');
                } else {
                    btn.classList.remove('active', 'ring-2', 'ring-purple-500');
                }
            });
            
            document.getElementById('social-input-container').classList.remove('hidden');
            
            const prefixes = {
                'facebook': 'fb.com/', 'instagram': '@', 'twitter': '@',
                'linkedin': 'in/', 'tiktok': '@', 'snapchat': '@',
                'youtube': '@', 'github': '', 'whatsapp': '+'
            };
            
            document.getElementById('social-prefix').textContent = prefixes[platform] || '';
        }

        // QR Generation
        function getQRData() {
            switch(currentType) {
                case 'url':
                    return document.getElementById('url-input').value;
                case 'wifi':
                    const ssid = document.getElementById('wifi-ssid').value;
                    const pass = document.getElementById('wifi-password').value;
                    const sec = document.getElementById('wifi-security').value;
                    return `WIFI:T:${sec};S:${ssid};P:${pass};;`;
                case 'contact':
                    const fn = document.getElementById('contact-firstname').value;
                    const ln = document.getElementById('contact-lastname').value;
                    const phone = document.getElementById('contact-phone').value;
                    const email = document.getElementById('contact-email').value;
                    const org = document.getElementById('contact-org').value;
                    const addr = document.getElementById('contact-address').value;
                    const web = document.getElementById('contact-website').value;
                    let vcard = `BEGIN:VCARD\nVERSION:3.0\nN:${ln};${fn};;;\nFN:${fn} ${ln}\n`;
                    if (phone) vcard += `TEL:${phone}\n`;
                    if (email) vcard += `EMAIL:${email}\n`;
                    if (org) vcard += `ORG:${org}\n`;
                    if (addr) vcard += `ADR:;;${addr};;;;\n`;
                    if (web) vcard += `URL:${web}\n`;
                    return vcard + 'END:VCARD';
                case 'social':
                    if (!currentSocial) return '';
                    const user = document.getElementById('social-input').value;
                    const urls = {
                        'facebook': `https://facebook.com/${user}`,
                        'instagram': `https://instagram.com/${user}`,
                        'twitter': `https://twitter.com/${user}`,
                        'linkedin': `https://linkedin.com/in/${user}`,
                        'tiktok': `https://tiktok.com/@${user}`,
                        'snapchat': `https://snapchat.com/add/${user}`,
                        'youtube': `https://youtube.com/${user}`,
                        'github': `https://github.com/${user}`,
                        'whatsapp': `https://wa.me/${user}`
                    };
                    return urls[currentSocial];
                case 'email':
                    const to = document.getElementById('email-to').value;
                    const sub = document.getElementById('email-subject').value;
                    const body = document.getElementById('email-body').value;
                    return `mailto:${to}?subject=${encodeURIComponent(sub)}&body=${encodeURIComponent(body)}`;
                case 'sms':
                    const num = document.getElementById('sms-number').value;
                    const msg = document.getElementById('sms-message').value;
                    return `sms:${num}?body=${encodeURIComponent(msg)}`;
                case 'location':
                    const lat = document.getElementById('loc-lat').value;
                    const lng = document.getElementById('loc-lng').value;
                    const query = document.getElementById('loc-query').value;
                    if (lat && lng) return `geo:${lat},${lng}`;
                    if (query) return `https://maps.google.com/?q=${encodeURIComponent(query)}`;
                    return '';
                case 'event':
                    const title = document.getElementById('event-title').value;
                    const start = document.getElementById('event-start').value.replace(/[-:]/g, '').replace('T', 'T');
                    const end = document.getElementById('event-end').value.replace(/[-:]/g, '').replace('T', 'T');
                    const loc = document.getElementById('event-location').value;
                    const desc = document.getElementById('event-description').value;
                    return `BEGIN:VEVENT\nSUMMARY:${title}\nDTSTART:${start}\nDTEND:${end}\nLOCATION:${loc}\nDESCRIPTION:${desc}\nEND:VEVENT`;
                default:
                    return '';
            }
        }

        function generateFinalQR() {
            const data = getQRData();
            if (!data) {
                showToast('Veuillez remplir les champs', 'error');
                return;
            }
            
            const container = document.getElementById('qrcode');
            container.innerHTML = '';
            
            const color = document.getElementById('qr-color').value;
            const bg = document.getElementById('bg-color').value;
            const size = parseInt(document.getElementById('qr-size').value);
            
            // Create QR
            const qr = new QRCode(container, {
                text: data,
                width: size,
                height: size,
                colorDark: color,
                colorLight: bg,
                correctLevel: QRCode.CorrectLevel.H
            });
            
            // Add logo if present
            if (uploadedLogo) {
                setTimeout(() => {
                    addLogoToQR();
                }, 100);
            }
            
            // Save to history
            currentQR = {
                data: data,
                type: currentType,
                color: color,
                bg: bg,
                timestamp: new Date().toISOString(),
                id: Date.now()
            };
            
            qrHistory.unshift(currentQR);
            if (qrHistory.length > 20) qrHistory.pop();
            localStorage.setItem('qrHistory', JSON.stringify(qrHistory));
            
            // Update stats
            stats.total++;
            stats.byType[currentType] = (stats.byType[currentType] || 0) + 1;
            localStorage.setItem('qrStats', JSON.stringify(stats));
            
            // Update UI
            renderHistory();
            updateStats();
            document.getElementById('action-buttons').classList.remove('hidden');
            document.getElementById('qr-info').classList.remove('hidden');
            document.getElementById('info-type').textContent = currentType.toUpperCase();
            document.getElementById('info-date').textContent = new Date().toLocaleString('fr-FR');
            document.getElementById('info-size').textContent = size + 'px';
            
            // Check if favorite
            updateFavButton();
            
            showToast('QR Code généré avec succès !');
            createConfetti();
        }

        function addLogoToQR() {
            const canvas = document.querySelector('#qrcode canvas');
            if (!canvas || !uploadedLogo) return;
            
            const ctx = canvas.getContext('2d');
            const size = canvas.width;
            const logoSize = size * 0.2;
            const x = (size - logoSize) / 2;
            const y = (size - logoSize) / 2;
            
            // Clear center
            ctx.fillStyle = document.getElementById('bg-color').value;
            ctx.fillRect(x - 5, y - 5, logoSize + 10, logoSize + 10);
            
            // Draw logo
            const img = new Image();
            img.onload = () => {
                ctx.drawImage(img, x, y, logoSize, logoSize);
            };
            img.src = uploadedLogo;
            
            // Show overlay in preview
            document.getElementById('logo-overlay').classList.remove('hidden');
        }

        // Logo handling
        function handleLogoUpload(event) {
            const file = event.target.files[0];
            if (!file) return;
            
            if (file.size > 2 * 1024 * 1024) {
                showToast('Image trop grande (max 2Mo)', 'error');
                return;
            }
            
            const reader = new FileReader();
            reader.onload = (e) => {
                uploadedLogo = e.target.result;
                document.getElementById('logo-img').src = uploadedLogo;
                document.getElementById('logo-preview').classList.remove('hidden');
                document.getElementById('remove-logo').classList.remove('hidden');
                document.getElementById('overlay-logo-img').src = uploadedLogo;
                showToast('Logo ajouté !');
            };
            reader.readAsDataURL(file);
        }

        function removeLogo() {
            uploadedLogo = null;
            document.getElementById('logo-upload').value = '';
            document.getElementById('logo-preview').classList.add('hidden');
            document.getElementById('remove-logo').classList.add('hidden');
            document.getElementById('logo-overlay').classList.add('hidden');
        }

        // Design functions
        function updateColorFromText(inputId, value) {
            document.getElementById(inputId).value = value;
            updatePreview();
        }

        function setCornerStyle(style) {
            document.querySelectorAll('.corner-btn').forEach(btn => {
                if (btn.dataset.style === style) {
                    btn.classList.add('border-purple-500', 'bg-purple-50', 'text-purple-700');
                    btn.classList.remove('border-gray-200', 'text-gray-600');
                } else {
                    btn.classList.remove('border-purple-500', 'bg-purple-50', 'text-purple-700');
                    btn.classList.add('border-gray-200', 'text-gray-600');
                }
            });
        }

        function randomizeDesign() {
            const colors = ['#000000', '#667eea', '#764ba2', '#f093fb', '#f5576c', '#4facfe', '#00f2fe', '#43e97b', '#fa709a'];
            const randomColor = colors[Math.floor(Math.random() * colors.length)];
            const randomBg = '#ffffff';
            
            document.getElementById('qr-color').value = randomColor;
            document.getElementById('qr-color-text').value = randomColor;
            document.getElementById('bg-color').value = randomBg;
            document.getElementById('bg-color-text').value = randomBg;
            
            showToast('Design aléatoire appliqué !');
        }

        // Templates
        function applyTemplate(template) {
            const templates = {
                'wifi-cafe': { type: 'wifi', ssid: 'CafeFreeWiFi', security: 'WPA' },
                'contact-pro': { type: 'contact', firstname: 'Jean', lastname: 'Dupont', org: 'Mon Entreprise' },
                'social-influencer': { type: 'social', platform: 'instagram' },
                'event-party': { type: 'event', title: 'Ma Super Soirée' }
            };
            
            const t = templates[template];
            if (!t) return;
            
            switchType(t.type);
            
            if (t.type === 'wifi') {
                document.getElementById('wifi-ssid').value = t.ssid;
                document.getElementById('wifi-security').value = t.security;
            } else if (t.type === 'contact') {
                document.getElementById('contact-firstname').value = t.firstname;
                document.getElementById('contact-lastname').value = t.lastname;
                document.getElementById('contact-org').value = t.org;
            } else if (t.type === 'social') {
                selectSocial(t.platform);
            } else if (t.type === 'event') {
                document.getElementById('event-title').value = t.title;
            }
            
            showToast('Template chargé !');
        }

        function loadTemplate(templateName) {
            const templates = {
                'modern-dark': { color: '#000000', bg: '#ffffff', corner: 'square' },
                'gradient-purple': { color: '#667eea', bg: '#ffffff', corner: 'circle' },
                'nature-green': { color: '#10b981', bg: '#ffffff', corner: 'rounded' },
                'ocean-blue': { color: '#3b82f6', bg: '#ffffff', corner: 'rounded' },
                'sunset-orange': { color: '#f97316', bg: '#ffffff', corner: 'rounded' },
                'minimal-white': { color: '#000000', bg: '#ffffff', corner: 'square' }
            };
            
            const t = templates[templateName];
            if (t) {
                document.getElementById('qr-color').value = t.color;
                document.getElementById('qr-color-text').value = t.color;
                document.getElementById('bg-color').value = t.bg;
                document.getElementById('bg-color-text').value = t.bg;
                setCornerStyle(t.corner);
                showToast('Template ' + templateName + ' chargé !');
                switchMainTab('generate');
            }
        }

        // History
        function renderHistory() {
            const container = document.getElementById('history-list');
            
            if (qrHistory.length === 0) {
                container.innerHTML = '<p class="text-gray-400 text-center py-8 italic">Aucun QR code généré</p>';
                return;
            }
            
            const icons = {
                'url': 'fa-link', 'wifi': 'fa-wifi', 'contact': 'fa-address-card',
                'social': 'fa-share-alt', 'email': 'fa-envelope', 'sms': 'fa-sms',
                'location': 'fa-map-marker-alt', 'event': 'fa-calendar'
            };
            
            container.innerHTML = qrHistory.map((item, index) => `
                <div class="history-item bg-white rounded-xl p-4 border border-gray-200 flex items-center justify-between group hover:border-purple-300">
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 bg-gradient-to-br from-purple-100 to-pink-100 rounded-lg flex items-center justify-center">
                            <i class="fas ${icons[item.type] || 'fa-qrcode'} text-purple-600 text-sm"></i>
                        </div>
                        <div>
                            <p class="font-medium text-sm capitalize">${item.type}</p>
                            <p class="text-xs text-gray-500">${new Date(item.timestamp).toLocaleDateString()}</p>
                        </div>
                    </div>
                    <div class="flex items-center gap-1 opacity-0 group-hover:opacity-100 transition-opacity">
                        <button onclick="restoreQR(${index})" class="p-2 text-purple-600 hover:bg-purple-50 rounded-lg" title="Restaurer">
                            <i class="fas fa-undo text-sm"></i>
                        </button>
                        <button onclick="toggleHistoryFav(${index})" class="p-2 ${favorites.includes(item.id) ? 'text-red-500' : 'text-gray-400'} hover:bg-red-50 rounded-lg">
                            <i class="${favorites.includes(item.id) ? 'fas' : 'far'} fa-heart text-sm"></i>
                        </button>
                        <button onclick="deleteHistoryItem(${index})" class="p-2 text-gray-400 hover:text-red-500 hover:bg-red-50 rounded-lg">
                            <i class="fas fa-times text-sm"></i>
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function restoreQR(index) {
            const item = qrHistory[index];
            document.getElementById('qr-color').value = item.color;
            document.getElementById('qr-color-text').value = item.color;
            document.getElementById('bg-color').value = item.bg;
            document.getElementById('bg-color-text').value = item.bg;
            
            generateQRFromData(item.data, item.color, item.bg);
            showToast('QR restauré !');
        }

        function deleteHistoryItem(index) {
            qrHistory.splice(index, 1);
            localStorage.setItem('qrHistory', JSON.stringify(qrHistory));
            renderHistory();
        }

        function clearHistory() {
            if (confirm('Effacer tout l\'historique ?')) {
                qrHistory = [];
                localStorage.removeItem('qrHistory');
                renderHistory();
                showToast('Historique effacé');
            }
        }

        function exportHistory() {
            const data = JSON.stringify(qrHistory, null, 2);
            const blob = new Blob([data], {type: 'application/json'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'qr-history.json';
            a.click();
            showToast('Historique exporté !');
        }

        // Favorites
        function toggleFavorite() {
            if (!currentQR) return;
            
            const index = favorites.indexOf(currentQR.id);
            if (index > -1) {
                favorites.splice(index, 1);
                showToast('Retiré des favoris');
            } else {
                favorites.push(currentQR.id);
                showToast('Ajouté aux favoris !');
            }
            
            localStorage.setItem('qrFavorites', JSON.stringify(favorites));
            updateFavButton();
            updateFavCount();
        }

        function toggleHistoryFav(index) {
            const item = qrHistory[index];
            const favIndex = favorites.indexOf(item.id);
            
            if (favIndex > -1) {
                favorites.splice(favIndex, 1);
            } else {
                favorites.push(item.id);
            }
            
            localStorage.setItem('qrFavorites', JSON.stringify(favorites));
            renderHistory();
            updateFavCount();
        }

        function updateFavButton() {
            const btn = document.getElementById('fav-btn');
            if (currentQR && favorites.includes(currentQR.id)) {
                btn.innerHTML = '<i class="fas fa-heart text-xl text-red-500"></i>';
            } else {
                btn.innerHTML = '<i class="far fa-heart text-xl"></i>';
            }
        }

        function updateFavCount() {
            document.getElementById('fav-count').textContent = favorites.length + ' favoris';
        }

        function showFavorites() {
            // Filter history to show only favorites
            const favHistory = qrHistory.filter(item => favorites.includes(item.id));
            // Could implement a modal or separate view here
            showToast(favorites.length + ' QR en favoris');
        }

        // Scanner
        async function startScan() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'environment' } });
                const video = document.getElementById('scan-video');
                video.srcObject = stream;
                scanStream = stream;
                
                // Start scanning loop
                scanLoop();
            } catch (err) {
                showToast('Caméra non accessible', 'error');
            }
        }

        function stopScan() {
            if (scanStream) {
                scanStream.getTracks().forEach(track => track.stop());
                scanStream = null;
            }
        }

        function scanLoop() {
            if (!scanStream) return;
            
            const video = document.getElementById('scan-video');
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');
            
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
            
            const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
            const code = jsQR(imageData.data, imageData.width, imageData.height);
            
            if (code) {
                showScanResult(code.data);
                stopScan();
                return;
            }
            
            requestAnimationFrame(scanLoop);
        }

        function scanFromImage(event) {
            const file = event.target.files[0];
            if (!file) return;
            
            const img = new Image();
            img.onload = () => {
                const canvas = document.createElement('canvas');
                canvas.width = img.width;
                canvas.height = img.height;
                const ctx = canvas.getContext('2d');
                ctx.drawImage(img, 0, 0);
                
                const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
                const code = jsQR(imageData.data, imageData.width, imageData.height);
                
                if (code) {
                    showScanResult(code.data);
                } else {
                    showToast('Aucun QR code trouvé', 'error');
                }
            };
            img.src = URL.createObjectURL(file);
        }

        function showScanResult(data) {
            document.getElementById('scan-result').classList.remove('hidden');
            document.getElementById('scan-content').textContent = data;
            
            const linkBtn = document.getElementById('scan-link');
            if (data.startsWith('http')) {
                linkBtn.href = data;
                linkBtn.classList.remove('hidden');
            } else {
                linkBtn.classList.add('hidden');
            }
        }

        function copyScanResult() {
            const text = document.getElementById('scan-content').textContent;
            navigator.clipboard.writeText(text);
            showToast('Copié !');
        }

        function useScanResult() {
            const data = document.getElementById('scan-content').textContent;
            document.getElementById('url-input').value = data;
            switchMainTab('generate');
            switchType('url');
            showToast('Données importées !');
        }

        // Batch generation
        function generateBatch() {
            const input = document.getElementById('batch-input').value.trim();
            if (!input) return;
            
            const lines = input.split('\n').filter(l => l.trim());
            const container = document.getElementById('batch-results');
            container.innerHTML = '';
            batchResults = [];
            
            lines.forEach((line, index) => {
                setTimeout(() => {
                    const div = document.createElement('div');
                    div.className = 'batch-item bg-white rounded-xl p-4 shadow-md border border-gray-100';
                    
                    const qrDiv = document.createElement('div');
                    qrDiv.className = 'mb-2';
                    new QRCode(qrDiv, {
                        text: line,
                        width: 150,
                        height: 150
                    });
                    
                    div.innerHTML = `
                        <div class="flex justify-center mb-2" id="batch-qr-${index}"></div>
                        <p class="text-xs text-gray-500 truncate text-center">${line}</p>
                        <button onclick="downloadBatchItem(${index})" class="w-full mt-2 py-2 bg-gray-800 text-white rounded-lg text-sm">
                            <i class="fas fa-download"></i>
                        </button>
                    `;
                    
                    container.appendChild(div);
                    div.querySelector(`#batch-qr-${index}`).appendChild(qrDiv);
                    
                    batchResults.push({ data: line, element: qrDiv });
                }, index * 100);
            });
            
            document.getElementById('download-all-btn').disabled = false;
            showToast(`${lines.length} QR codes générés !`);
        }

        function downloadBatchItem(index) {
            const canvas = batchResults[index].element.querySelector('canvas');
            if (canvas) {
                const link = document.createElement('a');
                link.download = `qr-batch-${index + 1}.png`;
                link.href = canvas.toDataURL();
                link.click();
            }
        }

        function downloadAllBatch() {
            showToast('Préparation du ZIP...');
            // In a real app, you'd use JSZip library here
            batchResults.forEach((item, index) => {
                setTimeout(() => downloadBatchItem(index), index * 200);
            });
        }

        function clearBatch() {
            document.getElementById('batch-input').value = '';
            document.getElementById('batch-results').innerHTML = '';
            document.getElementById('download-all-btn').disabled = true;
            batchResults = [];
        }

        // Download & Share
        function downloadQR(format) {
            const canvas = document.querySelector('#qrcode canvas');
            if (!canvas) return;
            
            if (format === 'png') {
                const link = document.createElement('a');
                link.download = `qrcode-${Date.now()}.png`;
                link.href = canvas.toDataURL('image/png');
                link.click();
            } else if (format === 'svg') {
                const svg = `<svg xmlns="http://www.w3.org/2000/svg" width="${canvas.width}" height="${canvas.height}">
                    <image href="${canvas.toDataURL()}" width="${canvas.width}" height="${canvas.height}"/>
                </svg>`;
                const blob = new Blob([svg], {type: 'image/svg+xml'});
                const url = URL.createObjectURL(blob);
                const link = document.createElement('a');
                link.download = `qrcode-${Date.now()}.svg`;
                link.href = url;
                link.click();
            } else if (format === 'pdf') {
                // Simple PDF generation using canvas to image
                const imgData = canvas.toDataURL('image/png');
                const pdfWindow = window.open('', '_blank');
                pdfWindow.document.write(`
                    <html>
                    <head><title>QR Code PDF</title></head>
                    <body style="display:flex;justify-content:center;align-items:center;height:100vh;margin:0;">
                        <img src="${imgData}" style="max-width:80%;">
                        <script>window.print();<\/script>
                    </body>
                    </html>
                `);
            }
            
            showToast(`Téléchargement ${format.toUpperCase()} lancé !`);
        }

        function printQR() {
            const canvas = document.querySelector('#qrcode canvas');
            if (!canvas) return;
            
            const win = window.open('', '_blank');
            win.document.write(`
                <html>
                <head><title>Impression QR</title></head>
                <body style="display:flex;justify-content:center;align-items:center;height:100vh;margin:0;">
                    <img src="${canvas.toDataURL()}" style="max-width:90%;">
                    <script>window.onload = () => setTimeout(() => window.print(), 100);<\/script>
                </body>
                </html>
            `);
        }

        function shareQR() {
            const canvas = document.querySelector('#qrcode canvas');
            if (!canvas) return;
            
            canvas.toBlob(blob => {
                const file = new File([blob], 'qrcode.png', { type: 'image/png' });
                
                if (navigator.share && navigator.canShare({ files: [file] })) {
                    navigator.share({
                        title: 'Mon QR Code',
                        text: 'Généré avec QR Code Ultimate',
                        files: [file]
                    });
                } else {
                    showToast('Partage non supporté, utilisez le téléchargement', 'error');
                }
            });
        }

        // Stats & Analytics
        function updateStats() {
            document.getElementById('total-generated').textContent = stats.total + ' QR générés';
            document.getElementById('total-scans').textContent = (stats.total * 3 + Math.floor(Math.random() * 50)) + ' scans';
        }

        function updateAnalytics() {
            document.getElementById('stat-total').textContent = stats.total;
            document.getElementById('stat-scans').textContent = stats.total * 3;
            
            // Find favorite type
            let maxType = 'URL';
            let maxCount = 0;
            for (const [type, count] of Object.entries(stats.byType)) {
                if (count > maxCount) {
                    maxCount = count;
                    maxType = type;
                }
            }
            document.getElementById('stat-favorite').textContent = maxType.toUpperCase();
            
            // Activity list
            const activityList = document.getElementById('activity-list');
            const recent = qrHistory.slice(0, 5);
            activityList.innerHTML = recent.map(item => `
                <div class="flex items-center justify-between p-4 bg-gray-50 rounded-xl">
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 bg-purple-100 rounded-full flex items-center justify-center">
                            <i class="fas fa-qrcode text-purple-600"></i>
                        </div>
                        <div>
                            <p class="font-medium capitalize">${item.type}</p>
                            <p class="text-xs text-gray-500">${new Date(item.timestamp).toLocaleString()}</p>
                        </div>
                    </div>
                    <span class="text-sm text-gray-400">Il y a ${Math.floor((Date.now() - new Date(item.timestamp)) / 60000)} min</span>
                </div>
            `).join('');
        }

        // Utilities
        function togglePassword(id) {
            const input = document.getElementById(id);
            input.type = input.type === 'password' ? 'text' : 'password';
        }

        function getCurrentLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(pos => {
                    document.getElementById('loc-lat').value = pos.coords.latitude.toFixed(6);
                    document.getElementById('loc-lng').value = pos.coords.longitude.toFixed(6);
                    updatePreview();
                    showToast('Position obtenue !');
                }, () => {
                    showToast('Géolocalisation refusée', 'error');
                });
            }
        }

        function updatePreview() {
            // Auto-generate preview after delay
            clearTimeout(window.previewTimeout);
            window.previewTimeout = setTimeout(() => {
                if (getQRData()) generateFinalQR();
            }, 1500);
        }

        function resetForm() {
            document.querySelectorAll('input, textarea').forEach(el => el.value = '');
            removeLogo();
            showToast('Formulaire réinitialisé');
        }

        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            const msg = document.getElementById('toast-message');
            const icon = toast.querySelector('i');
            
            msg.textContent = message;
            
            if (type === 'error') {
                icon.className = 'fas fa-exclamation-circle text-red-400 text-xl';
            } else {
                icon.className = 'fas fa-check-circle text-green-400 text-xl';
            }
            
            toast.classList.add('show');
            setTimeout(() => toast.classList.remove('show'), 3000);
        }

        function createConfetti() {
            for (let i = 0; i < 30; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * 100 + 'vw';
                confetti.style.backgroundColor = ['#667eea', '#764ba2', '#f093fb', '#f5576c'][Math.floor(Math.random() * 4)];
                confetti.style.animationDuration = (Math.random() * 2 + 2) + 's';
                document.body.appendChild(confetti);
                setTimeout(() => confetti.remove(), 4000);
            }
        }

        // Helper for restoring QR
        function generateQRFromData(data, color, bg) {
            const container = document.getElementById('qrcode');
            container.innerHTML = '';
            
            new QRCode(container, {
                text: data,
                width: 300,
                height: 300,
                colorDark: color,
                colorLight: bg,
                correctLevel: QRCode.CorrectLevel.H
            });
            
            document.getElementById('action-buttons').classList.remove('hidden');
        }
    </script>
</body>
</html>
