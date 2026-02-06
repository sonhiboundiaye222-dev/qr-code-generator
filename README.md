<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Pro - Carte de Visite Digitale</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Poppins:wght@400;500;600;700&display=swap');
        
        * { font-family: 'Inter', sans-serif; }
        
        body { background: #f8fafc; min-height: 100vh; }
        
        .step-active { color: #10b981; }
        .step-inactive { color: #cbd5e1; }
        
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
        
        /* iPhone Mockup */
        .phone-mockup {
            background: #1a1a1a;
            border-radius: 40px;
            padding: 12px;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
            position: sticky;
            top: 100px;
        }
        .phone-screen {
            background: white;
            border-radius: 32px;
            overflow: hidden;
            position: relative;
            height: 700px;
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
            z-index: 50;
        }
        
        /* Digital Card Preview */
        .digital-card {
            height: 100%;
            overflow-y: auto;
            scrollbar-width: none;
        }
        .digital-card::-webkit-scrollbar { display: none; }
        
        .card-header {
            background: linear-gradient(135deg, #064e3b 0%, #065f46 50%, #047857 100%);
            position: relative;
            overflow: hidden;
        }
        
        .card-header::before {
            content: '';
            position: absolute;
            top: -50%;
            right: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, transparent 70%);
            animation: pulse 4s ease-in-out infinite;
        }
        
        @keyframes pulse {
            0%, 100% { transform: scale(1); opacity: 0.5; }
            50% { transform: scale(1.1); opacity: 0.8; }
        }
        
        .wave-bottom {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            height: 60px;
            background: white;
            clip-path: ellipse(60% 100% at 50% 100%);
        }
        
        .profile-img {
            border: 4px solid white;
            box-shadow: 0 10px 30px -5px rgba(0,0,0,0.3);
            animation: float 3s ease-in-out infinite;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }
        
        .action-btn {
            transition: all 0.3s ease;
        }
        .action-btn:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 0 10px 20px -5px rgba(0,0,0,0.2);
        }
        
        .social-icon {
            transition: all 0.3s ease;
        }
        .social-icon:hover {
            transform: translateY(-5px) scale(1.2) rotate(10deg);
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
        .color-option:hover { transform: scale(1.1); }
        .color-option.active {
            border-color: #1a1a1a;
            box-shadow: 0 0 0 2px white, 0 0 0 4px #1a1a1a;
        }
        
        .accordion-item {
            border-bottom: 1px solid #e2e8f0;
        }
        .accordion-header {
            cursor: pointer;
            transition: all 0.3s;
        }
        .accordion-header:hover {
            background: #f8fafc;
        }
        .accordion-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease;
        }
        .accordion-content.open {
            max-height: 1000px;
        }
        
        .social-grid {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 8px;
        }
        .social-select {
            aspect-ratio: 1;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.2s;
            border: 2px solid transparent;
        }
        .social-select:hover { transform: scale(1.1); }
        .social-select.active {
            border-color: #10b981;
            background: #ecfdf5;
        }
        
        /* Landing Page Styles */
        .landing-page {
            display: none;
            position: fixed;
            inset: 0;
            background: white;
            z-index: 100;
            overflow-y: auto;
        }
        .landing-page.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }
        
        .landing-hero {
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
        }
        
        .landing-bg {
            position: absolute;
            inset: 0;
            background: linear-gradient(135deg, #064e3b 0%, #065f46 50%, #047857 100%);
        }
        
        .landing-bg::before {
            content: '';
            position: absolute;
            inset: 0;
            background: url('data:image/svg+xml,<svg width="100" height="100" xmlns="http://www.w3.org/2000/svg"><circle cx="50" cy="50" r="40" fill="none" stroke="rgba(255,255,255,0.1)" stroke-width="2"/></svg>');
            background-size: 100px 100px;
            animation: bgMove 20s linear infinite;
        }
        
        @keyframes bgMove {
            0% { transform: translate(0, 0); }
            100% { transform: translate(100px, 100px); }
        }
        
        .landing-content {
            position: relative;
            z-index: 10;
            text-align: center;
            padding: 2rem;
        }
        
        .landing-profile {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            border: 5px solid white;
            box-shadow: 0 20px 60px -10px rgba(0,0,0,0.5);
            margin-bottom: 2rem;
            animation: landingFloat 3s ease-in-out infinite;
        }
        
        @keyframes landingFloat {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }
        
        .landing-name {
            font-size: 3rem;
            font-weight: 800;
            color: white;
            margin-bottom: 0.5rem;
            animation: slideUp 0.8s ease;
        }
        
        .landing-title {
            font-size: 1.5rem;
            color: rgba(255,255,255,0.9);
            margin-bottom: 2rem;
            animation: slideUp 0.8s ease 0.2s both;
        }
        
        @keyframes slideUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        .landing-actions {
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
            justify-content: center;
            animation: slideUp 0.8s ease 0.4s both;
        }
        
        .landing-btn {
            padding: 1rem 2rem;
            border-radius: 50px;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .landing-btn:hover {
            transform: translateY(-5px) scale(1.05);
            box-shadow: 0 15px 30px -5px rgba(0,0,0,0.3);
        }
        
        .landing-section {
            padding: 4rem 2rem;
            max-width: 800px;
            margin: 0 auto;
        }
        
        .info-card {
            background: white;
            border-radius: 20px;
            padding: 2rem;
            margin-bottom: 1.5rem;
            box-shadow: 0 10px 40px -10px rgba(0,0,0,0.1);
            transform: translateY(50px);
            opacity: 0;
            transition: all 0.6s ease;
        }
        
        .info-card.visible {
            transform: translateY(0);
            opacity: 1;
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
        }
        .toast.show {
            transform: translateX(-50%) translateY(0);
            opacity: 1;
        }
        
        .tab-content { display: none; }
        .tab-content.active {
            display: block;
            animation: fadeIn 0.4s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="text-gray-800">
    
    <!-- Header -->
    <header class="bg-white border-b border-gray-200 sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-emerald-500 rounded-xl flex items-center justify-center">
                        <i class="fas fa-qrcode text-white text-xl"></i>
                    </div>
                    <div>
                        <h1 class="font-bold text-lg leading-tight">QR Code</h1>
                        <p class="text-xs text-gray-500">Digital Business Card</p>
                    </div>
                </div>
                
                <div class="hidden md:flex items-center gap-4 flex-1 justify-center max-w-2xl">
                    <div class="flex items-center gap-2 step-active" id="step-1-indicator">
                        <div class="w-6 h-6 rounded-full bg-emerald-500 text-white flex items-center justify-center text-xs font-bold">1</div>
                        <span class="text-sm font-medium">Select</span>
                    </div>
                    <div class="h-0.5 w-16 bg-emerald-500"></div>
                    <div class="flex items-center gap-2 step-inactive" id="step-2-indicator">
                        <div class="w-6 h-6 rounded-full bg-gray-200 text-gray-500 flex items-center justify-center text-xs font-bold">2</div>
                        <span class="text-sm font-medium">Information</span>
                    </div>
                    <div class="h-0.5 w-16 bg-gray-200" id="line-2"></div>
                    <div class="flex items-center gap-2 step-inactive" id="step-3-indicator">
                        <div class="w-6 h-6 rounded-full bg-gray-200 text-gray-500 flex items-center justify-center text-xs font-bold">3</div>
                        <span class="text-sm font-medium">Design</span>
                    </div>
                    <div class="h-0.5 w-16 bg-gray-200" id="line-3"></div>
                    <div class="flex items-center gap-2 step-inactive" id="step-4-indicator">
                        <div class="w-6 h-6 rounded-full bg-gray-200 text-gray-500 flex items-center justify-center text-xs font-bold">4</div>
                        <span class="text-sm font-medium">Download</span>
                    </div>
                </div>
                
                <div class="flex items-center gap-3">
                    <button onclick="previewLandingPage()" class="px-4 py-2 bg-emerald-100 text-emerald-700 rounded-lg text-sm font-medium hover:bg-emerald-200 transition-all">
                        <i class="fas fa-eye mr-2"></i>Preview Site
                    </button>
                    <button onclick="toggleTheme()" class="p-2 text-gray-500 hover:text-gray-700">
                        <i class="fas fa-moon text-xl"></i>
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        
        <!-- STEP 1: Select Type -->
        <section id="step-1" class="tab-content active">
            <div class="text-center mb-10">
                <h2 class="text-3xl md:text-4xl font-bold text-emerald-600 mb-4">
                    Create your digital business card
                </h2>
                <p class="text-gray-500">Choose a card type to get started</p>
            </div>
            
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 max-w-4xl mx-auto">
                <button onclick="selectType('vcard')" class="type-card active bg-white rounded-2xl p-6 text-center shadow-sm">
                    <div class="w-14 h-14 bg-emerald-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-id-card text-2xl text-emerald-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">vCard</h3>
                    <p class="text-xs text-gray-500">Standard digital card</p>
                </button>
                
                <button onclick="selectType('business')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm">
                    <div class="w-14 h-14 bg-blue-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-briefcase text-2xl text-blue-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Business</h3>
                    <p class="text-xs text-gray-500">Company profile</p>
                </button>
                
                <button onclick="selectType('personal')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm">
                    <div class="w-14 h-14 bg-purple-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-user text-2xl text-purple-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Personal</h3>
                    <p class="text-xs text-gray-500">Individual profile</p>
                </button>
                
                <button onclick="selectType('creative')" class="type-card bg-white rounded-2xl p-6 text-center shadow-sm">
                    <div class="w-14 h-14 bg-pink-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-palette text-2xl text-pink-600"></i>
                    </div>
                    <h3 class="font-semibold text-gray-900 mb-1">Creative</h3>
                    <p class="text-xs text-gray-500">Portfolio style</p>
                </button>
            </div>
        </section>

        <!-- STEP 2: Information -->
        <section id="step-2" class="tab-content">
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <!-- Form -->
                <div class="space-y-4">
                    <button onclick="goToStep(1)" class="flex items-center gap-2 text-gray-500 hover:text-gray-700 mb-4">
                        <i class="fas fa-arrow-left"></i> Back
                    </button>
                    
                    <!-- Templates -->
                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
                        <h3 class="font-bold text-lg mb-4">Card Template</h3>
                        <div class="grid grid-cols-4 gap-3">
                            <button onclick="setCardTemplate('green')" class="h-20 rounded-xl bg-gradient-to-br from-emerald-800 to-emerald-600 border-4 border-emerald-500 shadow-lg transform scale-105" data-template="green"></button>
                            <button onclick="setCardTemplate('blue')" class="h-20 rounded-xl bg-gradient-to-br from-blue-800 to-blue-600 border-2 border-transparent hover:border-blue-400 transition-all" data-template="blue"></button>
                            <button onclick="setCardTemplate('purple')" class="h-20 rounded-xl bg-gradient-to-br from-purple-800 to-purple-600 border-2 border-transparent hover:border-purple-400 transition-all" data-template="purple"></button>
                            <button onclick="setCardTemplate('dark')" class="h-20 rounded-xl bg-gradient-to-br from-gray-900 to-gray-700 border-2 border-transparent hover:border-gray-500 transition-all" data-template="dark"></button>
                        </div>
                    </div>

                    <!-- Personal Info -->
                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
                        <div class="accordion-item">
                            <div class="accordion-header flex items-center justify-between p-4" onclick="toggleAccordion(this)">
                                <div class="flex items-center gap-3">
                                    <div class="w-10 h-10 bg-emerald-100 rounded-lg flex items-center justify-center">
                                        <i class="fas fa-user text-emerald-600"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-semibold">Personal Information</h4>
                                        <p class="text-xs text-gray-500">Name, photo, and title</p>
                                    </div>
                                </div>
                                <i class="fas fa-chevron-down text-gray-400 transition-transform"></i>
                            </div>
                            <div class="accordion-content open px-4 pb-4">
                                <div class="space-y-4">
                                    <div class="flex items-center gap-4">
                                        <div class="relative">
                                            <div id="form-photo-preview" class="w-20 h-20 rounded-full bg-gray-200 flex items-center justify-center overflow-hidden border-4 border-dashed border-gray-300">
                                                <i class="fas fa-camera text-gray-400 text-xl"></i>
                                            </div>
                                            <label class="absolute bottom-0 right-0 w-8 h-8 bg-emerald-500 text-white rounded-full flex items-center justify-center cursor-pointer hover:bg-emerald-600 transition-all shadow-lg">
                                                <i class="fas fa-plus text-sm"></i>
                                                <input type="file" id="profile-photo" accept="image/*" class="hidden" onchange="handlePhotoUpload(event)">
                                            </label>
                                        </div>
                                        <div class="flex-1">
                                            <input type="text" id="first-name" placeholder="First Name" class="input-field w-full px-4 py-3 rounded-xl mb-2" oninput="updateCard()">
                                            <input type="text" id="last-name" placeholder="Last Name" class="input-field w-full px-4 py-3 rounded-xl" oninput="updateCard()">
                                        </div>
                                    </div>
                                    <input type="text" id="job-title" placeholder="Job Title (e.g., Account Manager)" class="input-field w-full px-4 py-3 rounded-xl" oninput="updateCard()">
                                    <input type="text" id="department" placeholder="Department (optional)" class="input-field w-full px-4 py-3 rounded-xl" oninput="updateCard()">
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Contact Details -->
                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
                        <div class="accordion-item">
                            <div class="accordion-header flex items-center justify-between p-4" onclick="toggleAccordion(this)">
                                <div class="flex items-center gap-3">
                                    <div class="w-10 h-10 bg-blue-100 rounded-lg flex items-center justify-center">
                                        <i class="fas fa-address-book text-blue-600"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-semibold">Contact Details</h4>
                                        <p class="text-xs text-gray-500">Phone, email, website</p>
                                    </div>
                                </div>
                                <i class="fas fa-chevron-down text-gray-400 transition-transform"></i>
                            </div>
                            <div class="accordion-content px-4 pb-4">
                                <div class="space-y-3">
                                    <div class="flex gap-2">
                                        <select class="input-field px-3 py-3 rounded-xl w-24">
                                            <option>Work</option>
                                            <option>Mobile</option>
                                            <option>Home</option>
                                        </select>
                                        <input type="tel" id="phone" placeholder="+1 234 567 890" class="input-field flex-1 px-4 py-3 rounded-xl" oninput="updateCard()">
                                        <button class="w-12 h-12 bg-emerald-500 text-white rounded-xl flex items-center justify-center hover:bg-emerald-600">
                                            <i class="fas fa-plus"></i>
                                        </button>
                                    </div>
                                    <div class="flex gap-2">
                                        <select class="input-field px-3 py-3 rounded-xl w-24">
                                            <option>Work</option>
                                            <option>Personal</option>
                                        </select>
                                        <input type="email" id="email" placeholder="john@company.com" class="input-field flex-1 px-4 py-3 rounded-xl" oninput="updateCard()">
                                        <button class="w-12 h-12 bg-emerald-500 text-white rounded-xl flex items-center justify-center hover:bg-emerald-600">
                                            <i class="fas fa-plus"></i>
                                        </button>
                                    </div>
                                    <input type="url" id="website" placeholder="www.company.com" class="input-field w-full px-4 py-3 rounded-xl" oninput="updateCard()">
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Location -->
                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
                        <div class="accordion-item">
                            <div class="accordion-header flex items-center justify-between p-4" onclick="toggleAccordion(this)">
                                <div class="flex items-center gap-3">
                                    <div class="w-10 h-10 bg-green-100 rounded-lg flex items-center justify-center">
                                        <i class="fas fa-map-marker-alt text-green-600"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-semibold">Location</h4>
                                        <p class="text-xs text-gray-500">Office address</p>
                                    </div>
                                </div>
                                <i class="fas fa-chevron-down text-gray-400 transition-transform"></i>
                            </div>
                            <div class="accordion-content px-4 pb-4">
                                <input type="text" id="address" placeholder="Street Address" class="input-field w-full px-4 py-3 rounded-xl mb-3" oninput="updateCard()">
                                <div class="grid grid-cols-2 gap-3">
                                    <input type="text" id="city" placeholder="City" class="input-field w-full px-4 py-3 rounded-xl" oninput="updateCard()">
                                    <input type="text" id="country" placeholder="Country" class="input-field w-full px-4 py-3 rounded-xl" oninput="updateCard()">
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Company -->
                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
                        <div class="accordion-item">
                            <div class="accordion-header flex items-center justify-between p-4" onclick="toggleAccordion(this)">
                                <div class="flex items-center gap-3">
                                    <div class="w-10 h-10 bg-purple-100 rounded-lg flex items-center justify-center">
                                        <i class="fas fa-building text-purple-600"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-semibold">Company Details</h4>
                                        <p class="text-xs text-gray-500">Organization info</p>
                                    </div>
                                </div>
                                <i class="fas fa-chevron-down text-gray-400 transition-transform"></i>
                            </div>
                            <div class="accordion-content px-4 pb-4">
                                <input type="text" id="company-name" placeholder="Company Name" class="input-field w-full px-4 py-3 rounded-xl mb-3" oninput="updateCard()">
                                <textarea id="bio" placeholder="Professional summary or bio..." rows="3" class="input-field w-full px-4 py-3 rounded-xl resize-none" oninput="updateCard()"></textarea>
                            </div>
                        </div>
                    </div>

                    <!-- Social Networks -->
                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
                        <div class="accordion-item border-0">
                            <div class="accordion-header flex items-center justify-between p-4" onclick="toggleAccordion(this)">
                                <div class="flex items-center gap-3">
                                    <div class="w-10 h-10 bg-pink-100 rounded-lg flex items-center justify-center">
                                        <i class="fas fa-share-alt text-pink-600"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-semibold">Social Networks</h4>
                                        <p class="text-xs text-gray-500">Connect your profiles</p>
                                    </div>
                                </div>
                                <i class="fas fa-chevron-down text-gray-400 transition-transform"></i>
                            </div>
                            <div class="accordion-content open px-4 pb-4">
                                <div class="social-grid mb-4">
                                    <button onclick="toggleSocial('linkedin')" class="social-select bg-blue-500 text-white" data-social="linkedin"><i class="fab fa-linkedin-in"></i></button>
                                    <button onclick="toggleSocial('twitter')" class="social-select bg-sky-400 text-white" data-social="twitter"><i class="fab fa-twitter"></i></button>
                                    <button onclick="toggleSocial('instagram')" class="social-select bg-gradient-to-br from-purple-500 to-pink-500 text-white" data-social="instagram"><i class="fab fa-instagram"></i></button>
                                    <button onclick="toggleSocial('facebook')" class="social-select bg-blue-600 text-white" data-social="facebook"><i class="fab fa-facebook-f"></i></button>
                                    <button onclick="toggleSocial('youtube')" class="social-select bg-red-600 text-white" data-social="youtube"><i class="fab fa-youtube"></i></button>
                                    <button onclick="toggleSocial('tiktok')" class="social-select bg-black text-white" data-social="tiktok"><i class="fab fa-tiktok"></i></button>
                                    <button onclick="toggleSocial('github')" class="social-select bg-gray-800 text-white" data-social="github"><i class="fab fa-github"></i></button>
                                    <button onclick="toggleSocial('dribbble')" class="social-select bg-pink-400 text-white" data-social="dribbble"><i class="fab fa-dribbble"></i></button>
                                    <button onclick="toggleSocial('behance')" class="social-select bg-blue-700 text-white" data-social="behance"><i class="fab fa-behance"></i></button>
                                    <button onclick="toggleSocial('medium')" class="social-select bg-gray-700 text-white" data-social="medium"><i class="fab fa-medium-m"></i></button>
                                    <button onclick="toggleSocial('spotify')" class="social-select bg-green-500 text-white" data-social="spotify"><i class="fab fa-spotify"></i></button>
                                    <button onclick="toggleSocial('snapchat')" class="social-select bg-yellow-400 text-black" data-social="snapchat"><i class="fab fa-snapchat-ghost"></i></button>
                                </div>
                                <div id="social-inputs" class="space-y-2">
                                    <input type="text" id="social-linkedin" placeholder="LinkedIn username" class="input-field w-full px-4 py-3 rounded-xl hidden" oninput="updateCard()">
                                    <input type="text" id="social-twitter" placeholder="Twitter username" class="input-field w-full px-4 py-3 rounded-xl hidden" oninput="updateCard()">
                                    <input type="text" id="social-instagram" placeholder="Instagram username" class="input-field w-full px-4 py-3 rounded-xl hidden" oninput="updateCard()">
                                </div>
                            </div>
                        </div>
                    </div>

                    <button onclick="goToStep(3)" class="w-full btn-primary py-4 rounded-xl text-white font-bold text-lg shadow-lg">
                        Continue to Design <i class="fas fa-arrow-right ml-2"></i>
                    </button>
                </div>

                <!-- iPhone Preview -->
                <div class="hidden lg:block">
                    <div class="phone-mockup">
                        <div class="phone-screen">
                            <div class="notch"></div>
                            <div class="digital-card bg-white">
                                <!-- Card Header -->
                                <div class="card-header pt-12 pb-20 px-6 text-white relative">
                                    <div class="flex justify-between items-start mb-6">
                                        <button class="w-10 h-10 rounded-full bg-white/20 flex items-center justify-center backdrop-blur-sm">
                                            <i class="fas fa-arrow-left"></i>
                                        </button>
                                        <button class="w-10 h-10 rounded-full bg-white/20 flex items-center justify-center backdrop-blur-sm">
                                            <i class="fas fa-ellipsis-v"></i>
                                        </button>
                                    </div>
                                    
                                    <div class="text-center relative z-10">
                                        <div id="card-photo" class="profile-img w-24 h-24 rounded-full bg-gray-300 mx-auto mb-4 flex items-center justify-center overflow-hidden">
                                            <i class="fas fa-user text-4xl text-gray-500"></i>
                                        </div>
                                        <h2 id="card-name" class="text-2xl font-bold mb-1">John Doe</h2>
                                        <p id="card-title" class="text-emerald-200 text-sm">Account Manager</p>
                                        
                                        <div class="flex justify-center gap-2 mt-4">
                                            <span id="card-dept" class="px-3 py-1 bg-white/20 rounded-full text-xs backdrop-blur-sm hidden">Sales</span>
                                        </div>
                                    </div>
                                    
                                    <div class="wave-bottom"></div>
                                </div>

                                <!-- Card Body -->
                                <div class="px-6 pb-6 -mt-10 relative z-10">
                                    <!-- Action Buttons -->
                                    <div class="flex justify-center gap-4 mb-6">
                                        <button class="action-btn w-14 h-14 rounded-2xl bg-emerald-500 text-white flex items-center justify-center shadow-lg">
                                            <i class="fas fa-comment-dots text-xl"></i>
                                        </button>
                                        <button class="action-btn w-14 h-14 rounded-2xl bg-blue-500 text-white flex items-center justify-center shadow-lg">
                                            <i class="fas fa-phone text-xl"></i>
                                        </button>
                                        <button class="action-btn w-14 h-14 rounded-2xl bg-purple-500 text-white flex items-center justify-center shadow-lg">
                                            <i class="fas fa-video text-xl"></i>
                                        </button>
                                        <button class="action-btn w-14 h-14 rounded-2xl bg-gray-800 text-white flex items-center justify-center shadow-lg">
                                            <i class="fas fa-envelope text-xl"></i>
                                        </button>
                                    </div>

                                    <!-- Info Sections -->
                                    <div class="space-y-4">
                                        <div class="bg-gray-50 rounded-2xl p-4">
                                            <p id="card-bio" class="text-gray-600 text-sm leading-relaxed text-center">
                                                Professional account manager with 5+ years of experience building client relationships.
                                            </p>
                                        </div>

                                        <div class="space-y-3">
                                            <a id="card-phone-link" href="#" class="flex items-center gap-4 p-4 bg-gray-50 rounded-2xl hover:bg-gray-100 transition-colors">
                                                <div class="w-12 h-12 rounded-xl bg-emerald-100 flex items-center justify-center text-emerald-600">
                                                    <i class="fas fa-phone"></i>
                                                </div>
                                                <div class="flex-1">
                                                    <p class="text-xs text-gray-500">Mobile</p>
                                                    <p id="card-phone" class="font-semibold text-gray-800">+1 234 567 890</p>
                                                </div>
                                                <i class="fas fa-chevron-right text-gray-400"></i>
                                            </a>

                                            <a id="card-email-link" href="#" class="flex items-center gap-4 p-4 bg-gray-50 rounded-2xl hover:bg-gray-100 transition-colors">
                                                <div class="w-12 h-12 rounded-xl bg-blue-100 flex items-center justify-center text-blue-600">
                                                    <i class="fas fa-envelope"></i>
                                                </div>
                                                <div class="flex-1">
                                                    <p class="text-xs text-gray-500">Work Email</p>
                                                    <p id="card-email" class="font-semibold text-gray-800">john@company.com</p>
                                                </div>
                                                <i class="fas fa-chevron-right text-gray-400"></i>
                                            </a>

                                            <a id="card-website-link" href="#" class="flex items-center gap-4 p-4 bg-gray-50 rounded-2xl hover:bg-gray-100 transition-colors">
                                                <div class="w-12 h-12 rounded-xl bg-purple-100 flex items-center justify-center text-purple-600">
                                                    <i class="fas fa-globe"></i>
                                                </div>
                                                <div class="flex-1">
                                                    <p class="text-xs text-gray-500">Website</p>
                                                    <p id="card-website" class="font-semibold text-gray-800">www.company.com</p>
                                                </div>
                                                <i class="fas fa-chevron-right text-gray-400"></i>
                                            </a>

                                            <div class="flex items-center gap-4 p-4 bg-gray-50 rounded-2xl">
                                                <div class="w-12 h-12 rounded-xl bg-orange-100 flex items-center justify-center text-orange-600">
                                                    <i class="fas fa-map-marker-alt"></i>
                                                </div>
                                                <div class="flex-1">
                                                    <p class="text-xs text-gray-500">Address</p>
                                                    <p id="card-address" class="font-semibold text-gray-800 text-sm">123 Business St, New York</p>
                                                </div>
                                            </div>
                                        </div>

                                        <!-- Social Links -->
                                        <div class="pt-4 border-t border-gray-100">
                                            <p class="text-xs text-gray-500 text-center mb-4">Connect on social media</p>
                                            <div id="card-socials" class="flex justify-center gap-4">
                                                <a href="#" class="social-icon w-12 h-12 rounded-full bg-blue-500 text-white flex items-center justify-center text-lg shadow-md"><i class="fab fa-linkedin-in"></i></a>
                                                <a href="#" class="social-icon w-12 h-12 rounded-full bg-sky-400 text-white flex items-center justify-center text-lg shadow-md"><i class="fab fa-twitter"></i></a>
                                                <a href="#" class="social-icon w-12 h-12 rounded-full bg-gradient-to-br from-purple-500 to-pink-500 text-white flex items-center justify-center text-lg shadow-md"><i class="fab fa-instagram"></i></a>
                                            </div>
                                        </div>

                                        <!-- Save Contact -->
                                        <button class="w-full py-4 bg-gray-900 text-white rounded-2xl font-semibold flex items-center justify-center gap-2 hover:bg-gray-800 transition-colors">
                                            <i class="fas fa-user-plus"></i>
                                            Save to Contacts
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- STEP 3: Design -->
        <section id="step-3" class="tab-content">
            <div class="max-w-4xl mx-auto">
                <button onclick="goToStep(2)" class="flex items-center gap-2 text-gray-500 hover:text-gray-700 mb-6">
                    <i class="fas fa-arrow-left"></i> Back
                </button>
                
                <h2 class="text-2xl font-bold mb-6">Customize your card design</h2>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
                        <h3 class="font-semibold mb-4">Colors</h3>
                        <div class="flex gap-3 flex-wrap">
                            <button onclick="setAccentColor('emerald')" class="w-12 h-12 rounded-full bg-emerald-500 border-4 border-emerald-200"></button>
                            <button onclick="setAccentColor('blue')" class="w-12 h-12 rounded-full bg-blue-500 border-4 border-transparent hover:border-blue-200"></button>
                            <button onclick="setAccentColor('purple')" class="w-12 h-12 rounded-full bg-purple-500 border-4 border-transparent hover:border-purple-200"></button>
                            <button onclick="setAccentColor('orange')" class="w-12 h-12 rounded-full bg-orange-500 border-4 border-transparent hover:border-orange-200"></button>
                            <button onclick="setAccentColor('red')" class="w-12 h-12 rounded-full bg-red-500 border-4 border-transparent hover:border-red-200"></button>
                        </div>
                    </div>

                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
                        <h3 class="font-semibold mb-4">Button Style</h3>
                        <div class="space-y-2">
                            <button class="w-full p-3 rounded-xl bg-emerald-500 text-white text-left flex items-center gap-3">
                                <i class="fas fa-check-circle"></i> Rounded (Default)
                            </button>
                            <button class="w-full p-3 rounded-xl bg-gray-100 text-gray-700 text-left flex items-center gap-3">
                                <i class="far fa-circle"></i> Square
                            </button>
                        </div>
                    </div>
                </div>

                <button onclick="goToStep(4)" class="w-full mt-8 btn-primary py-4 rounded-xl text-white font-bold text-lg shadow-lg">
                    Generate QR Code <i class="fas fa-arrow-right ml-2"></i>
                </button>
            </div>
        </section>

        <!-- STEP 4: Download -->
        <section id="step-4" class="tab-content">
            <div class="max-w-2xl mx-auto text-center">
                <button onclick="goToStep(3)" class="flex items-center gap-2 text-gray-500 hover:text-gray-700 mb-6">
                    <i class="fas fa-arrow-left"></i> Back
                </button>
                
                <div class="bg-white rounded-3xl p-12 shadow-lg border border-gray-100">
                    <div class="w-20 h-20 bg-emerald-100 rounded-full flex items-center justify-center mx-auto mb-6">
                        <i class="fas fa-check text-4xl text-emerald-600"></i>
                    </div>
                    
                    <h2 class="text-3xl font-bold mb-4">Your digital card is ready!</h2>
                    <p class="text-gray-500 mb-8">Scan the QR code to see your animated landing page</p>
                    
                    <div class="flex justify-center mb-8">
                        <div id="final-qr" class="p-6 bg-white rounded-2xl shadow-xl border-2 border-gray-100"></div>
                    </div>

                    <div class="flex flex-wrap justify-center gap-3 mb-8">
                        <button onclick="downloadQR('png')" class="px-6 py-3 bg-gray-100 rounded-xl font-semibold hover:bg-gray-200 transition-all">
                            <i class="fas fa-download mr-2"></i>PNG
                        </button>
                        <button onclick="downloadQR('svg')" class="px-6 py-3 bg-gray-100 rounded-xl font-semibold hover:bg-gray-200 transition-all">
                            <i class="fas fa-vector-square mr-2"></i>SVG
                        </button>
                        <button onclick="copyQRLink()" class="px-6 py-3 bg-emerald-100 text-emerald-700 rounded-xl font-semibold hover:bg-emerald-200 transition-all">
                            <i class="fas fa-link mr-2"></i>Copy Link
                        </button>
                    </div>

                    <button onclick="previewLandingPage()" class="w-full py-4 bg-gray-900 text-white rounded-xl font-bold flex items-center justify-center gap-2 hover:bg-gray-800 transition-all">
                        <i class="fas fa-external-link-alt"></i>
                        Preview Landing Page
                    </button>
                </div>
            </div>
        </section>

    </main>

    <!-- LANDING PAGE PREVIEW (Full Screen) -->
    <div id="landing-page" class="landing-page">
        <button onclick="closeLandingPage()" class="fixed top-6 right-6 z-50 w-12 h-12 bg-white/20 backdrop-blur-lg rounded-full flex items-center justify-center text-white hover:bg-white/30 transition-all">
            <i class="fas fa-times text-xl"></i>
        </button>

        <div class="landing-hero">
            <div class="landing-bg"></div>
            
            <div class="landing-content">
                <img id="landing-photo" src="" alt="Profile" class="landing-profile">
                
                <h1 id="landing-name" class="landing-name">John Doe</h1>
                <p id="landing-title" class="landing-title">Account Manager</p>
                
                <div class="landing-actions">
                    <button onclick="showLandingSection('contact')" class="landing-btn bg-white text-emerald-800">
                        <i class="fas fa-address-card"></i>
                        Contact Info
                    </button>
                    <button onclick="showLandingSection('about')" class="landing-btn bg-emerald-500 text-white">
                        <i class="fas fa-user"></i>
                        About Me
                    </button>
                    <button onclick="showLandingSection('social')" class="landing-btn bg-white/20 text-white backdrop-blur-lg">
                        <i class="fas fa-share-alt"></i>
                        Social
                    </button>
                </div>
                
                <div class="mt-12 animate-bounce">
                    <i class="fas fa-chevron-down text-white text-2xl"></i>
                </div>
            </div>
        </div>

        <div id="landing-contact" class="landing-section">
            <h2 class="text-3xl font-bold text-gray-800 mb-8 text-center">Contact Information</h2>
            
            <div class="space-y-4">
                <a id="landing-phone" href="#" class="info-card flex items-center gap-4 p-6">
                    <div class="w-16 h-16 rounded-2xl bg-emerald-100 flex items-center justify-center text-emerald-600 text-2xl">
                        <i class="fas fa-phone"></i>
                    </div>
                    <div class="flex-1">
                        <p class="text-sm text-gray-500">Phone</p>
                        <p class="text-xl font-bold text-gray-800">+1 234 567 890</p>
                    </div>
                    <i class="fas fa-chevron-right text-gray-400 text-xl"></i>
                </a>

                <a id="landing-email" href="#" class="info-card flex items-center gap-4 p-6">
                    <div class="w-16 h-16 rounded-2xl bg-blue-100 flex items-center justify-center text-blue-600 text-2xl">
                        <i class="fas fa-envelope"></i>
                    </div>
                    <div class="flex-1">
                        <p class="text-sm text-gray-500">Email</p>
                        <p class="text-xl font-bold text-gray-800">john@company.com</p>
                    </div>
                    <i class="fas fa-chevron-right text-gray-400 text-xl"></i>
                </a>

                <a id="landing-website" href="#" class="info-card flex items-center gap-4 p-6">
                    <div class="w-16 h-16 rounded-2xl bg-purple-100 flex items-center justify-center text-purple-600 text-2xl">
                        <i class="fas fa-globe"></i>
                    </div>
                    <div class="flex-1">
                        <p class="text-sm text-gray-500">Website</p>
                        <p class="text-xl font-bold text-gray-800">www.company.com</p>
                    </div>
                    <i class="fas fa-chevron-right text-gray-400 text-xl"></i>
                </a>

                <div class="info-card p-6">
                    <div class="flex items-center gap-4 mb-4">
                        <div class="w-16 h-16 rounded-2xl bg-orange-100 flex items-center justify-center text-orange-600 text-2xl">
                            <i class="fas fa-map-marker-alt"></i>
                        </div>
                        <div>
                            <p class="text-sm text-gray-500">Address</p>
                            <p id="landing-address" class="text-lg font-bold text-gray-800">123 Business St, New York</p>
                        </div>
                    </div>
                    <div class="h-48 bg-gray-200 rounded-xl mt-4 flex items-center justify-center">
                        <i class="fas fa-map text-4xl text-gray-400"></i>
                    </div>
                </div>
            </div>
        </div>

        <div id="landing-about" class="landing-section bg-gray-50">
            <h2 class="text-3xl font-bold text-gray-800 mb-8 text-center">About Me</h2>
            
            <div class="info-card bg-white p-8">
                <div class="flex items-center gap-4 mb-6">
                    <img id="landing-about-photo" src="" class="w-20 h-20 rounded-full object-cover">
                    <div>
                        <h3 id="landing-about-name" class="text-xl font-bold">John Doe</h3>
                        <p id="landing-about-title" class="text-emerald-600">Account Manager</p>
                    </div>
                </div>
                
                <p id="landing-bio" class="text-gray-600 leading-relaxed text-lg">
                    Professional account manager with 5+ years of experience building client relationships and driving business growth.
                </p>

                <div class="mt-8 pt-6 border-t border-gray-100">
                    <h4 class="font-semibold mb-4">Company</h4>
                    <div class="flex items-center gap-4">
                        <div class="w-16 h-16 rounded-xl bg-gray-100 flex items-center justify-center">
                            <i class="fas fa-building text-2xl text-gray-400"></i>
                        </div>
                        <div>
                            <p id="landing-company" class="font-bold text-gray-800">Company Inc.</p>
                            <p class="text-sm text-gray-500">Technology & Solutions</p>
                        </div>
                    </div>
                </div>
            </div>

            <div class="mt-6 text-center">
                <button onclick="downloadVCard()" class="px-8 py-4 bg-emerald-500 text-white rounded-full font-bold text-lg shadow-lg hover:bg-emerald-600 transition-all flex items-center gap-2 mx-auto">
                    <i class="fas fa-download"></i>
                    Download vCard
                </button>
            </div>
        </div>

        <div id="landing-social" class="landing-section">
            <h2 class="text-3xl font-bold text-gray-800 mb-8 text-center">Connect With Me</h2>
            
            <div id="landing-socials" class="grid grid-cols-2 gap-4">
                <a href="#" class="info-card p-6 flex items-center gap-4 hover:bg-blue-50 transition-colors">
                    <div class="w-14 h-14 rounded-xl bg-blue-500 text-white flex items-center justify-center text-2xl">
                        <i class="fab fa-linkedin-in"></i>
                    </div>
                    <div>
                        <p class="font-bold text-gray-800">LinkedIn</p>
                        <p class="text-sm text-gray-500">Professional network</p>
                    </div>
                </a>

                <a href="#" class="info-card p-6 flex items-center gap-4 hover:bg-sky-50 transition-colors">
                    <div class="w-14 h-14 rounded-xl bg-sky-400 text-white flex items-center justify-center text-2xl">
                        <i class="fab fa-twitter"></i>
                    </div>
                    <div>
                        <p class="font-bold text-gray-800">Twitter</p>
                        <p class="text-sm text-gray-500">Follow me</p>
                    </div>
                </a>

                <a href="#" class="info-card p-6 flex items-center gap-4 hover:bg-pink-50 transition-colors">
                    <div class="w-14 h-14 rounded-xl bg-gradient-to-br from-purple-500 to-pink-500 text-white flex items-center justify-center text-2xl">
                        <i class="fab fa-instagram"></i>
                    </div>
                    <div>
                        <p class="font-bold text-gray-800">Instagram</p>
                        <p class="text-sm text-gray-500">Photos & stories</p>
                    </div>
                </a>

                <a href="#" class="info-card p-6 flex items-center gap-4 hover:bg-gray-100 transition-colors">
                    <div class="w-14 h-14 rounded-xl bg-gray-800 text-white flex items-center justify-center text-2xl">
                        <i class="fab fa-github"></i>
                    </div>
                    <div>
                        <p class="font-bold text-gray-800">GitHub</p>
                        <p class="text-sm text-gray-500">Code & projects</p>
                    </div>
                </a>
            </div>
        </div>

        <div class="py-12 text-center text-gray-400 text-sm">
            <p>Created with QR Code Digital Business Card</p>
        </div>
    </div>

    <!-- Toast -->
    <div id="toast" class="toast">
        <i class="fas fa-check-circle text-emerald-400 text-xl mr-3"></i>
        <span id="toast-message">Success</span>
    </div>

    <script>
        let currentStep = 1;
        let cardData = {
            template: 'green',
            photo: null,
            firstName: '',
            lastName: '',
            title: '',
            department: '',
            phone: '',
            email: '',
            website: '',
            address: '',
            city: '',
            country: '',
            company: '',
            bio: '',
            socials: {}
        };

        // Step Management
        function goToStep(step) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.getElementById(`step-${step}`).classList.add('active');
            
            // Update indicators
            for (let i = 1; i <= 4; i++) {
                const indicator = document.getElementById(`step-${i}-indicator`);
                const line = document.getElementById(`line-${i}`);
                
                if (i <= step) {
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
                    line.classList.toggle('bg-emerald-500', i < step);
                    line.classList.toggle('bg-gray-200', i >= step);
                }
            }
            
            currentStep = step;
            
            if (step === 4) generateFinalQR();
        }

        function selectType(type) {
            document.querySelectorAll('.type-card').forEach(c => c.classList.remove('active'));
            event.currentTarget.classList.add('active');
            setTimeout(() => goToStep(2), 300);
        }

        // Accordion
        function toggleAccordion(header) {
            const content = header.nextElementSibling;
            const icon = header.querySelector('.fa-chevron-down');
            
            content.classList.toggle('open');
            icon.style.transform = content.classList.contains('open') ? 'rotate(180deg)' : 'rotate(0)';
        }

        // Photo Upload
        function handlePhotoUpload(event) {
            const file = event.target.files[0];
            if (!file) return;
            
            const reader = new FileReader();
            reader.onload = (e) => {
                cardData.photo = e.target.result;
                document.getElementById('form-photo-preview').innerHTML = `<img src="${e.target.result}" class="w-full h-full object-cover">`;
                document.getElementById('card-photo').innerHTML = `<img src="${e.target.result}" class="w-full h-full object-cover">`;
                updateCard();
            };
            reader.readAsDataURL(file);
        }

        // Card Template
        function setCardTemplate(template) {
            cardData.template = template;
            document.querySelectorAll('[data-template]').forEach(btn => {
                btn.classList.remove('border-4', 'border-emerald-500', 'scale-105', 'shadow-lg');
                btn.classList.add('border-2', 'border-transparent');
            });
            event.currentTarget.classList.remove('border-2', 'border-transparent');
            event.currentTarget.classList.add('border-4', 'border-emerald-500', 'scale-105', 'shadow-lg');
            
            // Update preview colors
            const colors = {
                green: 'from-emerald-800 to-emerald-600',
                blue: 'from-blue-800 to-blue-600',
                purple: 'from-purple-800 to-purple-600',
                dark: 'from-gray-900 to-gray-700'
            };
            
            const header = document.querySelector('.card-header');
            header.className = `card-header pt-12 pb-20 px-6 text-white relative bg-gradient-to-br ${colors[template]}`;
        }

        // Social Toggle
        function toggleSocial(platform) {
            const btn = event.currentTarget;
            btn.classList.toggle('active');
            
            const input = document.getElementById(`social-${platform}`);
            if (input) {
                input.classList.toggle('hidden');
                if (!input.classList.contains('hidden')) {
                    input.focus();
                }
            }
            
            // Update card preview
            updateCard();
        }

        // Update Card Preview
        function updateCard() {
            cardData.firstName = document.getElementById('first-name')?.value || 'John';
            cardData.lastName = document.getElementById('last-name')?.value || 'Doe';
            cardData.title = document.getElementById('job-title')?.value || 'Account Manager';
            cardData.department = document.getElementById('department')?.value || '';
            cardData.phone = document.getElementById('phone')?.value || '+1 234 567 890';
            cardData.email = document.getElementById('email')?.value || 'john@company.com';
            cardData.website = document.getElementById('website')?.value || 'www.company.com';
            cardData.address = document.getElementById('address')?.value || '123 Business St';
            cardData.city = document.getElementById('city')?.value || 'New York';
            cardData.country = document.getElementById('country')?.value || 'USA';
            cardData.company = document.getElementById('company-name')?.value || 'Company Inc.';
            cardData.bio = document.getElementById('bio')?.value || 'Professional account manager with 5+ years of experience building client relationships.';
            
            // Update preview
            document.getElementById('card-name').textContent = `${cardData.firstName} ${cardData.lastName}`;
            document.getElementById('card-title').textContent = cardData.title;
            document.getElementById('card-dept').textContent = cardData.department;
            document.getElementById('card-dept').classList.toggle('hidden', !cardData.department);
            document.getElementById('card-phone').textContent = cardData.phone;
            document.getElementById('card-email').textContent = cardData.email;
            document.getElementById('card-website').textContent = cardData.website;
            document.getElementById('card-address').textContent = `${cardData.address}, ${cardData.city}`;
            document.getElementById('card-bio').textContent = cardData.bio;
            
            // Update links
            document.getElementById('card-phone-link').href = `tel:${cardData.phone}`;
            document.getElementById('card-email-link').href = `mailto:${cardData.email}`;
            document.getElementById('card-website-link').href = `https://${cardData.website}`;
        }

        // Generate Final QR
        function generateFinalQR() {
            const vcard = `BEGIN:VCARD\nVERSION:3.0\nFN:${cardData.firstName} ${cardData.lastName}\nTITLE:${cardData.title}\nTEL:${cardData.phone}\nEMAIL:${cardData.email}\nURL:${cardData.website}\nADR:;;${cardData.address};${cardData.city};;${cardData.country}\nORG:${cardData.company}\nNOTE:${cardData.bio}\nEND:VCARD`;
            
            const container = document.getElementById('final-qr');
            container.innerHTML = '';
            
            new QRCode(container, {
                text: vcard,
                width: 200,
                height: 200,
                colorDark: '#064e3b',
                colorLight: '#ffffff',
                correctLevel: QRCode.CorrectLevel.H
            });
        }

        // Download QR
        function downloadQR(format) {
            const canvas = document.querySelector('#final-qr canvas');
            if (!canvas) return;
            
            if (format === 'png') {
                const link = document.createElement('a');
                link.download = `business-card-${Date.now()}.png`;
                link.href = canvas.toDataURL('image/png');
                link.click();
            } else if (format === 'svg') {
                const svg = `<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200">
                    <image href="${canvas.toDataURL()}" width="200" height="200"/>
                </svg>`;
                const blob = new Blob([svg], {type: 'image/svg+xml'});
                const url = URL.createObjectURL(blob);
                const link = document.createElement('a');
                link.download = `business-card-${Date.now()}.svg`;
                link.href = url;
                link.click();
            }
            
            showToast(`Downloaded as ${format.toUpperCase()}`);
        }

        // Landing Page Preview
        function previewLandingPage() {
            const landing = document.getElementById('landing-page');
            
            // Populate landing page
            document.getElementById('landing-name').textContent = `${cardData.firstName} ${cardData.lastName}`;
            document.getElementById('landing-title').textContent = cardData.title;
            document.getElementById('landing-phone').href = `tel:${cardData.phone}`;
            document.getElementById('landing-phone').querySelector('p:last-child').textContent = cardData.phone;
            document.getElementById('landing-email').href = `mailto:${cardData.email}`;
            document.getElementById('landing-email').querySelector('p:last-child').textContent = cardData.email;
            document.getElementById('landing-website').href = `https://${cardData.website}`;
            document.getElementById('landing-website').querySelector('p:last-child').textContent = cardData.website;
            document.getElementById('landing-address').textContent = `${cardData.address}, ${cardData.city}, ${cardData.country}`;
            document.getElementById('landing-bio').textContent = cardData.bio;
            document.getElementById('landing-company').textContent = cardData.company;
            document.getElementById('landing-about-name').textContent = `${cardData.firstName} ${cardData.lastName}`;
            document.getElementById('landing-about-title').textContent = cardData.title;
            
            if (cardData.photo) {
                document.getElementById('landing-photo').src = cardData.photo;
                document.getElementById('landing-about-photo').src = cardData.photo;
            }
            
            landing.classList.add('active');
            document.body.style.overflow = 'hidden';
            
            // Animate cards on scroll
            setTimeout(() => {
                document.querySelectorAll('.info-card').forEach((card, index) => {
                    setTimeout(() => card.classList.add('visible'), index * 100);
                });
            }, 500);
        }

        function closeLandingPage() {
            document.getElementById('landing-page').classList.remove('active');
            document.body.style.overflow = '';
        }

        function showLandingSection(section) {
            const element = document.getElementById(`landing-${section}`);
            element.scrollIntoView({ behavior: 'smooth' });
        }

        function downloadVCard() {
            const vcard = `BEGIN:VCARD\nVERSION:3.0\nFN:${cardData.firstName} ${cardData.lastName}\nTITLE:${cardData.title}\nTEL:${cardData.phone}\nEMAIL:${cardData.email}\nURL:${cardData.website}\nADR:;;${cardData.address};${cardData.city};;${cardData.country}\nORG:${cardData.company}\nNOTE:${cardData.bio}\nEND:VCARD`;
            
            const blob = new Blob([vcard], {type: 'text/vcard'});
            const url = URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.href = url;
            link.download = `${cardData.firstName}_${cardData.lastName}.vcf`;
            link.click();
            
            showToast('vCard downloaded!');
        }

        function copyQRLink() {
            navigator.clipboard.writeText(window.location.href);
            showToast('Link copied to clipboard!');
        }

        function toggleTheme() {
            document.body.classList.toggle('dark');
        }

        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            document.getElementById('toast-message').textContent = message;
            toast.classList.add('show');
            setTimeout(() => toast.classList.remove('show'), 3000);
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            updateCard();
        });
    </script>
</body>
</html>
