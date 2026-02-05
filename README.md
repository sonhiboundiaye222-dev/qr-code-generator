<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Ultimate Pro - Cartes de Visite & Tout-en-Un</title>
    
    <!-- Libraries -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jsQR/1.4.0/jsQR.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=Poppins:wght@400;500;600;700&display=swap');
        
        * { font-family: 'Inter', sans-serif; }
        
        :root {
            --primary: #667eea;
            --secondary: #764ba2;
            --accent: #f093fb;
        }
        
        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        
        .glass {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.3);
            box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
        }
        
        .dark .glass {
            background: rgba(15, 15, 35, 0.95);
            border-color: rgba(255, 255, 255, 0.1);
            color: white;
        }
        
        .btn-gradient {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            transition: all 0.3s ease;
        }
        
        .btn-gradient:hover {
            transform: translateY(-2px);
            box-shadow: 0 15px 30px -5px rgba(102, 126, 234, 0.4);
        }
        
        .tab-active {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
        }
        
        .card-hover {
            transition: all 0.3s ease;
        }
        
        .card-hover:hover {
            transform: translateY(-5px);
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
        }
        
        .business-card {
            aspect-ratio: 1.586;
            position: relative;
            overflow: hidden;
            transition: all 0.3s ease;
        }
        
        .business-card:hover {
            transform: scale(1.02) rotateY(5deg);
        }
        
        .card-template-1 {
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            color: white;
        }
        
        .card-template-2 {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        
        .card-template-3 {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
        }
        
        .card-template-4 {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
        }
        
        .card-template-5 {
            background: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%);
            color: #1a1a2e;
        }
        
        .card-template-6 {
            background: linear-gradient(135deg, #fa709a 0%, #fee140 100%);
            color: #1a1a2e;
        }
        
        .card-template-7 {
            background: #ffffff;
            color: #1a1a2e;
            border: 2px solid #e5e7eb;
        }
        
        .card-template-8 {
            background: linear-gradient(135deg, #2c3e50 0%, #34495e 100%);
            color: white;
        }
        
        .qr-preview {
            background-image: 
                linear-gradient(45deg, #f0f0f0 25%, transparent 25%), 
                linear-gradient(-45deg, #f0f0f0 25%, transparent 25%), 
                linear-gradient(45deg, transparent 75%, #f0f0f0 75%), 
                linear-gradient(-45deg, transparent 75%, #f0f0f0 75%);
            background-size: 20px 20px;
        }
        
        .input-animate {
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }
        
        .input-animate:focus {
            transform: translateY(-2px);
            box-shadow: 0 10px 30px -5px rgba(102, 126, 234, 0.3);
            border-color: var(--primary);
        }
        
        .scanner-line {
            position: absolute;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, transparent, #00ff00, transparent);
            box-shadow: 0 0 20px #00ff00;
            animation: scan 2s linear infinite;
        }
        
        @keyframes scan {
            0% { top: 0; opacity: 0; }
            10% { opacity: 1; }
            90% { opacity: 1; }
            100% { top: 100%; opacity: 0; }
        }
        
        .toast {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%) translateY(100px);
            background: #1a1a2e;
            color: white;
            padding: 16px 28px;
            border-radius: 16px;
            opacity: 0;
            transition: all 0.4s ease;
            z-index: 10000;
        }
        
        .toast.show {
            transform: translateX(-50%) translateY(0);
            opacity: 1;
        }
        
        .floating {
            animation: floating 3s ease-in-out infinite;
        }
        
        @keyframes floating {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }
        
        .confetti {
            position: fixed;
            width: 10px; height: 10px;
            pointer-events: none;
            z-index: 9999;
        }
        
        .progress-bar {
            height: 4px;
            background: linear-gradient(90deg, var(--primary), var(--accent));
            border-radius: 2px;
            transition: width 0.3s ease;
        }
        
        .social-icon {
            transition: all 0.3s ease;
        }
        
        .social-icon:hover {
            transform: scale(1.2) rotate(10deg);
        }
        
        .contact-menu {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
            gap: 1rem;
        }
        
        .contact-item {
            aspect-ratio: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            border-radius: 1rem;
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .contact-item:hover {
            transform: translateY(-5px) scale(1.05);
        }
        
        .contact-item.active {
            ring: 3px solid var(--primary);
            transform: scale(1.1);
        }
    </style>
</head>
<body class="text-gray-800 dark:text-white transition-colors duration-300">
    
    <!-- Install Prompt -->
    <div id="install-prompt" class="fixed top-0 left-0 right-0 bg-gradient-to-r from-purple-600 to-pink-600 text-white p-4 z-50 hidden">
        <div class="max-w-6xl mx-auto flex items-center justify-between">
            <div class="flex items-center gap-3">
                <i class="fas fa-mobile-alt text-2xl"></i>
                <div>
                    <p class="font-bold">Installez l'application !</p>
                    <p class="text-sm opacity-90">Accès rapide hors-ligne</p>
                </div>
            </div>
            <div class="flex gap-2">
                <button onclick="installApp()" class="px-4 py-2 bg-white text-purple-600 rounded-lg font-bold">Installer</button>
                <button onclick="dismissInstall()" class="w-10 h-10 flex items-center justify-center hover:bg-white/20 rounded-lg"><i class="fas fa-times"></i></button>
            </div>
        </div>
    </div>

    <!-- Header -->
    <header class="relative overflow-hidden">
        <div class="absolute inset-0 bg-gradient-to-r from-purple-600/20 to-pink-600/20"></div>
        <div class="relative max-w-7xl mx-auto px-4 py-8 text-center">
            <div class="inline-flex items-center justify-center w-20 h-20 rounded-2xl bg-white/20 backdrop-blur-lg mb-6 floating shadow-2xl">
                <i class="fas fa-qrcode text-4xl text-white"></i>
            </div>
            <h1 class="text-5xl md:text-6xl font-black mb-4 text-white">
                QR Code <span class="bg-clip-text text-transparent bg-gradient-to-r from-yellow-300 to-pink-300">Ultimate</span>
            </h1>
            <p class="text-xl text-white/90 mb-8">Cartes de visite digitales, QR codes & Scanner tout-en-un</p>
            
            <!-- Stats -->
            <div class="flex flex-wrap justify-center gap-4">
                <div class="glass rounded-full px-6 py-3 flex items-center gap-3 text-white">
                    <i class="fas fa-check-circle text-green-400 text-xl"></i>
                    <div>
                        <p class="text-xs opacity-80">QR Générés</p>
                        <p class="text-xl font-bold" id="stat-total">0</p>
                    </div>
                </div>
                <div class="glass rounded-full px-6 py-3 flex items-center gap-3 text-white">
                    <i class="fas fa-id-card text-blue-400 text-xl"></i>
                    <div>
                        <p class="text-xs opacity-80">Cartes</p>
                        <p class="text-xl font-bold" id="stat-cards">0</p>
                    </div>
                </div>
                <div class="glass rounded-full px-6 py-3 flex items-center gap-3 text-white cursor-pointer" onclick="showFavorites()">
                    <i class="fas fa-heart text-red-400 text-xl"></i>
                    <div>
                        <p class="text-xs opacity-80">Favoris</p>
                        <p class="text-xl font-bold" id="stat-favs">0</p>
                    </div>
                </div>
                <button onclick="toggleTheme()" class="glass rounded-full w-12 h-12 flex items-center justify-center text-white hover:scale-110 transition-transform">
                    <i class="fas fa-moon text-xl" id="theme-icon"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Navigation -->
    <nav class="sticky top-0 z-40 px-4 py-4">
        <div class="max-w-6xl mx-auto glass rounded-2xl p-2 flex flex-wrap justify-center gap-2">
            <button onclick="switchTab('cards')" class="nav-btn tab-active px-6 py-3 rounded-xl font-semibold transition-all flex items-center gap-2" data-tab="cards">
                <i class="fas fa-id-card"></i>
                <span class="hidden sm:inline">Cartes de Visite</span>
            </button>
            <button onclick="switchTab('create')" class="nav-btn px-6 py-3 rounded-xl font-semibold text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 transition-all flex items-center gap-2" data-tab="create">
                <i class="fas fa-magic"></i>
                <span class="hidden sm:inline">QR Simple</span>
            </button>
            <button onclick="switchTab('wifi')" class="nav-btn px-6 py-3 rounded-xl font-semibold text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 transition-all flex items-center gap-2" data-tab="wifi">
                <i class="fas fa-wifi"></i>
                <span class="hidden sm:inline">Wi-Fi</span>
            </button>
            <button onclick="switchTab('scan')" class="nav-btn px-6 py-3 rounded-xl font-semibold text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 transition-all flex items-center gap-2" data-tab="scan">
                <i class="fas fa-camera"></i>
                <span class="hidden sm:inline">Scanner</span>
            </button>
            <button onclick="switchTab('batch')" class="nav-btn px-6 py-3 rounded-xl font-semibold text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 transition-all flex items-center gap-2" data-tab="batch">
                <i class="fas fa-layer-group"></i>
                <span class="hidden sm:inline">Lot</span>
            </button>
            <button onclick="switchTab('history')" class="nav-btn px-6 py-3 rounded-xl font-semibold text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 transition-all flex items-center gap-2" data-tab="history">
                <i class="fas fa-history"></i>
                <span class="hidden sm:inline">Historique</span>
            </button>
        </div>
    </nav>

    <!-- MAIN CONTENT -->
    <main class="max-w-7xl mx-auto px-4 py-8 pb-32">

        <!-- CARDS TAB - Menu de Contact Complet -->
        <section id="tab-cards" class="tab-content">
            
            <!-- Contact Menu Grid -->
            <div class="glass rounded-2xl p-8 mb-8">
                <h2 class="text-2xl font-bold mb-6 text-center">
                    <i class="fas fa-address-book text-purple-600 mr-2"></i>
                    Menu de Contact - Choisissez ce que vous voulez partager
                </h2>
                
                <div class="contact-menu max-w-4xl mx-auto">
                    <button onclick="setCardType('full')" class="contact-item active bg-gradient-to-br from-purple-500 to-pink-500 text-white shadow-lg" data-card="full">
                        <i class="fas fa-id-card text-3xl mb-2"></i>
                        <span class="text-sm font-medium">Carte Complète</span>
                    </button>
                    
                    <button onclick="setCardType('phone')" class="contact-item bg-blue-100 dark:bg-blue-900/30 text-blue-700 dark:text-blue-300 hover:bg-blue-200" data-card="phone">
                        <i class="fas fa-phone text-3xl mb-2"></i>
                        <span class="text-sm font-medium">Téléphone</span>
                    </button>
                    
                    <button onclick="setCardType('email')" class="contact-item bg-red-100 dark:bg-red-900/30 text-red-700 dark:text-red-300 hover:bg-red-200" data-card="email">
                        <i class="fas fa-envelope text-3xl mb-2"></i>
                        <span class="text-sm font-medium">Email</span>
                    </button>
                    
                    <button onclick="setCardType('sms')" class="contact-item bg-green-100 dark:bg-green-900/30 text-green-700 dark:text-green-300 hover:bg-green-200" data-card="sms">
                        <i class="fas fa-sms text-3xl mb-2"></i>
                        <span class="text-sm font-medium">SMS</span>
                    </button>
                    
                    <button onclick="setCardType('whatsapp')" class="contact-item bg-green-100 dark:bg-green-900/30 text-green-600 dark:text-green-400 hover:bg-green-200" data-card="whatsapp">
                        <i class="fab fa-whatsapp text-3xl mb-2"></i>
                        <span class="text-sm font-medium">WhatsApp</span>
                    </button>
                    
                    <button onclick="setCardType('social')" class="contact-item bg-pink-100 dark:bg-pink-900/30 text-pink-700 dark:text-pink-300 hover:bg-pink-200" data-card="social">
                        <i class="fas fa-share-alt text-3xl mb-2"></i>
                        <span class="text-sm font-medium">Réseaux</span>
                    </button>
                    
                    <button onclick="setCardType('location')" class="contact-item bg-emerald-100 dark:bg-emerald-900/30 text-emerald-700 dark:text-emerald-300 hover:bg-emerald-200" data-card="location">
                        <i class="fas fa-map-marker-alt text-3xl mb-2"></i>
                        <span class="text-sm font-medium">Adresse</span>
                    </button>
                    
                    <button onclick="setCardType('website')" class="contact-item bg-indigo-100 dark:bg-indigo-900/30 text-indigo-700 dark:text-indigo-300 hover:bg-indigo-200" data-card="website">
                        <i class="fas fa-globe text-3xl mb-2"></i>
                        <span class="text-sm font-medium">Site Web</span>
                    </button>
                    
                    <button onclick="setCardType('event')" class="contact-item bg-orange-100 dark:bg-orange-900/30 text-orange-700 dark:text-orange-300 hover:bg-orange-200" data-card="event">
                        <i class="fas fa-calendar-alt text-3xl mb-2"></i>
                        <span class="text-sm font-medium">Événement</span>
                    </button>
                    
                    <button onclick="setCardType('vcard')" class="contact-item bg-gray-100 dark:bg-gray-800 text-gray-700 dark:text-gray-300 hover:bg-gray-200" data-card="vcard">
                        <i class="fas fa-user-plus text-3xl mb-2"></i>
                        <span class="text-sm font-medium">vCard</span>
                    </button>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                
                <!-- Form Section -->
                <div class="space-y-6">
                    
                    <!-- Personal Info -->
                    <div class="glass rounded-2xl p-6 card-hover">
                        <h3 class="font-bold text-lg mb-4 flex items-center gap-2">
                            <i class="fas fa-user text-purple-600"></i>
                            Informations personnelles
                        </h3>
                        
                        <div class="flex items-center gap-4 mb-6">
                            <div class="relative">
                                <div id="photo-preview" class="w-24 h-24 rounded-full bg-gray-200 flex items-center justify-center overflow-hidden border-4 border-white shadow-lg">
                                    <i class="fas fa-user text-4xl text-gray-400"></i>
                                </div>
                                <label class="absolute bottom-0 right-0 w-8 h-8 bg-purple-600 text-white rounded-full flex items-center justify-center cursor-pointer hover:bg-purple-700 transition-all shadow-lg">
                                    <i class="fas fa-camera text-sm"></i>
                                    <input type="file" id="card-photo" accept="image/*" class="hidden" onchange="handleCardPhoto(event)">
                                </label>
                            </div>
                            <div class="flex-1">
                                <input type="text" id="card-name" placeholder="Nom complet" class="input-animate w-full px-4 py-3 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none mb-2 font-bold text-lg" oninput="updateCardPreview()">
                                <input type="text" id="card-title" placeholder="Titre / Fonction" class="input-animate w-full px-4 py-2 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none text-sm" oninput="updateCardPreview()">
                            </div>
                        </div>

                        <div class="grid grid-cols-2 gap-3 mb-4">
                            <input type="tel" id="card-phone" placeholder="Téléphone" class="input-animate w-full px-4 py-3 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none" oninput="updateCardPreview()">
                            <input type="email" id="card-email" placeholder="Email" class="input-animate w-full px-4 py-3 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none" oninput="updateCardPreview()">
                        </div>
                        
                        <input type="text" id="card-company" placeholder="Entreprise" class="input-animate w-full px-4 py-3 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none mb-3" oninput="updateCardPreview()">
                        
                        <textarea id="card-address" placeholder="Adresse complète" rows="2" class="input-animate w-full px-4 py-3 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none resize-none mb-3" oninput="updateCardPreview()"></textarea>
                        
                        <input type="url" id="card-website" placeholder="Site web" class="input-animate w-full px-4 py-3 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none mb-4" oninput="updateCardPreview()">

                        <!-- Social Links -->
                        <div class="space-y-2">
                            <p class="text-sm font-medium text-gray-600 dark:text-gray-400">Réseaux sociaux</p>
                            <div class="grid grid-cols-2 gap-2">
                                <div class="relative">
                                    <i class="fab fa-linkedin absolute left-3 top-1/2 -translate-y-1/2 text-blue-600"></i>
                                    <input type="text" id="card-linkedin" placeholder="LinkedIn" class="input-animate w-full pl-10 pr-3 py-2 rounded-lg bg-gray-50 dark:bg-gray-800 focus:outline-none text-sm" oninput="updateCardPreview()">
                                </div>
                                <div class="relative">
                                    <i class="fab fa-twitter absolute left-3 top-1/2 -translate-y-1/2 text-blue-400"></i>
                                    <input type="text" id="card-twitter" placeholder="Twitter" class="input-animate w-full pl-10 pr-3 py-2 rounded-lg bg-gray-50 dark:bg-gray-800 focus:outline-none text-sm" oninput="updateCardPreview()">
                                </div>
                                <div class="relative">
                                    <i class="fab fa-instagram absolute left-3 top-1/2 -translate-y-1/2 text-pink-600"></i>
                                    <input type="text" id="card-instagram" placeholder="Instagram" class="input-animate w-full pl-10 pr-3 py-2 rounded-lg bg-gray-50 dark:bg-gray-800 focus:outline-none text-sm" oninput="updateCardPreview()">
                                </div>
                                <div class="relative">
                                    <i class="fab fa-github absolute left-3 top-1/2 -translate-y-1/2 text-gray-800 dark:text-gray-200"></i>
                                    <input type="text" id="card-github" placeholder="GitHub" class="input-animate w-full pl-10 pr-3 py-2 rounded-lg bg-gray-50 dark:bg-gray-800 focus:outline-none text-sm" oninput="updateCardPreview()">
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Card Design -->
                    <div class="glass rounded-2xl p-6 card-hover">
                        <h3 class="font-bold text-lg mb-4 flex items-center gap-2">
                            <i class="fas fa-paint-brush text-purple-600"></i>
                            Design de la carte
                        </h3>
                        
                        <div class="grid grid-cols-4 gap-3 mb-4">
                            <button onclick="setCardTemplate(1)" class="h-16 rounded-xl card-template-1 shadow-md hover:scale-105 transition-transform border-2 border-transparent hover:border-white" data-template="1"></button>
                            <button onclick="setCardTemplate(2)" class="h-16 rounded-xl card-template-2 shadow-md hover:scale-105 transition-transform border-2 border-transparent hover:border-white" data-template="2"></button>
                            <button onclick="setCardTemplate(3)" class="h-16 rounded-xl card-template-3 shadow-md hover:scale-105 transition-transform border-2 border-transparent hover:border-white" data-template="3"></button>
                            <button onclick="setCardTemplate(4)" class="h-16 rounded-xl card-template-4 shadow-md hover:scale-105 transition-transform border-2 border-transparent hover:border-white" data-template="4"></button>
                            <button onclick="setCardTemplate(5)" class="h-16 rounded-xl card-template-5 shadow-md hover:scale-105 transition-transform border-2 border-transparent hover:border-gray-400" data-template="5"></button>
                            <button onclick="setCardTemplate(6)" class="h-16 rounded-xl card-template-6 shadow-md hover:scale-105 transition-transform border-2 border-transparent hover:border-gray-400" data-template="6"></button>
                            <button onclick="setCardTemplate(7)" class="h-16 rounded-xl card-template-7 shadow-md hover:scale-105 transition-transform border-2 border-transparent hover:border-gray-400" data-template="7"></button>
                            <button onclick="setCardTemplate(8)" class="h-16 rounded-xl card-template-8 shadow-md hover:scale-105 transition-transform border-2 border-transparent hover:border-white" data-template="8"></button>
                        </div>

                        <div class="flex gap-3">
                            <button onclick="generateCard()" class="btn-gradient flex-1 py-4 rounded-xl text-white font-bold text-lg shadow-lg flex items-center justify-center gap-2">
                                <i class="fas fa-magic"></i>
                                Créer la carte
                            </button>
                            <button onclick="downloadCard()" class="px-6 py-4 rounded-xl bg-gray-200 dark:bg-gray-700 text-gray-800 dark:text-white font-bold hover:bg-gray-300 transition-all">
                                <i class="fas fa-download"></i>
                            </button>
                        </div>
                    </div>

                </div>

                <!-- Preview Section -->
                <div class="space-y-6">
                    
                    <!-- Business Card Preview -->
                    <div class="glass rounded-2xl p-6 sticky top-24">
                        <h3 class="font-bold text-lg mb-4 flex items-center justify-between">
                            <span><i class="fas fa-eye text-purple-600 mr-2"></i>Aperçu de la carte</span>
                            <button onclick="toggleCardFav()" id="card-fav-btn" class="text-gray-400 hover:text-red-500 transition-all">
                                <i class="far fa-heart text-xl"></i>
                            </button>
                        </h3>

                        <div class="flex justify-center mb-6">
                            <div id="card-preview" class="business-card w-full max-w-md rounded-2xl shadow-2xl p-6 card-template-1 relative overflow-hidden">
                                <!-- Card Content -->
                                <div class="relative z-10 h-full flex flex-col justify-between">
                                    <div class="flex items-start justify-between">
                                        <div>
                                            <h4 id="preview-name" class="text-2xl font-bold mb-1">Votre Nom</h4>
                                            <p id="preview-title" class="text-sm opacity-80">Titre / Fonction</p>
                                        </div>
                                        <div id="preview-photo" class="w-16 h-16 rounded-full bg-white/20 flex items-center justify-center">
                                            <i class="fas fa-user text-2xl"></i>
                                        </div>
                                    </div>
                                    
                                    <div class="space-y-1 text-sm">
                                        <p id="preview-company" class="font-semibold opacity-90">Entreprise</p>
                                        <p id="preview-contact" class="opacity-80">
                                            <i class="fas fa-phone mr-2"></i>Téléphone<br>
                                            <i class="fas fa-envelope mr-2"></i>Email
                                        </p>
                                        <p id="preview-address" class="opacity-70 text-xs mt-2">Adresse</p>
                                    </div>

                                    <div class="flex justify-between items-end">
                                        <div class="flex gap-2" id="preview-socials">
                                            <i class="fab fa-linkedin social-icon"></i>
                                            <i class="fab fa-twitter social-icon"></i>
                                            <i class="fab fa-instagram social-icon"></i>
                                        </div>
                                        <div id="preview-qr" class="w-20 h-20 bg-white rounded-lg p-1">
                                            <div class="w-full h-full bg-gray-200 rounded flex items-center justify-center">
                                                <i class="fas fa-qrcode text-gray-400"></i>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                
                                <!-- Decorative elements -->
                                <div class="absolute top-0 right-0 w-32 h-32 bg-white/10 rounded-full -mr-16 -mt-16"></div>
                                <div class="absolute bottom-0 left-0 w-24 h-24 bg-white/10 rounded-full -ml-12 -mb-12"></div>
                            </div>
                        </div>

                        <!-- QR Code Detail -->
                        <div id="card-qr-detail" class="hidden mt-4 p-4 bg-gray-50 dark:bg-gray-800 rounded-xl">
                            <div class="flex justify-center mb-3">
                                <div id="card-qr-code"></div>
                            </div>
                            <div class="flex gap-2">
                                <button onclick="downloadCardQR('png')" class="flex-1 py-2 bg-gray-800 text-white rounded-lg text-sm">
                                    <i class="fas fa-download mr-1"></i> PNG
                                </button>
                                <button onclick="shareCard()" class="flex-1 py-2 bg-purple-600 text-white rounded-lg text-sm">
                                    <i class="fas fa-share-alt mr-1"></i> Partager
                                </button>
                            </div>
                        </div>
                    </div>

                    <!-- My Cards Gallery -->
                    <div class="glass rounded-2xl p-6">
                        <h3 class="font-bold text-lg mb-4 flex items-center justify-between">
                            <span><i class="fas fa-images text-purple-600 mr-2"></i>Mes cartes</span>
                            <button onclick="clearAllCards()" class="text-xs text-red-500 hover:underline">Tout effacer</button>
                        </h3>
                        <div id="my-cards-gallery" class="grid grid-cols-2 gap-3 max-h-64 overflow-y-auto">
                            <p class="text-gray-400 text-center py-8 col-span-2 italic">Aucune carte sauvegardée</p>
                        </div>
                    </div>

                </div>
            </div>
        </section>

        <!-- SIMPLE QR TAB -->
        <section id="tab-create" class="tab-content hidden">
            <!-- [Previous QR creation code - same as before] -->
            <div class="text-center py-12">
                <p class="text-gray-500">Utilisez l'onglet "Cartes de Visite" pour des QR codes complets, ou sélectionnez un type spécifique ci-dessous:</p>
                
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mt-8 max-w-4xl mx-auto">
                    <button onclick="switchTab('wifi')" class="glass p-6 rounded-2xl hover:shadow-xl transition-all">
                        <i class="fas fa-wifi text-4xl text-blue-500 mb-3"></i>
                        <p class="font-bold">Wi-Fi</p>
                        <p class="text-xs text-gray-500">Connexion rapide</p>
                    </button>
                    <button onclick="switchToSimpleQR('url')" class="glass p-6 rounded-2xl hover:shadow-xl transition-all">
                        <i class="fas fa-link text-4xl text-purple-500 mb-3"></i>
                        <p class="font-bold">URL</p>
                        <p class="text-xs text-gray-500">Lien web</p>
                    </button>
                    <button onclick="switchToSimpleQR('text')" class="glass p-6 rounded-2xl hover:shadow-xl transition-all">
                        <i class="fas fa-font text-4xl text-green-500 mb-3"></i>
                        <p class="font-bold">Texte</p>
                        <p class="text-xs text-gray-500">Message libre</p>
                    </button>
                    <button onclick="switchTab('batch')" class="glass p-6 rounded-2xl hover:shadow-xl transition-all">
                        <i class="fas fa-layer-group text-4xl text-orange-500 mb-3"></i>
                        <p class="font-bold">Lot</p>
                        <p class="text-xs text-gray-500">Plusieurs QR</p>
                    </button>
                </div>
            </div>
        </section>

        <!-- WIFI TAB - Special Section -->
        <section id="tab-wifi" class="tab-content hidden">
            <div class="max-w-2xl mx-auto">
                <div class="glass rounded-2xl p-8">
                    <div class="text-center mb-8">
                        <div class="w-20 h-20 bg-blue-100 dark:bg-blue-900/30 rounded-full flex items-center justify-center mx-auto mb-4">
                            <i class="fas fa-wifi text-4xl text-blue-600"></i>
                        </div>
                        <h2 class="text-3xl font-bold mb-2">QR Code Wi-Fi</h2>
                        <p class="text-gray-600 dark:text-gray-400">Vos invités se connectent en scannant, sans taper le mot de passe</p>
                    </div>

                    <div class="space-y-4 mb-6">
                        <input type="text" id="wifi-ssid-simple" placeholder="Nom du réseau (SSID)" class="input-animate w-full px-4 py-4 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none text-lg" oninput="generateWiFiQR()">
                        
                        <div class="relative">
                            <input type="password" id="wifi-password-simple" placeholder="Mot de passe" class="input-animate w-full px-4 py-4 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none text-lg" oninput="generateWiFiQR()">
                            <button onclick="togglePassword('wifi-password-simple')" class="absolute right-4 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-600">
                                <i class="fas fa-eye"></i>
                            </button>
                        </div>

                        <select id="wifi-security-simple" class="input-animate w-full px-4 py-4 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none" onchange="generateWiFiQR()">
                            <option value="WPA">WPA/WPA2 (Standard)</option>
                            <option value="WEP">WEP (Ancien)</option>
                            <option value="nopass">Sans mot de passe (Ouvert)</option>
                        </select>

                        <div class="flex items-center gap-3 p-4 bg-yellow-50 dark:bg-yellow-900/20 rounded-xl">
                            <i class="fas fa-lightbulb text-yellow-600 text-xl"></i>
                            <p class="text-sm text-yellow-800 dark:text-yellow-300">Astuce: Imprimez ce QR et affichez-le dans votre salon ou bureau pour un accès facile</p>
                        </div>
                    </div>

                    <div class="flex justify-center mb-6">
                        <div id="wifi-qr-preview" class="qr-preview p-8 rounded-2xl bg-white">
                            <div class="w-64 h-64 bg-gray-100 rounded-xl flex items-center justify-center">
                                <i class="fas fa-qrcode text-6xl text-gray-300"></i>
                            </div>
                        </div>
                    </div>

                    <div id="wifi-actions" class="hidden grid grid-cols-2 gap-3">
                        <button onclick="downloadWiFiQR('png')" class="btn-gradient py-3 rounded-xl text-white font-bold">
                            <i class="fas fa-download mr-2"></i> Télécharger
                        </button>
                        <button onclick="printWiFiQR()" class="py-3 rounded-xl bg-gray-200 dark:bg-gray-700 text-gray-800 dark:text-white font-bold hover:bg-gray-300 transition-all">
                            <i class="fas fa-print mr-2"></i> Imprimer
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- SCAN TAB -->
        <section id="tab-scan" class="tab-content hidden">
            <div class="max-w-3xl mx-auto">
                <div class="glass rounded-2xl p-8 text-center">
                    <h2 class="text-3xl font-bold mb-6 flex items-center justify-center gap-3">
                        <i class="fas fa-camera text-purple-600"></i>
                        Scanner un QR Code
                    </h2>
                    
                    <div class="relative rounded-2xl overflow-hidden bg-black aspect-video mb-6 shadow-2xl">
                        <video id="scan-video" class="w-full h-full object-cover" autoplay playsinline></video>
                        <div class="absolute inset-0 flex items-center justify-center pointer-events-none">
                            <div class="w-64 h-64 border-2 border-white/30 rounded-2xl relative">
                                <div class="scanner-line"></div>
                                <div class="absolute top-0 left-0 w-8 h-8 border-t-4 border-l-4 border-green-400 rounded-tl-lg"></div>
                                <div class="absolute top-0 right-0 w-8 h-8 border-t-4 border-r-4 border-green-400 rounded-tr-lg"></div>
                                <div class="absolute bottom-0 left-0 w-8 h-8 border-b-4 border-l-4 border-green-400 rounded-bl-lg"></div>
                                <div class="absolute bottom-0 right-0 w-8 h-8 border-b-4 border-r-4 border-green-400 rounded-br-lg"></div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="flex gap-3 justify-center">
                        <button onclick="startScan()" class="btn-gradient px-8 py-4 rounded-xl text-white font-bold text-lg flex items-center gap-3 shadow-xl">
                            <i class="fas fa-play"></i> Démarrer
                        </button>
                        <label class="px-6 py-4 rounded-xl bg-gray-200 dark:bg-gray-700 text-gray-800 dark:text-white font-bold flex items-center gap-3 cursor-pointer hover:bg-gray-300 transition-all shadow-lg">
                            <i class="fas fa-image"></i> Importer
                            <input type="file" accept="image/*" class="hidden" onchange="scanFromImage(event)">
                        </label>
                    </div>

                    <div id="scan-result" class="hidden mt-8 p-6 bg-green-50 dark:bg-green-900/20 border-2 border-green-200 dark:border-green-800 rounded-2xl text-left">
                        <h4 class="font-bold text-green-800 dark:text-green-300 mb-3">QR Code détecté !</h4>
                        <p id="scan-content" class="text-gray-800 dark:text-gray-200 break-all font-mono text-sm mb-4 p-3 bg-white dark:bg-gray-800 rounded-lg"></p>
                        <div class="flex gap-2 flex-wrap">
                            <button onclick="copyScanResult()" class="px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700">
                                <i class="fas fa-copy mr-1"></i> Copier
                            </button>
                            <button onclick="useScanResult()" class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
                                <i class="fas fa-edit mr-1"></i> Utiliser
                            </button>
                            <a id="scan-link" href="#" target="_blank" class="hidden px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700">
                                <i class="fas fa-external-link-alt mr-1"></i> Ouvrir
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- BATCH TAB -->
        <section id="tab-batch" class="tab-content hidden">
            <div class="glass rounded-2xl p-8">
                <h2 class="text-3xl font-bold mb-6 flex items-center gap-3">
                    <i class="fas fa-layer-group text-purple-600"></i>
                    Génération en lot
                </h2>
                
                <textarea id="batch-input" rows="8" placeholder="https://site1.com&#10;https://site2.com&#10;mailto:email@example.com&#10;tel:+33123456789&#10;..." class="input-animate w-full px-4 py-4 rounded-xl bg-gray-50 dark:bg-gray-800 focus:outline-none font-mono text-sm mb-4"></textarea>
                
                <div class="flex gap-3 mb-6">
                    <button onclick="generateBatch()" class="btn-gradient px-8 py-3 rounded-xl text-white font-bold shadow-lg">
                        <i class="fas fa-magic mr-2"></i> Générer
                    </button>
                    <button onclick="downloadBatchZIP()" id="batch-zip-btn" class="px-6 py-3 rounded-xl bg-gray-200 text-gray-800 font-bold opacity-50 cursor-not-allowed" disabled>
                        <i class="fas fa-file-zip mr-2"></i> ZIP
                    </button>
                </div>

                <div id="batch-results" class="grid grid-cols-2 sm:grid-cols-4 md:grid-cols-6 gap-4"></div>
            </div>
        </section>

        <!-- HISTORY TAB -->
        <section id="tab-history" class="tab-content hidden">
            <div class="glass rounded-2xl p-8">
                <h2 class="text-3xl font-bold mb-6">Historique complet</h2>
                <div id="full-history" class="space-y-3 max-h-96 overflow-y-auto">
                    <p class="text-gray-400 text-center py-8">Aucun élément dans l'historique</p>
                </div>
            </div>
        </section>

    </main>

    <!-- Toast -->
    <div id="toast" class="toast">
        <i class="fas fa-check-circle text-green-400 text-xl mr-3"></i>
        <span id="toast-message">Opération réussie</span>
    </div>

    <script>
        // Global State
        let currentCardType = 'full';
        let currentCardTemplate = 1;
        let cardPhoto = null;
        let myCards = JSON.parse(localStorage.getItem('myCards') || '[]');
        let scanStream = null;
        let qrHistory = JSON.parse(localStorage.getItem('qrHistory') || '[]');
        let favorites = JSON.parse(localStorage.getItem('favorites') || '[]');

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            updateStats();
            renderMyCards();
            renderHistory();
            updateCardPreview();
        });

        // Tab Switching
        function switchTab(tab) {
            document.querySelectorAll('.nav-btn').forEach(btn => {
                if (btn.dataset.tab === tab) {
                    btn.classList.add('tab-active');
                    btn.classList.remove('text-gray-600', 'dark:text-gray-300');
                } else {
                    btn.classList.remove('tab-active');
                    btn.classList.add('text-gray-600', 'dark:text-gray-300');
                }
            });
            
            document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'));
            document.getElementById(`tab-${tab}`).classList.remove('hidden');
            
            if (tab === 'history') renderHistory();
        }

        // Card Type Selection
        function setCardType(type) {
            currentCardType = type;
            
            document.querySelectorAll('.contact-item').forEach(btn => {
                if (btn.dataset.card === type) {
                    btn.classList.add('active');
                } else {
                    btn.classList.remove('active');
                }
            });
            
            updateCardPreview();
        }

        // Card Template Selection
        function setCardTemplate(template) {
            currentCardTemplate = template;
            
            const preview = document.getElementById('card-preview');
            preview.className = `business-card w-full max-w-md rounded-2xl shadow-2xl p-6 relative overflow-hidden card-template-${template}`;
            
            document.querySelectorAll('[data-template]').forEach(btn => {
                if (parseInt(btn.dataset.template) === template) {
                    btn.classList.add('ring-4', 'ring-white', 'scale-110');
                } else {
                    btn.classList.remove('ring-4', 'ring-white', 'scale-110');
                }
            });
        }

        // Photo Handling
        function handleCardPhoto(event) {
            const file = event.target.files[0];
            if (!file) return;
            
            const reader = new FileReader();
            reader.onload = (e) => {
                cardPhoto = e.target.result;
                document.getElementById('photo-preview').innerHTML = `<img src="${cardPhoto}" class="w-full h-full object-cover">`;
                updateCardPreview();
            };
            reader.readAsDataURL(file);
        }

        // Update Card Preview
        function updateCardPreview() {
            const name = document.getElementById('card-name').value || 'Votre Nom';
            const title = document.getElementById('card-title').value || 'Titre / Fonction';
            const company = document.getElementById('card-company').value || 'Entreprise';
            const phone = document.getElementById('card-phone').value;
            const email = document.getElementById('card-email').value;
            const address = document.getElementById('card-address').value;
            const website = document.getElementById('card-website').value;
            
            document.getElementById('preview-name').textContent = name;
            document.getElementById('preview-title').textContent = title;
            document.getElementById('preview-company').textContent = company;
            
            // Update contact info based on type
            let contactHTML = '';
            if (currentCardType === 'full' || currentCardType === 'phone') {
                if (phone) contactHTML += `<i class="fas fa-phone mr-2"></i>${phone}<br>`;
            }
            if (currentCardType === 'full' || currentCardType === 'email') {
                if (email) contactHTML += `<i class="fas fa-envelope mr-2"></i>${email}<br>`;
            }
            if (currentCardType === 'full' || currentCardType === 'website') {
                if (website) contactHTML += `<i class="fas fa-globe mr-2"></i>${website}<br>`;
            }
            if (currentCardType === 'sms') {
                contactHTML = '<i class="fas fa-sms mr-2"></i>Envoyer un SMS';
            }
            if (currentCardType === 'whatsapp') {
                contactHTML = '<i class="fab fa-whatsapp mr-2"></i>WhatsApp';
            }
            
            document.getElementById('preview-contact').innerHTML = contactHTML || '<i class="fas fa-info-circle mr-2"></i>Remplissez vos coordonnées';
            document.getElementById('preview-address').textContent = address || 'Adresse';
            
            // Photo
            if (cardPhoto) {
                document.getElementById('preview-photo').innerHTML = `<img src="${cardPhoto}" class="w-full h-full object-cover">`;
            }
            
            // Generate QR for preview
            generateCardQR();
        }

        // Generate Card QR
        function generateCardQR() {
            const name = document.getElementById('card-name').value;
            const phone = document.getElementById('card-phone').value;
            const email = document.getElementById('card-email').value;
            const website = document.getElementById('card-website').value;
            const address = document.getElementById('card-address').value;
            
            let qrData = '';
            
            switch(currentCardType) {
                case 'full':
                case 'vcard':
                    qrData = `BEGIN:VCARD\nVERSION:3.0\nFN:${name}\n`;
                    if (phone) qrData += `TEL:${phone}\n`;
                    if (email) qrData += `EMAIL:${email}\n`;
                    if (website) qrData += `URL:${website}\n`;
                    if (address) qrData += `ADR:;;${address};;;;\n`;
                    qrData += 'END:VCARD';
                    break;
                case 'phone':
                    qrData = `tel:${phone}`;
                    break;
                case 'email':
                    qrData = `mailto:${email}`;
                    break;
                case 'sms':
                    qrData = `sms:${phone}`;
                    break;
                case 'whatsapp':
                    qrData = `https://wa.me/${phone?.replace(/[^0-9]/g, '')}`;
                    break;
                case 'website':
                    qrData = website;
                    break;
                case 'location':
                    qrData = `geo:0,0?q=${encodeURIComponent(address)}`;
                    break;
                case 'event':
                    qrData = website || address;
                    break;
                case 'social':
                    const linkedin = document.getElementById('card-linkedin').value;
                    qrData = linkedin ? `https://linkedin.com/in/${linkedin}` : website;
                    break;
            }
            
            if (!qrData) return;
            
            // Generate mini QR for card preview
            const qrDiv = document.getElementById('preview-qr');
            qrDiv.innerHTML = '';
            new QRCode(qrDiv, {
                text: qrData,
                width: 80,
                height: 80,
                colorDark: currentCardTemplate === 7 ? '#000000' : '#000000',
                colorLight: '#ffffff',
                correctLevel: QRCode.CorrectLevel.H
            });
            
            return qrData;
        }

        // Generate Full Card
        function generateCard() {
            const qrData = generateCardQR();
            if (!qrData) {
                showToast('Veuillez remplir au moins un champ', 'error');
                return;
            }
            
            // Show full QR
            const detailDiv = document.getElementById('card-qr-detail');
            detailDiv.classList.remove('hidden');
            
            const qrContainer = document.getElementById('card-qr-code');
            qrContainer.innerHTML = '';
            new QRCode(qrContainer, {
                text: qrData,
                width: 200,
                height: 200,
                correctLevel: QRCode.CorrectLevel.H
            });
            
            // Save to my cards
            const card = {
                id: Date.now(),
                name: document.getElementById('card-name').value || 'Sans nom',
                template: currentCardTemplate,
                data: qrData,
                timestamp: new Date().toISOString()
            };
            
            myCards.unshift(card);
            if (myCards.length > 20) myCards.pop();
            localStorage.setItem('myCards', JSON.stringify(myCards));
            
            renderMyCards();
            updateStats();
            showToast('Carte créée avec succès !');
            createConfetti();
        }

        // Download Card
        async function downloadCard() {
            const cardElement = document.getElementById('card-preview');
            
            try {
                const canvas = await html2canvas(cardElement, {
                    scale: 2,
                    backgroundColor: null
                });
                
                const link = document.createElement('a');
                link.download = `carte-${Date.now()}.png`;
                link.href = canvas.toDataURL();
                link.click();
                
                showToast('Carte téléchargée !');
            } catch (err) {
                showToast('Erreur de génération', 'error');
            }
        }

        // Render My Cards
        function renderMyCards() {
            const container = document.getElementById('my-cards-gallery');
            
            if (myCards.length === 0) {
                container.innerHTML = '<p class="text-gray-400 text-center py-8 col-span-2 italic">Aucune carte sauvegardée</p>';
                return;
            }
            
            container.innerHTML = myCards.map((card, index) => `
                <div class="business-card rounded-xl p-3 card-template-${card.template} cursor-pointer hover:scale-105 transition-transform" onclick="loadCard(${index})">
                    <div class="h-full flex flex-col justify-between text-xs">
                        <p class="font-bold truncate">${card.name}</p>
                        <div class="w-8 h-8 bg-white/20 rounded flex items-center justify-center self-end">
                            <i class="fas fa-qrcode"></i>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // Load Card
        function loadCard(index) {
            const card = myCards[index];
            showToast(`Carte "${card.name}" chargée`);
        }

        // Clear Cards
        function clearAllCards() {
            if (confirm('Effacer toutes les cartes ?')) {
                myCards = [];
                localStorage.removeItem('myCards');
                renderMyCards();
                updateStats();
            }
        }

        // Wi-Fi QR Generation
        function generateWiFiQR() {
            const ssid = document.getElementById('wifi-ssid-simple').value;
            const password = document.getElementById('wifi-password-simple').value;
            const security = document.getElementById('wifi-security-simple').value;
            
            if (!ssid) return;
            
            const qrData = `WIFI:T:${security};S:${ssid};P:${password};;`;
            
            const container = document.getElementById('wifi-qr-preview');
            container.innerHTML = '';
            new QRCode(container, {
                text: qrData,
                width: 256,
                height: 256,
                correctLevel: QRCode.CorrectLevel.H
            });
            
            document.getElementById('wifi-actions').classList.remove('hidden');
        }

        // Scanner Functions
        async function startScan() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'environment' } });
                const video = document.getElementById('scan-video');
                video.srcObject = stream;
                scanStream = stream;
                
                scanLoop();
            } catch (err) {
                showToast('Caméra non accessible', 'error');
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

        function stopScan() {
            if (scanStream) {
                scanStream.getTracks().forEach(track => track.stop());
                scanStream = null;
            }
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
                    showToast('Aucun QR trouvé', 'error');
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

        // Batch Generation
        function generateBatch() {
            const input = document.getElementById('batch-input').value.trim();
            if (!input) return;
            
            const lines = input.split('\n').filter(l => l.trim());
            const container = document.getElementById('batch-results');
            container.innerHTML = '';
            
            lines.forEach((line, index) => {
                const div = document.createElement('div');
                div.className = 'bg-white dark:bg-gray-800 rounded-xl p-4 shadow-md';
                
                const qrDiv = document.createElement('div');
                new QRCode(qrDiv, {
                    text: line,
                    width: 120,
                    height: 120
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
            });
            
            document.getElementById('batch-zip-btn').disabled = false;
            document.getElementById('batch-zip-btn').classList.remove('opacity-50', 'cursor-not-allowed');
            
            showToast(`${lines.length} QR générés`);
        }

        // Utilities
        function togglePassword(id) {
            const input = document.getElementById(id);
            const icon = input.nextElementSibling.querySelector('i');
            if (input.type === 'password') {
                input.type = 'text';
                icon.classList.remove('fa-eye');
                icon.classList.add('fa-eye-slash');
            } else {
                input.type = 'password';
                icon.classList.remove('fa-eye-slash');
                icon.classList.add('fa-eye');
            }
        }

        function toggleTheme() {
            document.body.classList.toggle('dark');
            const icon = document.getElementById('theme-icon');
            if (document.body.classList.contains('dark')) {
                icon.classList.remove('fa-moon');
                icon.classList.add('fa-sun');
            } else {
                icon.classList.remove('fa-sun');
                icon.classList.add('fa-moon');
            }
        }

        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            const msg = document.getElementById('toast-message');
            msg.textContent = message;
            
            toast.classList.add('show');
            setTimeout(() => toast.classList.remove('show'), 3000);
        }

        function createConfetti() {
            for (let i = 0; i < 30; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * 100 + 'vw';
                confetti.style.backgroundColor = ['#667eea', '#764ba2', '#f093fb', '#f5576c'][Math.floor(Math.random() * 4)];
                confetti.style.animation = `confetti-fall ${Math.random() * 2 + 2}s linear forwards`;
                document.body.appendChild(confetti);
                setTimeout(() => confetti.remove(), 4000);
            }
        }

        function updateStats() {
            document.getElementById('stat-total').textContent = qrHistory.length;
            document.getElementById('stat-cards').textContent = myCards.length;
            document.getElementById('stat-favs').textContent = favorites.length;
        }

        function renderHistory() {
            // Implementation for full history
        }
    </script>
</body>
</html>
