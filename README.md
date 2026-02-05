<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Pro Generator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        .glass-panel {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        .tab-active {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        .qr-preview {
            background-image: 
                linear-gradient(45deg, #f0f0f0 25%, transparent 25%), 
                linear-gradient(-45deg, #f0f0f0 25%, transparent 25%), 
                linear-gradient(45deg, transparent 75%, #f0f0f0 75%), 
                linear-gradient(-45deg, transparent 75%, #f0f0f0 75%);
            background-size: 20px 20px;
            background-position: 0 0, 0 10px, 10px -10px, -10px 0px;
        }
        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            transition: all 0.3s ease;
        }
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px -5px rgba(102, 126, 234, 0.4);
        }
        .input-field {
            transition: all 0.3s ease;
        }
        .input-field:focus {
            transform: translateY(-1px);
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.15);
        }
    </style>
</head>
<body class="p-4 md:p-8">
    <div class="max-w-6xl mx-auto">
        <!-- Header -->
        <header class="text-center mb-8 text-white">
            <h1 class="text-4xl md:text-5xl font-bold mb-2">
                <i class="fas fa-qrcode mr-3"></i>QR Code Pro
            </h1>
            <p class="text-lg opacity-90">Générateur gratuit - Aucune donnée stockée</p>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
            <!-- Formulaire -->
            <div class="glass-panel rounded-2xl p-6">
                <!-- Tabs -->
                <div class="flex flex-wrap gap-2 mb-6">
                    <button onclick="switchTab('url')" class="tab-btn tab-active px-4 py-2 rounded-xl text-sm font-medium transition-all" data-tab="url">
                        <i class="fas fa-link mr-2"></i>URL
                    </button>
                    <button onclick="switchTab('wifi')" class="tab-btn px-4 py-2 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-all" data-tab="wifi">
                        <i class="fas fa-wifi mr-2"></i>Wi-Fi
                    </button>
                    <button onclick="switchTab('contact')" class="tab-btn px-4 py-2 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-all" data-tab="contact">
                        <i class="fas fa-address-card mr-2"></i>Contact
                    </button>
                    <button onclick="switchTab('text')" class="tab-btn px-4 py-2 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-all" data-tab="text">
                        <i class="fas fa-font mr-2"></i>Texte
                    </button>
                </div>

                <!-- URL Form -->
                <div id="form-url" class="tab-content space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">URL du site</label>
                        <input type="url" id="url-input" placeholder="https://example.com" 
                            class="input-field w-full px-4 py-3 rounded-xl border border-gray-300 focus:border-purple-500 focus:outline-none">
                    </div>
                </div>

                <!-- WiFi Form -->
                <div id="form-wifi" class="tab-content hidden space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">Nom du réseau (SSID)</label>
                        <input type="text" id="wifi-ssid" placeholder="Mon_WiFi" 
                            class="input-field w-full px-4 py-3 rounded-xl border border-gray-300 focus:border-purple-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">Mot de passe</label>
                        <input type="text" id="wifi-password" placeholder="Mot de passe" 
                            class="input-field w-full px-4 py-3 rounded-xl border border-gray-300 focus:border-purple-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">Type de sécurité</label>
                        <select id="wifi-security" class="input-field w-full px-4 py-3 rounded-xl border border-gray-300 focus:border-purple-500 focus:outline-none">
                            <option value="WPA">WPA/WPA2</option>
                            <option value="WEP">WEP</option>
                            <option value="nopass">Aucune</option>
                        </select>
                    </div>
                </div>

                <!-- Contact Form -->
                <div id="form-contact" class="tab-content hidden space-y-4">
                    <div class="grid grid-cols-2 gap-4">
                        <input type="text" id="contact-firstname" placeholder="Prénom" 
                            class="input-field w-full px-4 py-3 rounded-xl border border-gray-300 focus:border-purple-500 focus:outline-none">
                        <input type="text" id="contact-lastname" placeholder="Nom" 
                            class="input-field w-full px-4 py-3 rounded-xl border border-gray-300 focus:border-purple-500 focus:outline-none">
                    </div>
                    <input type="tel" id="contact-phone" placeholder="Téléphone" 
                        class="input-field w-full px-4 py-3 rounded-xl border border-gray-300 focus:border-purple-500 focus:outline-none">
                    <input type="email" id="contact-email" placeholder="Email" 
                        class="input-field w-full px-4 py-3 rounded-xl border border-gray-300 focus:border-purple-500 focus:outline-none">
                </div>

                <!-- Text Form -->
                <div id="form-text" class="tab-content hidden">
                    <textarea id="text-input" rows="4" placeholder="Votre texte ici..." 
                        class="input-field w-full px-4 py-3 rounded-xl border border-gray-300 focus:border-purple-500 focus:outline-none"></textarea>
                </div>

                <!-- Design Options -->
                <div class="mt-6 pt-6 border-t border-gray-200">
                    <h3 class="text-sm font-semibold text-gray-700 mb-3">Personnalisation</h3>
                    <div class="flex gap-4 mb-4">
                        <div class="flex-1">
                            <label class="text-xs text-gray-600 block mb-1">Couleur QR</label>
                            <input type="color" id="qr-color" value="#000000" class="w-full h-10 rounded cursor-pointer">
                        </div>
                        <div class="flex-1">
                            <label class="text-xs text-gray-600 block mb-1">Fond</label>
                            <input type="color" id="bg-color" value="#ffffff" class="w-full h-10 rounded cursor-pointer">
                        </div>
                    </div>
                </div>

                <button onclick="generateQR()" class="btn-primary w-full mt-6 py-4 rounded-xl text-white font-semibold text-lg flex items-center justify-center gap-2">
                    <i class="fas fa-magic"></i>
                    Générer le QR Code
                </button>
            </div>

            <!-- Preview -->
            <div class="glass-panel rounded-2xl p-6">
                <h3 class="text-lg font-semibold mb-4 text-gray-800">Aperçu</h3>
                <div class="qr-preview rounded-xl p-8 flex items-center justify-center min-h-[300px] bg-white" id="qr-container">
                    <div id="qrcode" class="text-center text-gray-400">
                        <i class="fas fa-qrcode text-6xl mb-4 opacity-30"></i>
                        <p>Cliquez sur "Générer" pour créer votre QR</p>
                    </div>
                </div>
                <div id="download-section" class="hidden mt-4 grid grid-cols-2 gap-3">
                    <button onclick="downloadQR('png')" class="py-3 rounded-xl bg-gray-800 text-white hover:bg-gray-900 transition-all">
                        <i class="fas fa-download mr-2"></i>PNG
                    </button>
                    <button onclick="downloadQR('svg')" class="py-3 rounded-xl bg-gray-200 text-gray-800 hover:bg-gray-300 transition-all">
                        <i class="fas fa-vector-square mr-2"></i>SVG
                    </button>
                </div>
            </div>
        </div>

        <!-- Footer -->
        <footer class="text-center mt-8 text-white opacity-70 text-sm">
            <p>🔒 100% sécurisé • Aucune donnée stockée • Fonctionne hors-ligne</p>
        </footer>
    </div>

    <script>
        let currentTab = 'url';

        function switchTab(tab) {
            currentTab = tab;
            document.querySelectorAll('.tab-btn').forEach(btn => {
                if (btn.dataset.tab === tab) {
                    btn.classList.add('tab-active');
                    btn.classList.remove('text-gray-600', 'hover:bg-gray-100');
                } else {
                    btn.classList.remove('tab-active');
                    btn.classList.add('text-gray-600', 'hover:bg-gray-100');
                }
            });
            document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'));
            document.getElementById(`form-${tab}`).classList.remove('hidden');
        }

        function generateQR() {
            let qrData = '';
            
            switch(currentTab) {
                case 'url':
                    const url = document.getElementById('url-input').value.trim();
                    if (!url) { alert('Veuillez entrer une URL'); return; }
                    qrData = url;
                    break;
                case 'wifi':
                    const ssid = document.getElementById('wifi-ssid').value;
                    const pass = document.getElementById('wifi-password').value;
                    const sec = document.getElementById('wifi-security').value;
                    if (!ssid) { alert('Veuillez entrer le nom du réseau'); return; }
                    qrData = `WIFI:T:${sec};S:${ssid};P:${pass};;`;
                    break;
                case 'contact':
                    const fn = document.getElementById('contact-firstname').value;
                    const ln = document.getElementById('contact-lastname').value;
                    const ph = document.getElementById('contact-phone').value;
                    const em = document.getElementById('contact-email').value;
                    if (!fn && !ln) { alert('Veuillez entrer un nom'); return; }
                    qrData = `BEGIN:VCARD\nVERSION:3.0\nN:${ln};${fn};;;\nFN:${fn} ${ln}\n`;
                    if (ph) qrData += `TEL:${ph}\n`;
                    if (em) qrData += `EMAIL:${em}\n`;
                    qrData += 'END:VCARD';
                    break;
                case 'text':
                    const txt = document.getElementById('text-input').value;
                    if (!txt) { alert('Veuillez entrer du texte'); return; }
                    qrData = txt;
                    break;
            }

            const container = document.getElementById('qrcode');
            container.innerHTML = '';
            
            const color = document.getElementById('qr-color').value;
            const bg = document.getElementById('bg-color').value;

            try {
                new QRCode(container, {
                    text: qrData,
                    width: 256,
                    height: 256,
                    colorDark: color,
                    colorLight: bg,
                    correctLevel: QRCode.CorrectLevel.H
                });
                
                document.getElementById('download-section').classList.remove('hidden');
            } catch(e) {
                alert('Erreur de génération');
            }
        }

        function downloadQR(format) {
            const canvas = document.querySelector('#qrcode canvas');
            if (!canvas) { alert('Générez d\'abord un QR'); return; }
            
            if (format === 'png') {
                const link = document.createElement('a');
                link.download = `qrcode-${Date.now()}.png`;
                link.href = canvas.toDataURL();
                link.click();
            } else {
                const svg = `<svg xmlns="http://www.w3.org/2000/svg" width="256" height="256">
                    <image href="${canvas.toDataURL()}" width="256" height="256"/>
                </svg>`;
                const blob = new Blob([svg], {type: 'image/svg+xml'});
                const link = document.createElement('a');
                link.download = `qrcode-${Date.now()}.svg`;
                link.href = URL.createObjectURL(blob);
                link.click();
            }
        }
    </script>
</body>
</html>
