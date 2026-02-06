<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Pro - Générateur Professionnel</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jsQR/1.4.0/jsQR.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        
        * {
            font-family: 'Inter', sans-serif;
        }
        
        body {
            background: #f8fafc;
            min-height: 100vh;
        }
        
        .step-active {
            color: #10b981;
        }
        
        .step-inactive {
            color: #cbd5e1;
        }
        
        .type-card {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            border: 2px solid transparent;
        }
        
        .type-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 20px 40px -12px rgba(0, 0, 0, 0.15);
            border-color: #10b981;
        }
        
        .type-card.active {
            border-color: #10b981;
            background: #ecfdf5;
        }
        
        .phone-mockup {
            background: #1a1a1a;
            border-radius: 40px;
            padding: 12px;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
        }
        
        .phone-screen {
            background: white;
            border-radius: 32px;
            overflow: hidden;
            position: relative;
        }
        
        .notch {
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 120px;
            height: 30px;
            background: #1a1a1a;
            border-bottom-left-radius: 20px;
            border-bottom-right-radius: 20px;
            z-index: 10;
        }
        
        .qr-preview-area {
            background: linear-gradient(135deg, #f1f5f9 0%, #e2e8f0 100%);
            min-height: 400px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .input-field {
            transition: all 0.3s ease;
            border: 2px solid #e2e8f0;
        }
        
        .input-field:focus {
            border-color: #10b981;
            box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.1);
        }
        
        .btn-primary {
            background: #10b981;
            transition: all 0.3s ease;
        }
        
        .btn-primary:hover {
            background: #059669;
            transform: translateY(-2px);
            box-shadow: 0 10px 20px -5px rgba(16, 185, 129, 0.4);
        }
        
        .color-option {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            border: 3px solid transparent;
            transition: all 0.2s;
        }
        
        .color-option:hover {
            transform: scale(1.1);
        }
        
        .color-option.active {
            border-color: #1a1a1a;
            box-shadow: 0 0 0 2px white, 0 0 0 4px #1a1a1a;
        }
        
        .progress-line {
            height: 2px;
            background: #e2e8f0;
            flex: 1;
            margin: 0 10px;
            position: relative;
        }
        
        .progress-line.active::after {
            content: '';
            position: absolute;
            left: 0;
            top: 0;
            height: 100%;
            width: 100%;
            background: #10b981;
            animation: progressFill 0.5s ease;
        }
        
        @keyframes progressFill {
            from { width: 0; }
            to { width: 100%; }
        }
        
        .template-card {
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .template-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 20px 40px -12px rgba(0, 0, 0, 0.15);
        }
        
        .template-card.active {
            ring: 3px solid #10b981;
        }
        
        .scanner-frame {
            position: relative;
            overflow: hidden;
        }
        
        .scanner-line {
            position: absolute;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, transparent, #10b981, transparent);
            box-shadow: 0 0 20px #10b981;
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
            background: #1a1a1a;
            color: white;
            padding: 16px 28px;
            border-radius: 12px;
            opacity: 0;
            transition: all 0.4s ease;
            z-index: 10000;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        
        .toast.show {
            transform: translateX(-50%) translateY(0);
            opacity: 1;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
            animation: fadeIn 0.4s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .history-item {
            animation: slideIn 0.3s ease;
        }
        
        @keyframes slideIn {
            from { opacity: 0; transform: translateX(-20px); }
            to { opacity: 1; transform: translateX(0); }
        }
    </style>
</head>
<body class="text-gray-800">
    
    <!-- Header -->
    <header class="bg-white border-b border-gray-200 sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <!-- Logo -->
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-emerald-500 rounded-xl flex items-center justify-center">
                        <i class="fas fa-qrcode text-white text-xl"></i>
                    </div>
                    <div>
                        <h1 class="font-bold text-lg leading-tight">QR Code</h1>
                        <p class="text-xs text-gray-500">Generator</p>
                    </div>
                </div>
                
                <!-- Progress Steps -->
                <div class="hidden md:flex items-center gap-4 flex-1 justify-center max-w-2xl">
                    <div class="flex items-center gap-2 step-active" id="step-1-indicator">
                        <div class="w-6 h-6 rounded-full bg-emerald-500 text-white flex items-center justify-center text-xs font-bold">1</div>
                        <span class="text-sm font-medium">Select QR type</span>
                    </div>
                    <div class="progress-line active" id="line-1"></div>
                    <div class="flex items-center gap-2 step-inactive" id="step-2-indicator">
                        <div class="w-6 h-6 rounded-full bg-gray-200 text-gray-500 flex items-center justify-center text-xs font-bold">2</div>
                        <span class="text-sm font-medium">Add content</span>
                    </div>
                    <div class="progress-line" id="line-2"></div>
                    <div class="flex items-center gap-2 step-inactive" id="step-3-indicator">
                        <div class="w-6 h-6 rounded-full bg-gray-200 text-gray-500 flex items-center justify-center text-xs font-bold">3</div>
                        <span class="text-sm font-medium">Design QR code</span>
                    </div>
                    <div class="progress-line" id="line-3"></div>
                    <div class="flex items-center gap-2 step-inactive" id="step-4-indicator">
                        <div class="w-6 h-6 rounded-full bg-gray-200 text-gray-500 flex items-center justify-center text-xs font-bold">4</div>
                        <span class="text-sm font-medium">Download</span>
                    </div>
                </div>
                
                <!-- Right Actions -->
                <div class="flex items-center gap-3">
                    <button onclick="showHistory()" class="p-2 text-gray-500 hover:text-gray-700 transition-colors">
                        <i class="fas fa-history text-xl"></i>
                    </button>
                    <button onclick="toggleTheme()" class="p-2 text-gray-500 hover:text-gray-700 transition-colors">
                        <i class="fas fa-moon text-xl"></i>
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        
        <!-- STEP 1: Select QR Type -->
        <section id="step-1" class="tab-content active">
            <div class="text-center mb-10">
                <h2 class="text-3xl md:text-4xl font-bold text-emerald-500 mb-4">
                    Easily create a QR code<br>for any occasion in seconds!
                </h2>
                <p class="text-gray-500">Choose a QR Type to get started</p>
            </div>
            
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 max-w-5xl mx-auto">
                <!-- Website -->
                <button onclick="selectType('website')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-emerald-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-globe text-2xl text-emerald-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Website</h3>
                    <p class="text-xs text-gray-500">Link to any website URL</p>
                </button>
                
                <!-- vCard -->
                <button onclick="selectType('vcard')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-blue-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-address-card text-2xl text-blue-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">vCard</h3>
                    <p class="text-xs text-gray-500">Share a digital business card</p>
                </button>
                
                <!-- Business -->
                <button onclick="selectType('business')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-purple-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-briefcase text-2xl text-purple-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Business</h3>
                    <p class="text-xs text-gray-500">Share info about your business</p>
                </button>
                
                <!-- PDF -->
                <button onclick="selectType('pdf')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-red-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-file-pdf text-2xl text-red-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">PDF</h3>
                    <p class="text-xs text-gray-500">Share a PDF document</p>
                </button>
                
                <!-- List of Links -->
                <button onclick="selectType('links')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-orange-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-list text-2xl text-orange-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">List of Links</h3>
                    <p class="text-xs text-gray-500">Share multiple links</p>
                </button>
                
                <!-- Video -->
                <button onclick="selectType('video')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-pink-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-play-circle text-2xl text-pink-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Video</h3>
                    <p class="text-xs text-gray-500">Share a video</p>
                </button>
                
                <!-- Images -->
                <button onclick="selectType('images')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-indigo-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-images text-2xl text-indigo-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Images</h3>
                    <p class="text-xs text-gray-500">Share multiple images</p>
                </button>
                
                <!-- Facebook -->
                <button onclick="selectType('facebook')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-blue-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fab fa-facebook-f text-2xl text-blue-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Facebook</h3>
                    <p class="text-xs text-gray-500">Share your Facebook page</p>
                </button>
                
                <!-- Instagram -->
                <button onclick="selectType('instagram')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-pink-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fab fa-instagram text-2xl text-pink-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Instagram</h3>
                    <p class="text-xs text-gray-500">Share your Instagram</p>
                </button>
                
                <!-- Social Media -->
                <button onclick="selectType('social')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-cyan-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-share-alt text-2xl text-cyan-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Social Media</h3>
                    <p class="text-xs text-gray-500">Share your social channels</p>
                </button>
                
                <!-- WhatsApp -->
                <button onclick="selectType('whatsapp')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-green-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fab fa-whatsapp text-2xl text-green-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">WhatsApp</h3>
                    <p class="text-xs text-gray-500">Get WhatsApp messages</p>
                </button>
                
                <!-- WiFi -->
                <button onclick="selectType('wifi')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-teal-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-wifi text-2xl text-teal-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">WiFi</h3>
                    <p class="text-xs text-gray-500">Connect to a Wi-Fi network</p>
                </button>
                
                <!-- Menu -->
                <button onclick="selectType('menu')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-yellow-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-utensils text-2xl text-yellow-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Menu</h3>
                    <p class="text-xs text-gray-500">Create a restaurant menu</p>
                </button>
                
                <!-- Apps -->
                <button onclick="selectType('apps')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-gray-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-mobile-alt text-2xl text-gray-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Apps</h3>
                    <p class="text-xs text-gray-500">Redirect to an app store</p>
                </button>
                
                <!-- Coupon -->
                <button onclick="selectType('coupon')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-rose-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-ticket-alt text-2xl text-rose-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Coupon</h3>
                    <p class="text-xs text-gray-500">Share a coupon</p>
                </button>
                
                <!-- Event -->
                <button onclick="selectType('event')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-violet-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-calendar-alt text-2xl text-violet-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Event</h3>
                    <p class="text-xs text-gray-500">Share event details</p>
                </button>
                
                <!-- Email -->
                <button onclick="selectType('email')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm hover:shadow-lg">
                    <div class="w-14 h-14 bg-sky-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-envelope text-2xl text-sky-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Email</h3>
                    <p class="text-xs text-gray-500">Send an email</p>
                </button>
            </div>
        </section>

        <!-- STEP 2: Add Content -->
        <section id="step-2" class="tab-content">
            <div class="max-w-4xl mx-auto">
                <button onclick="goBack()" class="mb-6 flex items-center gap-2 text-gray-500 hover:text-gray-700 transition-colors">
                    <i class="fas fa-arrow-left"></i>
                    Back to types
                </button>
                
                <h2 id="content-title" class="text-2xl font-bold mb-6">Enter your content</h2>
                
                <!-- Dynamic Form Container -->
                <div id="dynamic-form" class="bg-white rounded-2xl p-8 shadow-sm border border-gray-100">
                    <!-- Forms will be injected here -->
                </div>
                
                <div class="flex justify-end mt-6">
                    <button onclick="goToStep(3)" class="btn-primary px-8 py-3 rounded-xl text-white font-semibold flex items-center gap-2">
                        Continue to Design
                        <i class="fas fa-arrow-right"></i>
                    </button>
                </div>
            </div>
        </section>

        <!-- STEP 3: Design -->
        <section id="step-3" class="tab-content">
            <div class="max-w-6xl mx-auto">
                <button onclick="goBack()" class="mb-6 flex items-center gap-2 text-gray-500 hover:text-gray-700 transition-colors">
                    <i class="fas fa-arrow-left"></i>
                    Back to content
                </button>
                
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    <!-- Design Options -->
                    <div>
                        <h2 class="text-2xl font-bold mb-6">Design your QR code</h2>
                        
                        <!-- Templates -->
                        <div class="mb-8">
                            <h3 class="font-semibold mb-4 text-gray-700">Choose a template</h3>
                            <div class="grid grid-cols-4 gap-3">
                                <button onclick="setTemplate('classic')" class="template-card active p-4 rounded-xl border-2 border-emerald-500 bg-emerald-50" data-template="classic">
                                    <div class="w-full aspect-square bg-white rounded-lg flex items-center justify-center mb-2">
                                        <i class="fas fa-qrcode text-3xl text-gray-800"></i>
                                    </div>
                                    <p class="text-xs font-medium text-center">Classic</p>
                                </button>
                                
                                <button onclick="setTemplate('rounded')" class="template-card p-4 rounded-xl border-2 border-gray-200 hover:border-emerald-300" data-template="rounded">
                                    <div class="w-full aspect-square bg-white rounded-2xl flex items-center justify-center mb-2">
                                        <i class="fas fa-qrcode text-3xl text-gray-800"></i>
                                    </div>
                                    <p class="text-xs font-medium text-center">Rounded</p>
                                </button>
                                
                                <button onclick="setTemplate('dots')" class="template-card p-4 rounded-xl border-2 border-gray-200 hover:border-emerald-300" data-template="dots">
                                    <div class="w-full aspect-square bg-white rounded-lg flex items-center justify-center mb-2">
                                        <div class="grid grid-cols-3 gap-1">
                                            <div class="w-2 h-2 bg-gray-800 rounded-full"></div>
                                            <div class="w-2 h-2 bg-gray-800 rounded-full"></div>
                                            <div class="w-2 h-2 bg-gray-800 rounded-full"></div>
                                            <div class="w-2 h-2 bg-gray-800 rounded-full"></div>
                                            <div class="w-2 h-2 bg-gray-800 rounded-full"></div>
                                            <div class="w-2 h-2 bg-gray-800 rounded-full"></div>
                                        </div>
                                    </div>
                                    <p class="text-xs font-medium text-center">Dots</p>
                                </button>
                                
                                <button onclick="setTemplate('gradient')" class="template-card p-4 rounded-xl border-2 border-gray-200 hover:border-emerald-300" data-template="gradient">
                                    <div class="w-full aspect-square bg-gradient-to-br from-emerald-400 to-blue-500 rounded-lg flex items-center justify-center mb-2">
                                        <i class="fas fa-qrcode text-3xl text-white"></i>
                                    </div>
                                    <p class="text-xs font-medium text-center">Gradient</p>
                                </button>
                            </div>
                        </div>
                        
                        <!-- Colors -->
                        <div class="mb-8">
                            <h3 class="font-semibold mb-4 text-gray-700">QR Color</h3>
                            <div class="flex gap-3 flex-wrap">
                                <button onclick="setColor('#000000')" class="color-option active bg-black" data-color="#000000"></button>
                                <button onclick="setColor('#10b981')" class="color-option bg-emerald-500" data-color="#10b981"></button>
                                <button onclick="setColor('#3b82f6')" class="color-option bg-blue-500" data-color="#3b82f6"></button>
                                <button onclick="setColor('#ef4444')" class="color-option bg-red-500" data-color="#ef4444"></button>
                                <button onclick="setColor('#f59e0b')" class="color-option bg-amber-500" data-color="#f59e0b"></button>
                                <button onclick="setColor('#8b5cf6')" class="color-option bg-violet-500" data-color="#8b5cf6"></button>
                                <button onclick="setColor('#ec4899')" class="color-option bg-pink-500" data-color="#ec4899"></button>
                                <button onclick="setColor('#06b6d4')" class="color-option bg-cyan-500" data-color="#06b6d4"></button>
                            </div>
                        </div>
                        
                        <!-- Logo Upload -->
                        <div class="mb-8">
                            <h3 class="font-semibold mb-4 text-gray-700">Add logo (optional)</h3>
                            <div class="flex items-center gap-4">
                                <label class="flex-1 cursor-pointer">
                                    <div class="border-2 border-dashed border-gray-300 rounded-xl p-6 text-center hover:border-emerald-500 hover:bg-emerald-50 transition-all">
                                        <i class="fas fa-cloud-upload-alt text-3xl text-gray-400 mb-2"></i>
                                        <p class="text-sm text-gray-600">Click to upload logo</p>
                                        <p class="text-xs text-gray-400 mt-1">PNG, JPG up to 2MB</p>
                                        <input type="file" id="design-logo" accept="image/*" class="hidden" onchange="handleDesignLogo(event)">
                                    </div>
                                </label>
                                <div id="design-logo-preview" class="hidden w-20 h-20 rounded-xl bg-gray-100 flex items-center justify-center overflow-hidden">
                                    <img id="design-logo-img" class="w-full h-full object-contain">
                                </div>
                            </div>
                        </div>
                        
                        <!-- Size -->
                        <div class="mb-8">
                            <h3 class="font-semibold mb-4 text-gray-700">Size</h3>
                            <input type="range" id="qr-size" min="200" max="1000" value="300" class="w-full accent-emerald-500" oninput="updateDesignPreview()">
                            <div class="flex justify-between text-sm text-gray-500 mt-1">
                                <span>Small</span>
                                <span id="size-display">300px</span>
                                <span>Large</span>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Preview Phone -->
                    <div class="flex justify-center items-start">
                        <div class="phone-mockup">
                            <div class="phone-screen w-80 h-[600px]">
                                <div class="notch"></div>
                                <div class="qr-preview-area h-full flex flex-col items-center justify-center p-6">
                                    <div id="phone-qr-container" class="mb-6">
                                        <div class="w-48 h-48 bg-white rounded-2xl shadow-xl flex items-center justify-center">
                                            <i class="fas fa-qrcode text-6xl text-gray-300"></i>
                                        </div>
                                    </div>
                                    <p class="text-gray-500 text-center text-sm">Scan this QR code<br>with your phone</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="flex justify-end mt-8">
                    <button onclick="goToStep(4)" class="btn-primary px-8 py-3 rounded-xl text-white font-semibold flex items-center gap-2">
                        Continue to Download
                        <i class="fas fa-arrow-right"></i>
                    </button>
                </div>
            </div>
        </section>

        <!-- STEP 4: Download -->
        <section id="step-4" class="tab-content">
            <div class="max-w-4xl mx-auto text-center">
                <button onclick="goBack()" class="mb-6 flex items-center gap-2 text-gray-500 hover:text-gray-700 transition-colors">
                    <i class="fas fa-arrow-left"></i>
                    Back to design
                </button>
                
                <div class="bg-white rounded-3xl p-12 shadow-lg border border-gray-100">
                    <div class="w-20 h-20 bg-emerald-100 rounded-full flex items-center justify-center mx-auto mb-6">
                        <i class="fas fa-check text-4xl text-emerald-600"></i>
                    </div>
                    
                    <h2 class="text-3xl font-bold mb-4">Your QR code is ready!</h2>
                    <p class="text-gray-500 mb-8">Download your QR code in your preferred format</p>
                    
                    <div class="flex justify-center mb-8">
                        <div id="final-qr" class="p-4 bg-white rounded-2xl shadow-xl"></div>
                    </div>
                    
                    <div class="grid grid-cols-2 md:grid-cols-4 gap-4 max-w-2xl mx-auto">
                        <button onclick="downloadFinal('png')" class="p-4 rounded-xl bg-gray-100 hover:bg-gray-200 transition-all">
                            <i class="fas fa-download text-2xl text-gray-700 mb-2"></i>
                            <p class="font-semibold text-sm">PNG</p>
                        </button>
                        
                        <button onclick="downloadFinal('svg')" class="p-4 rounded-xl bg-gray-100 hover:bg-gray-200 transition-all">
                            <i class="fas fa-vector-square text-2xl text-gray-700 mb-2"></i>
                            <p class="font-semibold text-sm">SVG</p>
                        </button>
                        
                        <button onclick="downloadFinal('pdf')" class="p-4 rounded-xl bg-gray-100 hover:bg-gray-200 transition-all">
                            <i class="fas fa-file-pdf text-2xl text-gray-700 mb-2"></i>
                            <p class="font-semibold text-sm">PDF</p>
                        </button>
                        
                        <button onclick="printFinal()" class="p-4 rounded-xl bg-gray-100 hover:bg-gray-200 transition-all">
                            <i class="fas fa-print text-2xl text-gray-700 mb-2"></i>
                            <p class="font-semibold text-sm">Print</p>
                        </button>
                    </div>
                    
                    <div class="mt-8 pt-8 border-t border-gray-200">
                        <button onclick="shareFinal()" class="btn-primary px-8 py-3 rounded-xl text-white font-semibold inline-flex items-center gap-2">
                            <i class="fas fa-share-alt"></i>
                            Share QR Code
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- Scanner Modal -->
        <div id="scanner-modal" class="fixed inset-0 bg-black/80 z-50 hidden flex items-center justify-center p-4">
            <div class="bg-white rounded-3xl p-6 max-w-lg w-full">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="font-bold text-lg">Scan QR Code</h3>
                    <button onclick="closeScanner()" class="w-10 h-10 rounded-full bg-gray-100 flex items-center justify-center hover:bg-gray-200">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <div class="relative rounded-2xl overflow-hidden bg-black aspect-video mb-4 scanner-frame">
                    <video id="scan-video" class="w-full h-full object-cover" autoplay playsinline></video>
                    <div class="absolute inset-0 flex items-center justify-center">
                        <div class="w-48 h-48 border-2 border-emerald-400 rounded-2xl relative">
                            <div class="scanner-line"></div>
                        </div>
                    </div>
                </div>
                
                <div class="flex gap-3">
                    <button onclick="startCamera()" class="flex-1 btn-primary py-3 rounded-xl text-white font-semibold">
                        <i class="fas fa-camera mr-2"></i> Start Camera
                    </button>
                    <label class="flex-1 py-3 rounded-xl bg-gray-100 text-gray-700 font-semibold text-center cursor-pointer hover:bg-gray-200 transition-all">
                        <i class="fas fa-image mr-2"></i> Upload
                        <input type="file" accept="image/*" class="hidden" onchange="scanFromFile(event)">
                    </label>
                </div>
            </div>
        </div>

        <!-- History Modal -->
        <div id="history-modal" class="fixed inset-0 bg-black/50 z-50 hidden flex items-center justify-center p-4">
            <div class="bg-white rounded-3xl p-6 max-w-2xl w-full max-h-[80vh] overflow-hidden flex flex-col">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="font-bold text-xl">History</h3>
                    <button onclick="closeHistory()" class="w-10 h-10 rounded-full bg-gray-100 flex items-center justify-center hover:bg-gray-200">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <div id="history-list" class="flex-1 overflow-y-auto space-y-3">
                    <!-- History items -->
                </div>
            </div>
        </div>

    </main>

    <!-- Toast -->
    <div id="toast" class="toast">
        <i class="fas fa-check-circle text-emerald-400 text-xl"></i>
        <span id="toast-message">Success</span>
    </div>

    <script>
        // State
        let currentStep = 1;
        let selectedType = '';
        let qrData = '';
        let qrColor = '#000000';
        let qrTemplate = 'classic';
        let qrSize = 300;
        let qrLogo = null;
        let scanStream = null;
        let history = JSON.parse(localStorage.getItem('qrHistory') || '[]');

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            updateStepIndicators();
        });

        // Step Management
        function updateStepIndicators() {
            for (let i = 1; i <= 4; i++) {
                const indicator = document.getElementById(`step-${i}-indicator`);
                const line = document.getElementById(`line-${i}`);
                
                if (i <= currentStep) {
                    indicator.classList.remove('step-inactive');
                    indicator.classList.add('step-active');
                    indicator.querySelector('div').classList.remove('bg-gray-200', 'text-gray-500');
                    indicator.querySelector('div').classList.add('bg-emerald-500', 'text-white');
                } else {
                    indicator.classList.add('step-inactive');
                    indicator.classList.remove('step-active');
                    indicator.querySelector('div').classList.add('bg-gray-200', 'text-gray-500');
                    indicator.querySelector('div').classList.remove('bg-emerald-500', 'text-white');
                }
                
                if (line) {
                    if (i < currentStep) {
                        line.classList.add('active');
                    } else {
                        line.classList.remove('active');
                    }
                }
            }
        }

        function goToStep(step) {
            // Validation
            if (step === 3 && !validateStep2()) return;
            if (step === 4) generateFinalQR();
            
            // Hide all
            document.querySelectorAll('.tab-content').forEach(el => {
                el.classList.remove('active');
            });
            
            // Show target
            document.getElementById(`step-${step}`).classList.add('active');
            currentStep = step;
            updateStepIndicators();
            
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function goBack() {
            if (currentStep > 1) {
                goToStep(currentStep - 1);
            }
        }

        // Type Selection
        function selectType(type) {
            selectedType = type;
            
            // Visual feedback
            document.querySelectorAll('.type-card').forEach(card => {
                card.classList.remove('active');
            });
            event.currentTarget.classList.add('active');
            
            // Build form
            buildDynamicForm(type);
            
            // Go to step 2
            setTimeout(() => goToStep(2), 300);
        }

        // Dynamic Form Builder
        function buildDynamicForm(type) {
            const container = document.getElementById('dynamic-form');
            const title = document.getElementById('content-title');
            
            const forms = {
                website: {
                    title: 'Enter website URL',
                    html: `
                        <div class="space-y-4">
                            <label class="block text-sm font-medium text-gray-700">Website URL</label>
                            <input type="url" id="input-url" placeholder="https://www.example.com" class="input-field w-full px-4 py-4 rounded-xl text-lg">
                            <p class="text-sm text-gray-500">Enter the full URL including https://</p>
                        </div>
                    `
                },
                vcard: {
                    title: 'Enter contact information',
                    html: `
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">First Name</label>
                                <input type="text" id="vcard-firstname" placeholder="John" class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Last Name</label>
                                <input type="text" id="vcard-lastname" placeholder="Doe" class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Phone</label>
                            <input type="tel" id="vcard-phone" placeholder="+1 234 567 890" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Email</label>
                            <input type="email" id="vcard-email" placeholder="john@example.com" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Company</label>
                            <input type="text" id="vcard-company" placeholder="Company Name" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                    `
                },
                business: {
                    title: 'Enter business information',
                    html: `
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Business Name</label>
                            <input type="text" id="biz-name" placeholder="Your Business" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Description</label>
                            <textarea id="biz-desc" rows="3" placeholder="What does your business do?" class="input-field w-full px-4 py-3 rounded-xl"></textarea>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Website</label>
                            <input type="url" id="biz-url" placeholder="https://yourbusiness.com" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                    `
                },
                wifi: {
                    title: 'Enter Wi-Fi credentials',
                    html: `
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Network Name (SSID)</label>
                            <input type="text" id="wifi-ssid" placeholder="MyWiFiNetwork" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Password</label>
                            <input type="text" id="wifi-pass" placeholder="WiFiPassword123" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Security</label>
                            <select id="wifi-sec" class="input-field w-full px-4 py-3 rounded-xl">
                                <option value="WPA">WPA/WPA2</option>
                                <option value="WEP">WEP</option>
                                <option value="nopass">No Password</option>
                            </select>
                        </div>
                    `
                },
                whatsapp: {
                    title: 'Enter WhatsApp number',
                    html: `
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Phone Number</label>
                            <input type="tel" id="wa-number" placeholder="+1 234 567 890" class="input-field w-full px-4 py-4 rounded-xl text-lg">
                            <p class="text-sm text-gray-500 mt-2">Include country code (e.g., +1 for USA)</p>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Prefilled Message (optional)</label>
                            <textarea id="wa-message" rows="3" placeholder="Hello, I'm interested in..." class="input-field w-full px-4 py-3 rounded-xl"></textarea>
                        </div>
                    `
                },
                email: {
                    title: 'Enter email details',
                    html: `
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">To</label>
                            <input type="email" id="email-to" placeholder="recipient@example.com" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Subject</label>
                            <input type="text" id="email-subject" placeholder="Email Subject" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Body</label>
                            <textarea id="email-body" rows="4" placeholder="Email message..." class="input-field w-full px-4 py-3 rounded-xl"></textarea>
                        </div>
                    `
                },
                facebook: {
                    title: 'Enter Facebook page URL',
                    html: `
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Facebook Page URL</label>
                            <input type="url" id="fb-url" placeholder="https://facebook.com/yourpage" class="input-field w-full px-4 py-4 rounded-xl text-lg">
                        </div>
                    `
                },
                instagram: {
                    title: 'Enter Instagram username',
                    html: `
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Instagram Username</label>
                            <div class="relative">
                                <span class="absolute left-4 top-1/2 -translate-y-1/2 text-gray-400 text-lg">@</span>
                                <input type="text" id="ig-user" placeholder="username" class="input-field w-full pl-12 pr-4 py-4 rounded-xl text-lg">
                            </div>
                        </div>
                    `
                },
                social: {
                    title: 'Enter social media links',
                    html: `
                        <div class="space-y-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2"><i class="fab fa-instagram text-pink-500 mr-2"></i>Instagram</label>
                                <input type="text" id="social-ig" placeholder="@username" class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2"><i class="fab fa-twitter text-blue-400 mr-2"></i>Twitter</label>
                                <input type="text" id="social-tw" placeholder="@username" class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2"><i class="fab fa-linkedin text-blue-700 mr-2"></i>LinkedIn</label>
                                <input type="text" id="social-li" placeholder="username" class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2"><i class="fab fa-tiktok text-black mr-2"></i>TikTok</label>
                                <input type="text" id="social-tt" placeholder="@username" class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                        </div>
                    `
                },
                event: {
                    title: 'Enter event details',
                    html: `
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Event Name</label>
                            <input type="text" id="event-name" placeholder="My Awesome Event" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Start</label>
                                <input type="datetime-local" id="event-start" class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">End</label>
                                <input type="datetime-local" id="event-end" class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Location</label>
                            <input type="text" id="event-loc" placeholder="Event Location" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                    `
                },
                pdf: {
                    title: 'Upload PDF file',
                    html: `
                        <div class="border-2 border-dashed border-gray-300 rounded-2xl p-12 text-center hover:border-emerald-500 hover:bg-emerald-50 transition-all cursor-pointer">
                            <i class="fas fa-file-pdf text-6xl text-red-500 mb-4"></i>
                            <p class="text-lg font-medium text-gray-700 mb-2">Click to upload PDF</p>
                            <p class="text-sm text-gray-500">or drag and drop</p>
                            <input type="file" accept=".pdf" class="hidden" onchange="handlePDFUpload(event)">
                        </div>
                    `
                },
                menu: {
                    title: 'Enter menu URL or upload',
                    html: `
                        <div class="space-y-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Restaurant Name</label>
                                <input type="text" id="menu-name" placeholder="Your Restaurant" class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Menu URL</label>
                                <input type="url" id="menu-url" placeholder="https://yourrestaurant.com/menu" class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                            <p class="text-center text-gray-500">or</p>
                            <div class="border-2 border-dashed border-gray-300 rounded-2xl p-8 text-center hover:border-emerald-500 hover:bg-emerald-50 transition-all cursor-pointer">
                                <i class="fas fa-upload text-3xl text-gray-400 mb-2"></i>
                                <p class="text-sm text-gray-600">Upload Menu PDF</p>
                                <input type="file" accept=".pdf" class="hidden">
                            </div>
                        </div>
                    `
                },
                apps: {
                    title: 'Enter app store links',
                    html: `
                        <div class="space-y-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2"><i class="fab fa-apple text-gray-800 mr-2"></i>App Store</label>
                                <input type="url" id="app-ios" placeholder="https://apps.apple.com/..." class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2"><i class="fab fa-google-play text-green-600 mr-2"></i>Google Play</label>
                                <input type="url" id="app-android" placeholder="https://play.google.com/..." class="input-field w-full px-4 py-3 rounded-xl">
                            </div>
                        </div>
                    `
                },
                coupon: {
                    title: 'Enter coupon details',
                    html: `
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Coupon Code</label>
                            <input type="text" id="coupon-code" placeholder="SAVE20" class="input-field w-full px-4 py-3 rounded-xl text-2xl font-mono text-center tracking-widest">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Description</label>
                            <input type="text" id="coupon-desc" placeholder="20% off your next purchase" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Valid Until</label>
                            <input type="date" id="coupon-expiry" class="input-field w-full px-4 py-3 rounded-xl">
                        </div>
                    `
                },
                video: {
                    title: 'Enter video URL',
                    html: `
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Video URL</label>
                            <input type="url" id="video-url" placeholder="https://youtube.com/watch?v=..." class="input-field w-full px-4 py-4 rounded-xl text-lg">
                            <p class="text-sm text-gray-500 mt-2">Supports YouTube, Vimeo, and other platforms</p>
                        </div>
                    `
                },
                images: {
                    title: 'Upload images',
                    html: `
                        <div class="border-2 border-dashed border-gray-300 rounded-2xl p-12 text-center hover:border-emerald-500 hover:bg-emerald-50 transition-all cursor-pointer">
                            <i class="fas fa-images text-6xl text-indigo-500 mb-4"></i>
                            <p class="text-lg font-medium text-gray-700 mb-2">Click to upload images</p>
                            <p class="text-sm text-gray-500">Select multiple images</p>
                            <input type="file" accept="image/*" multiple class="hidden">
                        </div>
                    `
                },
                links: {
                    title: 'Enter multiple links',
                    html: `
                        <div id="links-container" class="space-y-3">
                            <div class="flex gap-2">
                                <input type="url" placeholder="https://example.com" class="input-field flex-1 px-4 py-3 rounded-xl link-input">
                            </div>
                        </div>
                        <button onclick="addLinkField()" class="mt-4 flex items-center gap-2 text-emerald-600 font-medium hover:text-emerald-700">
                            <i class="fas fa-plus-circle"></i>
                            Add another link
                        </button>
                    `
                }
            };
            
            const form = forms[type] || forms.website;
            title.textContent = form.title;
            container.innerHTML = form.html;
        }

        function addLinkField() {
            const container = document.getElementById('links-container');
            const div = document.createElement('div');
            div.className = 'flex gap-2';
            div.innerHTML = `
                <input type="url" placeholder="https://example.com" class="input-field flex-1 px-4 py-3 rounded-xl link-input">
                <button onclick="this.parentElement.remove()" class="px-3 text-red-500 hover:text-red-700">
                    <i class="fas fa-times"></i>
                </button>
            `;
            container.appendChild(div);
        }

        // Validation
        function validateStep2() {
            // Collect data based on type
            switch(selectedType) {
                case 'website':
                    qrData = document.getElementById('input-url')?.value;
                    break;
                case 'vcard':
                    const fn = document.getElementById('vcard-firstname')?.value;
                    const ln = document.getElementById('vcard-lastname')?.value;
                    const phone = document.getElementById('vcard-phone')?.value;
                    const email = document.getElementById('vcard-email')?.value;
                    const company = document.getElementById('vcard-company')?.value;
                    qrData = `BEGIN:VCARD\nVERSION:3.0\nFN:${fn} ${ln}\nTEL:${phone}\nEMAIL:${email}\nORG:${company}\nEND:VCARD`;
                    break;
                case 'wifi':
                    const ssid = document.getElementById('wifi-ssid')?.value;
                    const pass = document.getElementById('wifi-pass')?.value;
                    const sec = document.getElementById('wifi-sec')?.value;
                    qrData = `WIFI:T:${sec};S:${ssid};P:${pass};;`;
                    break;
                case 'whatsapp':
                    const num = document.getElementById('wa-number')?.value.replace(/[^0-9]/g, '');
                    const msg = document.getElementById('wa-message')?.value;
                    qrData = `https://wa.me/${num}${msg ? '?text=' + encodeURIComponent(msg) : ''}`;
                    break;
                case 'email':
                    const to = document.getElementById('email-to')?.value;
                    const subject = document.getElementById('email-subject')?.value;
                    const body = document.getElementById('email-body')?.value;
                    qrData = `mailto:${to}?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
                    break;
                case 'facebook':
                    qrData = document.getElementById('fb-url')?.value;
                    break;
                case 'instagram':
                    const user = document.getElementById('ig-user')?.value;
                    qrData = `https://instagram.com/${user}`;
                    break;
                case 'social':
                    const ig = document.getElementById('social-ig')?.value;
                    const tw = document.getElementById('social-tw')?.value;
                    const li = document.getElementById('social-li')?.value;
                    const tt = document.getElementById('social-tt')?.value;
                    qrData = `https://linktr.ee/${ig || 'user'}`; // Simplified
                    break;
                case 'event':
                    const ename = document.getElementById('event-name')?.value;
                    const start = document.getElementById('event-start')?.value;
                    const end = document.getElementById('event-end')?.value;
                    const loc = document.getElementById('event-loc')?.value;
                    qrData = `BEGIN:VEVENT\nSUMMARY:${ename}\nDTSTART:${start}\nDTEND:${end}\nLOCATION:${loc}\nEND:VEVENT`;
                    break;
                case 'coupon':
                    const code = document.getElementById('coupon-code')?.value;
                    const desc = document.getElementById('coupon-desc')?.value;
                    qrData = `COUPON:${code}:${desc}`;
                    break;
                case 'video':
                    qrData = document.getElementById('video-url')?.value;
                    break;
                default:
                    qrData = 'https://example.com';
            }
            
            if (!qrData || qrData.includes('undefined')) {
                showToast('Please fill in the required fields', 'error');
                return false;
            }
            
            return true;
        }

        // Design Functions
        function setTemplate(template) {
            qrTemplate = template;
            document.querySelectorAll('.template-card').forEach(t => {
                t.classList.remove('active', 'border-emerald-500', 'bg-emerald-50');
                t.classList.add('border-gray-200');
            });
            event.currentTarget.classList.add('active', 'border-emerald-500', 'bg-emerald-50');
            event.currentTarget.classList.remove('border-gray-200');
            updateDesignPreview();
        }

        function setColor(color) {
            qrColor = color;
            document.querySelectorAll('.color-option').forEach(c => {
                c.classList.remove('active');
            });
            event.currentTarget.classList.add('active');
            updateDesignPreview();
        }

        function handleDesignLogo(event) {
            const file = event.target.files[0];
            if (!file) return;
            
            const reader = new FileReader();
            reader.onload = (e) => {
                qrLogo = e.target.result;
                document.getElementById('design-logo-preview').classList.remove('hidden');
                document.getElementById('design-logo-img').src = qrLogo;
                updateDesignPreview();
            };
            reader.readAsDataURL(file);
        }

        function updateDesignPreview() {
            const size = document.getElementById('qr-size').value;
            document.getElementById('size-display').textContent = size + 'px';
            qrSize = parseInt(size);
            
            // Generate preview QR
            const container = document.getElementById('phone-qr-container');
            container.innerHTML = '';
            
            const div = document.createElement('div');
            div.className = 'w-48 h-48 bg-white rounded-2xl shadow-xl flex items-center justify-center p-4';
            
            new QRCode(div, {
                text: qrData || 'https://example.com',
                width: 160,
                height: 160,
                colorDark: qrColor,
                colorLight: '#ffffff',
                correctLevel: QRCode.CorrectLevel.H
            });
            
            container.appendChild(div);
        }

        // Final Generation
        function generateFinalQR() {
            const container = document.getElementById('final-qr');
            container.innerHTML = '';
            
            new QRCode(container, {
                text: qrData,
                width: 250,
                height: 250,
                colorDark: qrColor,
                colorLight: '#ffffff',
                correctLevel: QRCode.CorrectLevel.H
            });
            
            // Save to history
            const item = {
                id: Date.now(),
                type: selectedType,
                data: qrData,
                color: qrColor,
                timestamp: new Date().toISOString()
            };
            history.unshift(item);
            if (history.length > 50) history.pop();
            localStorage.setItem('qrHistory', JSON.stringify(history));
        }

        // Downloads
        function downloadFinal(format) {
            const canvas = document.querySelector('#final-qr canvas');
            if (!canvas) return;
            
            if (format === 'png') {
                const link = document.createElement('a');
                link.download = `qrcode-${Date.now()}.png`;
                link.href = canvas.toDataURL('image/png');
                link.click();
            } else if (format === 'svg') {
                const svg = `<svg xmlns="http://www.w3.org/2000/svg" width="250" height="250">
                    <image href="${canvas.toDataURL()}" width="250" height="250"/>
                </svg>`;
                const blob = new Blob([svg], {type: 'image/svg+xml'});
                const url = URL.createObjectURL(blob);
                const link = document.createElement('a');
                link.download = `qrcode-${Date.now()}.svg`;
                link.href = url;
                link.click();
            } else if (format === 'pdf') {
                const { jsPDF } = window.jspdf;
                const pdf = new jsPDF();
                const imgData = canvas.toDataURL('image/png');
                pdf.addImage(imgData, 'PNG', 60, 60, 90, 90);
                pdf.save(`qrcode-${Date.now()}.pdf`);
            }
            
            showToast(`Downloaded as ${format.toUpperCase()}`);
        }

        function printFinal() {
            const canvas = document.querySelector('#final-qr canvas');
            if (!canvas) return;
            
            const win = window.open('', '_blank');
            win.document.write(`
                <html>
                <head><title>Print QR Code</title></head>
                <body style="display:flex;justify-content:center;align-items:center;height:100vh;margin:0;">
                    <img src="${canvas.toDataURL()}" style="max-width:80%;">
                    <script>window.onload = () => setTimeout(() => window.print(), 100);<\/script>
                </body>
                </html>
            `);
        }

        function shareFinal() {
            if (navigator.share) {
                navigator.share({
                    title: 'My QR Code',
                    text: 'Created with QR Code Generator',
                    url: window.location.href
                });
            } else {
                showToast('Sharing not supported on this device');
            }
        }

        // Scanner Modal
        function showScanner() {
            document.getElementById('scanner-modal').classList.remove('hidden');
        }

        function closeScanner() {
            stopCamera();
            document.getElementById('scanner-modal').classList.add('hidden');
        }

        async function startCamera() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'environment' } });
                const video = document.getElementById('scan-video');
                video.srcObject = stream;
                scanStream = stream;
                scanLoop();
            } catch (err) {
                showToast('Camera access denied', 'error');
            }
        }

        function stopCamera() {
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
                showToast('QR Code detected: ' + code.data.substring(0, 50) + '...');
                // Handle scanned data
                stopCamera();
                closeScanner();
                return;
            }
            
            requestAnimationFrame(scanLoop);
        }

        function scanFromFile(event) {
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
                    showToast('QR Code detected!');
                } else {
                    showToast('No QR code found', 'error');
                }
            };
            img.src = URL.createObjectURL(file);
        }

        // History
        function showHistory() {
            const modal = document.getElementById('history-modal');
            const list = document.getElementById('history-list');
            
            if (history.length === 0) {
                list.innerHTML = '<p class="text-center text-gray-500 py-8">No history yet</p>';
            } else {
                list.innerHTML = history.map(item => `
                    <div class="history-item flex items-center justify-between p-4 bg-gray-50 rounded-xl">
                        <div class="flex items-center gap-3">
                            <div class="w-12 h-12 bg-emerald-100 rounded-lg flex items-center justify-center">
                                <i class="fas fa-qrcode text-emerald-600"></i>
                            </div>
                            <div>
                                <p class="font-medium capitalize">${item.type}</p>
                                <p class="text-xs text-gray-500">${new Date(item.timestamp).toLocaleDateString()}</p>
                            </div>
                        </div>
                        <button onclick="restoreHistoryItem(${item.id})" class="px-4 py-2 bg-emerald-500 text-white rounded-lg text-sm hover:bg-emerald-600">
                            Restore
                        </button>
                    </div>
                `).join('');
            }
            
            modal.classList.remove('hidden');
        }

        function closeHistory() {
            document.getElementById('history-modal').classList.add('hidden');
        }

        function restoreHistoryItem(id) {
            const item = history.find(h => h.id === id);
            if (item) {
                qrData = item.data;
                selectedType = item.type;
                qrColor = item.color;
                goToStep(4);
                closeHistory();
            }
        }

        // Theme
        function toggleTheme() {
            document.body.classList.toggle('dark');
            const icon = document.querySelector('header .fa-moon');
            if (document.body.classList.contains('dark')) {
                icon.classList.remove('fa-moon');
                icon.classList.add('fa-sun');
            } else {
                icon.classList.remove('fa-sun');
                icon.classList.add('fa-moon');
            }
        }

        // Toast
        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            const msg = document.getElementById('toast-message');
            const icon = toast.querySelector('i');
            
            msg.textContent = message;
            icon.className = type === 'error' ? 'fas fa-exclamation-circle text-red-400 text-xl' : 'fas fa-check-circle text-emerald-400 text-xl';
            
            toast.classList.add('show');
            setTimeout(() => toast.classList.remove('show'), 3000);
        }

        // Handle PDF (placeholder)
        function handlePDFUpload(event) {
            showToast('PDF uploaded successfully');
        }
    </script>
</body>
</html>
