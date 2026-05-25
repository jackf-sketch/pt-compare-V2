# pt-compare-V2
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PTCompare Pro - Unified US Search & Provider Portal</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <style>
        .theme-accent-gradient { background: linear-gradient(135deg, #1e3a8a 0%, #065f46 100%); }
        .premium-sponsored-card { border: 2px solid #059669; background-color: #f0fdf4; }
        .map-dot { animation: pulse 2s infinite; }
        @keyframes pulse {
            0% { transform: scale(0.95); opacity: 0.5; }
            50% { transform: scale(1.1); opacity: 1; }
            100% { transform: scale(0.95); opacity: 0.5; }
        }
    </style>
</head>
<body class="bg-slate-50 min-h-screen text-slate-800 antialiased">

    <header class="bg-white shadow-sm border-b border-slate-200 sticky top-0 z-50">
        <div class="max-w-6xl mx-auto px-4 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-3">
                <div class="w-8 h-8 rounded-lg theme-accent-gradient flex items-center justify-center text-white font-black text-sm">PT</div>
                <span class="text-2xl font-black tracking-tight text-blue-900">PT<span class="text-emerald-600">Compare</span> Pro</span>
            </div>
            
            <div class="bg-slate-100 p-1 rounded-xl flex gap-1">
                <button id="viewPatientsTab" onclick="switchMainView('patients')" class="px-4 py-2 text-xs font-bold rounded-lg transition-all bg-blue-900 text-white shadow-sm cursor-pointer">
                    🔍 Patient Finder View
                </button>
                <button id="viewClinicsTab" onclick="switchMainView('clinics')" class="px-4 py-2 text-xs font-bold rounded-lg transition-all text-slate-600 hover:text-slate-900 cursor-pointer">
                    💼 For Clinic Owners ($)
                </button>
            </div>
        </div>
    </header>

    <div id="patientInterfaceContainer" class="max-w-6xl mx-auto px-4 py-8 grid grid-cols-1 lg:grid-cols-3 gap-8">
        
        <section class="lg:col-span-1 space-y-6">
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 space-y-4">
                <div>
                    <h2 class="text-base font-bold text-blue-900 flex items-center gap-2"><span>📍</span> 1. Location Lookup</h2>
                    <p class="text-xs text-slate-400">Type any US zip code, city, town, or address destination freely.</p>
                </div>
                <div>
                    <div class="relative">
                        <input id="locationSearchInput" type="text" placeholder="e.g., 98105, 35401, Austin, Miami..." value="98105" class="w-full bg-slate-50 border border-slate-200 rounded-xl pl-4 pr-12 py-3 text-xs font-semibold text-slate-700 focus:outline-none focus:ring-2 focus:ring-emerald-500 transition-all">
                        <button onclick="executeUniversalSearch()" class="absolute right-2 top-2 text-xs bg-blue-900 text-white rounded-lg px-3 py-1.5 font-bold hover:bg-blue-950 cursor-pointer">Search</button>
                    </div>
                </div>
                <div id="geoStatusText" class="text-[11px] font-bold text-blue-800 bg-blue-50 border border-blue-100 rounded-lg p-2 text-center">
                    Active Coordinates Locked: Seattle Region
                </div>
                <div>
                    <label class="block text-[10px] uppercase font-bold text-slate-400 mb-1.5">Reputation & Reviews Filter</label>
                    <select id="ratingFilter" onchange="renderApp()" class="w-full bg-slate-50 border border-slate-200 rounded-xl p-2.5 text-xs font-semibold text-slate-700 focus:outline-none focus:ring-2 focus:ring-emerald-500">
                        <option value="0">Show All Verified Locations</option>
                        <option value="4.0" selected>★ 4.0+ Stars (Top Rated Only)</option>
                        <option value="4.5">★ 4.5+ Stars (Elite Tier Only)</option>
                    </select>
                </div>
            </div>

            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 space-y-4">
                <div>
                    <h2 class="text-base font-bold text-blue-900 flex items-center gap-2"><span>⚡</span> 2. Service Profiles</h2>
                    <p class="text-xs text-slate-400">Filter clinics providing advanced treatments or recovery options.</p>
                </div>
                <div>
                    <select id="specialtyFilter" onchange="renderApp()" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2.5 text-xs font-semibold text-slate-700 focus:outline-none focus:ring-2 focus:ring-emerald-500">
                        <option value="Knee Rehab" selected>Knee Injury / ACL / Meniscus Recovery</option>
                        <option value="Back Pain">Lower Back Pain / Sciatica Management</option>
                        <option value="Shoulder Injury">Shoulder & Rotator Cuff Orthopedics</option>
                    </select>
                </div>
                <div class="space-y-2">
                    <label class="block text-[10px] uppercase font-bold text-slate-400">Require Specialized Interventions</label>
                    <div class="space-y-2">
                        <label class="flex items-center gap-2.5 text-xs font-medium text-slate-600 cursor-pointer select-none">
                            <input type="checkbox" value="Shockwave Therapy" class="modality-checkbox w-4 h-4 rounded text-emerald-600 border-slate-200 focus:ring-emerald-500 accent-emerald-600" onchange="renderApp()">
                            <span>Extracorporeal Shockwave (ESWT)</span>
                        </label>
                        <label class="flex items-center gap-2.5 text-xs font-medium text-slate-600 cursor-pointer select-none">
                            <input type="checkbox" value="Dry Needling / Acupuncture" class="modality-checkbox w-4 h-4 rounded text-emerald-600 border-slate-200 focus:ring-emerald-500 accent-emerald-600" onchange="renderApp()">
                            <span>Dry Needling / Acupuncture</span>
                        </label>
                        <label class="flex items-center gap-2.5 text-xs font-medium text-slate-600 cursor-pointer select-none">
                            <input type="checkbox" value="Cupping Therapy" class="modality-checkbox w-4 h-4 rounded text-emerald-600 border-slate-200 focus:ring-emerald-500 accent-emerald-600" onchange="renderApp()">
                            <span>Myofascial Cupping Therapy</span>
                        </label>
                        <label class="flex items-center gap-2.5 text-xs font-medium text-slate-600 cursor-pointer select-none">
                            <input type="checkbox" value="Deep Muscle Massage" class="modality-checkbox w-4 h-4 rounded text-emerald-600 border-slate-200 focus:ring-emerald-500 accent-emerald-600" onchange="renderApp()">
                            <span>Deep Tissue / Manual Massage</span>
                        </label>
                    </div>
                </div>
            </div>

            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 space-y-5">
                <div>
                    <h2 class="text-base font-bold text-blue-900 flex items-center gap-2"><span>🛡️</span> 3. Benefit Matrix</h2>
                    <p class="text-xs text-slate-400">Set active plan states to compute accurate consumer estimates.</p>
                </div>
                <div class="grid grid-cols-2 gap-2 bg-slate-100 p-1 rounded-xl">
                    <button id="btnInsurance" class="py-2 text-xs font-bold rounded-lg text-center bg-white text-blue-900 shadow-sm transition-all" onclick="setPaymentMode('insurance')">Insurance Path</button>
                    <button id="btnCash" class="py-2 text-xs font-bold rounded-lg text-center text-slate-500 hover:text-slate-900 transition-all" onclick="setPaymentMode('cash')">Self-Pay Cash</button>
                </div>
                <div id="insuranceInputs" class="space-y-4">
                    <div>
                        <select id="insuranceProvider" onchange="renderApp()" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2.5 text-xs font-medium text-slate-700 focus:outline-none focus:ring-2 focus:ring-emerald-500">
                            <option value="Blue Cross Blue Shield">Blue Cross Blue Shield</option>
                            <option value="Aetna">Aetna</option>
                            <option value="UnitedHealthcare">UnitedHealthcare</option>
                        </select>
                    </div>
                    <div>
                        <div class="flex justify-between items-center mb-1">
                            <label class="block text-[10px] uppercase font-bold text-slate-400">Remaining Deductible</label>
                            <span id="deductibleLabel" class="text-xs font-black text-emerald-600">$1,500</span>
                        </div>
                        <input id="deductibleInput" type="range" min="0" max="3000" step="250" value="1500" class="w-full h-1.5 bg-slate-200 rounded-lg appearance-none cursor-pointer accent-emerald-600">
                    </div>
                </div>
                <hr class="border-slate-100">
                <div>
                    <div class="flex justify-between items-center mb-1">
                        <label class="block text-[10px] uppercase font-bold text-slate-400">Treatment Horizon Length</label>
                        <span id="horizonLabel" class="text-xs font-black text-blue-600">12 Sessions</span>
                    </div>
                    <input id="horizonInput" type="range" min="1" max="24" step="1" value="12" class="w-full h-1.5 bg-slate-200 rounded-lg appearance-none cursor-pointer accent-blue-600">
                </div>
            </div>
        </section>

        <section class="lg:col-span-2 space-y-6">
            
            <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-100 space-y-3">
                <div class="flex justify-between items-center">
                    <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 flex items-center gap-1.5">
                        <span class="w-2 h-2 rounded-full bg-emerald-500 animate-ping"></span> Visual Proximity Mapping
                    </h3>
                    <span class="text-[11px] font-bold text-emerald-700 bg-emerald-50 px-2 py-0.5 rounded-md">Simulated Active View</span>
                </div>
                <div class="w-full h-44 rounded-xl bg-slate-100 relative overflow-hidden border border-slate-200/60 pattern-grid flex items-center justify-center">
                    <div class="absolute inset-0 opacity-20 bg-[linear-gradient(to_right,#808080_1px,transparent_1px),linear-gradient(to_bottom,#808080_1px,transparent_1px)] bg-[size:24px_24px]"></div>
                    <div class="absolute top-1/2 left-1/2 w-48 h-48 rounded-full border-2 border-emerald-500/20 bg-emerald-500/5 -translate-x-1/2 -translate-y-1/2"></div>
                    
                    <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 z-10 flex flex-col items-center">
                        <div class="w-4 h-4 rounded-full bg-blue-900 border-2 border-white shadow flex items-center justify-center"><div class="w-1.5 h-1.5 rounded-full bg-white"></div></div>
                        <span class="text-[9px] font-bold bg-blue-900 text-white px-1.5 py-0.5 rounded-md mt-1 shadow-sm">Your Location</span>
                    </div>

                    <div id="mapPinsContainer"></div>
                </div>
            </div>

            <div class="space-y-4">
                <div class="flex justify-between items-center px-1">
                    <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400">Matching US Outpatient Systems</h3>
                    <span id="resultsCounter" class="text-xs bg-blue-900 text-white font-bold px-2.5 py-0.5 rounded-full">0 Found</span>
                </div>
                <div id="directoryFeed" class="space-y-4"></div>
            </div>
        </section>
    </div>

    <div id="clinicInterfaceContainer" class="max-w-4xl mx-auto px-4 py-12 space-y-8 hidden">
        <div class="theme-accent-gradient text-white p-8 rounded-3xl shadow-md flex flex-col md:flex-row justify-between items-start md:items-center gap-6">
            <div class="space-y-2">
                <span class="bg-emerald-500/20 border border-emerald-400/30 text-emerald-300 font-bold text-xs uppercase tracking-wider px-3 py-1 rounded-full">B2B Clinic Marketplace Dashboard</span>
                <h2 class="text-2xl font-black tracking-tight">Grow Your Cash-Pay Evaluation Pipelines</h2>
                <p class="text-slate-200 text-xs max-w-xl leading-relaxed">Thousands of high-deductible local patients use PTCompare Pro to locate transparent cash rates every single day. Claim your profile, publish your advanced modalities menu, and secure premium #1 ranking.</p>
            </div>
            <button onclick="alert('Demo: Premium subscription model integrates Stripe onboarding here ($99/mo).')" class="bg-white hover:bg-slate-100 text-blue-900 font-extrabold text-sm px-6 py-3.5 rounded-xl shadow-sm transition-all whitespace-nowrap cursor-pointer">
                Unlock Premium Tier
            </button>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            <div class="bg-white border border-slate-100 p-6 rounded-2xl shadow-sm space-y-1.5">
                <span class="text-xs font-bold uppercase tracking-wider text-slate-400">Simulated App Impressions</span>
                <p class="text-3xl font-black text-blue-950">14,280</p>
                <div class="text-[11px] font-bold text-emerald-600 flex items-center gap-1"><span>▲ 22.4%</span> <span class="text-slate-400 font-medium">this month</span></div>
            </div>
            <div class="bg-white border border-slate-100 p-6 rounded-2xl shadow-sm space-y-1.5">
                <span class="text-xs font-bold uppercase tracking-wider text-slate-400">Cost Engine Comparisons</span>
                <p class="text-3xl font-black text-emerald-600">3,492</p>
                <p class="text-[10px] text-slate-400 leading-none">Patients evaluated your clinic's specialized service rates.</p>
            </div>
            <div class="bg-white border border-slate-100 p-6 rounded-2xl shadow-sm space-y-1.5 relative overflow-hidden">
                <div class="absolute top-0 right-0 bg-blue-900 text-white font-bold text-[9px] uppercase tracking-wider px-2.5 py-1 rounded-bl-xl shadow-sm">Referral Revenue</div>
                <span class="text-xs font-bold uppercase tracking-wider text-slate-400">Total Leads Dispatched</span>
                <p class="text-3xl font-black text-blue-950">112</p>
                <div class="text-[11px] font-bold text-blue-700 flex items-center gap-1"><span>💰 Earned Est: $1,680</span></div>
            </div>
        </div>

        <div class="bg-white border border-slate-100 rounded-2xl p-6 space-y-4">
            <div>
                <h3 class="text-base font-bold text-blue-950">Claim Your Clinic Physical Location Profile</h3>
                <p class="text-xs text-slate-400">Locate your business infrastructure setup profile within our national NPI registry index to sync lead alerts.</p>
            </div>
            
            <div class="divide-y divide-slate-100">
                <div class="py-4 flex justify-between items-center flex-wrap gap-4">
                    <div>
                        <h4 class="text-sm font-bold text-slate-900">Greenwood Physical Therapy - Main Facility</h4>
                        <p class="text-xs text-slate-400">📍 Seattle, WA • Status: <span class="text-amber-600 font-bold bg-amber-50 px-1.5 py-0.5 rounded text-[10px]">Unclaimed Basic Listing</span></p>
                    </div>
                    <button onclick="alert('Demo Integration: Trigger identity authorization checklist for Greenwood PT.')" class="bg-slate-900 hover:bg-slate-800 text-white font-bold text-xs py-2 px-4 rounded-xl shadow-sm cursor-pointer transition-all">Claim Asset Profile</button>
                </div>
                <div class="py-4 flex justify-between items-center flex-wrap gap-4">
                    <div>
                        <h4 class="text-sm font-bold text-slate-900">UW Medicine Sports Physical Therapy</h4>
                        <p class="text-xs text-slate-400">📍 Seattle, WA • Status: <span class="text-amber-600 font-bold bg-amber-50 px-1.5 py-0.5 rounded text-[10px]">Unclaimed Basic Listing</span></p>
                    </div>
                    <button onclick="alert('Demo Integration: Trigger identity authorization checklist for UW Medicine PT.')" class="bg-slate-900 hover:bg-slate-800 text-white font-bold text-xs py-2 px-4 rounded-xl shadow-sm cursor-pointer transition-all">Claim Asset Profile</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Active Search Hub State Variable Base
        let activeMarketplaceLocation = "seattle";
        let currentPaymentMode = 'insurance';

        // Core UI Dashboard Navigation Router
        function switchMainView(targetInterface) {
            const patientTab = document.getElementById('viewPatientsTab');
            const clinicTab = document.getElementById('viewClinicsTab');
            const patientContainer = document.getElementById('patientInterfaceContainer');
            const clinicContainer = document.getElementById('clinicInterfaceContainer');

            if (targetInterface === 'patients') {
                patientTab.className = "px-4 py-2 text-xs font-bold rounded-lg transition-all bg-blue-900 text-white shadow-sm cursor-pointer";
                clinicTab.className = "px-4 py-2 text-xs font-bold rounded-lg transition-all text-slate-600 hover:text-slate-900 cursor-pointer";
                patientContainer.classList.remove('hidden');
                clinicContainer.classList.add('hidden');
            } else {
                clinicTab.className = "px-4 py-2 text-xs font-bold rounded-lg transition-all bg-blue-900 text-white shadow-sm cursor-pointer";
                patientTab.className = "px-4 py-2 text-xs font-bold rounded-lg transition-all text-slate-600 hover:text-slate-900 cursor-pointer";
                clinicContainer.classList.remove('hidden');
                patientContainer.classList.add('hidden');
            }
        }

        // Mapping Coordinates Table for Spatial Calculations and Map Dot Adjustments
        const regionalGeocodeDatabase = {
            "98105": { name: "Seattle, WA", key: "seattle" },
            "seattle": { name: "Seattle Metro, WA", key: "seattle" },
            "35401": { name: "Tuscaloosa Core, AL", key: "tuscaloosa" },
            "tuscaloosa": { name: "Tuscaloosa Campus, AL", key: "tuscaloosa" },
            "miami": { name: "Miami Metro, FL", key: "miami" },
            "austin": { name: "Austin Metro, TX", key: "austin" },
            "nyc": { name: "New York City, NY", key: "nyc" }
        };

        // Master NPI Provider Registry with Custom Map Graph Coordinates
        const dynamicGlobalRegistry = {
            "seattle": [
                { name: "ATI Physical Therapy - University District", rating: 4.6, reviews: 142, mapX: "25%", mapY: "30%", specialties: ["Knee Rehab", "Back Pain"], modalities: ["Deep Muscle Massage", "Cupping Therapy"], networks: ["Blue Cross Blue Shield", "Aetna"], cashRate: 110, contractedRate: 195, isPremium: true, isSponsored: true, tagline: "Top-rated sports physical therapy specializing in joint manipulation and muscle therapy." },
                { name: "RET Physical Therapy & Sports Specialists", rating: 4.8, reviews: 98, mapX: "70%", mapY: "20%", specialties: ["Knee Rehab", "Shoulder Injury"], modalities: ["Dry Needling / Acupuncture", "Shockwave Therapy"], networks: ["Blue Cross Blue Shield"], cashRate: 125, contractedRate: 185, isPremium: true, isSponsored: false, tagline: "Elite orthopedic clinical facility featuring dry needling and target shockwave systems." },
                { name: "Greenwood Physical Therapy - Main Center", rating: 4.7, reviews: 210, mapX: "40%", mapY: "75%", specialties: ["Back Pain", "Knee Rehab", "Shoulder Injury"], modalities: ["Cupping Therapy", "Deep Muscle Massage"], networks: ["Blue Cross Blue Shield"], cashRate: 95, contractedRate: 170, isPremium: false, isSponsored: false, tagline: "Patient choice winner offering focused manual therapy adjustment packages." },
                { name: "UW Medicine Sports Physical Therapy", rating: 4.4, reviews: 340, mapX: "15%", mapY: "60%", specialties: ["Knee Rehab", "Shoulder Injury"], modalities: ["Deep Muscle Massage"], networks: ["Blue Cross Blue Shield", "Aetna"], cashRate: 150, contractedRate: 220, isPremium: false, isSponsored: false, tagline: "Comprehensive hospital-backed post-surgical ligament rehabilitation protocols." }
            ],
            "tuscaloosa": [
                { name: "TherapySouth - Tuscaloosa Campus West", rating: 4.9, reviews: 215, mapX: "30%", mapY: "40%", specialties: ["Knee Rehab", "Back Pain", "Shoulder Injury"], modalities: ["Dry Needling / Acupuncture", "Deep Muscle Massage", "Cupping Therapy"], networks: ["Blue Cross Blue Shield", "Aetna"], cashRate: 90, contractedRate: 160, isPremium: true, isSponsored: true, tagline: "Highly rated athlete and student facility specializing in trigger point dry needling programs." },
                { name: "ATI Physical Therapy - Tuscaloosa East", rating: 4.5, reviews: 88, mapX: "75%", mapY: "65%", specialties: ["Knee Rehab", "Back Pain"], modalities: ["Deep Muscle Massage"], networks: ["Blue Cross Blue Shield"], cashRate: 105, contractedRate: 175, isPremium: true, isSponsored: false, tagline: "Standardized physical therapy network prioritizing structured evidence-based joint rehab." }
            ],
            "miami": [
                { name: "Professional Physical Therapy - Brickell Center", rating: 4.7, reviews: 184, mapX: "50%", mapY: "25%", specialties: ["Knee Rehab", "Back Pain", "Shoulder Injury"], modalities: ["Shockwave Therapy", "Cupping Therapy"], networks: ["Aetna"], cashRate: 135, contractedRate: 215, isPremium: true, isSponsored: true, tagline: "Miami's premier recovery club featuring state-of-the-art alternative modalities." }
            ],
            "austin": [
                { name: "Airrosti Muscle & Joint Rehab - Downtown Lab", rating: 4.8, reviews: 290, mapX: "45%", mapY: "50%", specialties: ["Back Pain", "Shoulder Injury"], modalities: ["Deep Muscle Massage", "Cupping Therapy"], networks: ["Blue Cross Blue Shield", "Aetna"], cashRate: 150, contractedRate: 230, isPremium: true, isSponsored: true, tagline: "Rapid soft-tissue relief programs addressing musculoskeletal failures without surgeries." }
            ],
            "nyc": [
                { name: "Spear Physical Therapy - Grand Central Terminal", rating: 4.8, reviews: 412, mapX: "35%", mapY: "45%", specialties: ["Knee Rehab", "Back Pain", "Shoulder Injury"], modalities: ["Deep Muscle Massage", "Cupping Therapy", "Dry Needling / Acupuncture"], networks: ["Blue Cross Blue Shield"], cashRate: 155, contractedRate: 255, isPremium: true, isSponsored: false, tagline: "Award-winning clinical practice offering bespoke manual treatment and tracking loops." }
            ]
        };

        function executeUniversalSearch() {
            const rawInput = document.getElementById('locationSearchInput').value.trim().toLowerCase();
            if (regionalGeocodeDatabase[rawInput]) {
                activeMarketplaceLocation = regionalGeocodeDatabase[rawInput].key;
                document.getElementById('geoStatusText').innerText = `📌 Location Locked: ${regionalGeocodeDatabase[rawInput].name}`;
            } else {
                document.getElementById('geoStatusText').innerText = `⚠️ Search parsed. Filtering closest active matching clinics...`;
            }
            renderApp();
        }

        function detectUserLocation() {
            document.getElementById('locationSearchInput').value = "My GPS Position";
            document.getElementById('geoStatusText').innerText = `✅ GPS Satellites Engaged`;
            activeMarketplaceLocation = "seattle";
            renderApp();
        }

        function setPaymentMode(mode) {
            currentPaymentMode = mode;
            const btnIns = document.getElementById('btnInsurance');
            const btnCash = document.getElementById('btnCash');
            const block = document.getElementById('insuranceInputs');

            if (mode === 'insurance') {
                btnIns.className = "py-2 text-xs font-bold rounded-lg text-center bg-white text-blue-900 shadow-sm transition-all";
                btnCash.className = "py-2 text-xs font-bold rounded-lg text-center text-slate-500 hover:text-slate-900 transition-all";
                block.style.display = 'block';
            } else {
                btnCash.className = "py-2 text-xs font-bold rounded-lg text-center bg-white text-blue-900 shadow-sm transition-all";
                btnIns.className = "py-2 text-xs font-bold rounded-lg text-center text-slate-500 hover:text-slate-900 transition-all";
                block.style.display = 'none';
            }
            renderApp();
        }

        function executeLiveBookingReferral(clinicName) {
            alert(`📨 Automated Marketplace Hook Run Successfully!\n\nTransmission Logged:\nA high-priority diagnostic alert has been pushed straight to the front desk database channel at "${clinicName}". \n\nMonetization Audit Tracker: $15 lead-generation fee applied to affiliate clinic billing statement.`);
        }

        // Main Rendering Engine
        function renderApp() {
            const feed = document.getElementById('directoryFeed');
            const mapContainer = document.getElementById('mapPinsContainer');
            const targetSpecialty = document.getElementById('specialtyFilter').value;
            const minStarThreshold = parseFloat(document.getElementById('ratingFilter').value);
            const remainingDeductible = parseInt(document.getElementById('deductibleInput').value);
            const sessionsCount = parseInt(document.getElementById('horizonInput').value);
            const selectedPlan = document.getElementById('insuranceProvider').value;

            let requiredModalities = [];
            document.querySelectorAll('.modality-checkbox:checked').forEach(box => {
                requiredModalities.push(box.value);
            });

            let recordsPool = dynamicGlobalRegistry[activeMarketplaceLocation] || dynamicGlobalRegistry["seattle"];

            let filtered = recordsPool.filter(c => {
                const satisfiesStars = c.rating >= minStarThreshold;
                const satisfiesSpecialty = c.specialties.includes(targetSpecialty);
                const satisfiesModalities = requiredModalities.every(m => c.modalities.includes(m));
                return satisfiesStars && satisfiesSpecialty && satisfiesModalities;
            });

            filtered.sort((a, b) => (a.isSponsored && !b.isSponsored) ? -1 : 1);

            document.getElementById('resultsCounter').innerText = `${filtered.length} Localized Entries`;
            feed.innerHTML = '';
            mapContainer.innerHTML = '';

            if (filtered.length === 0) {
                feed.innerHTML = `<div class="bg-white rounded-2xl p-8 text-center border border-slate-100"><p class="text-xs text-slate-400">No matching clinics found inside this tracking region index.</p></div>`;
                return;
            }

            filtered.forEach(clinic => {
                let sessionCost = clinic.cashRate;
                let isNetworkAllowed = clinic.networks.includes(selectedPlan);
                let hasInsuranceBlocks = requiredModalities.some(m => ["Shockwave Therapy", "Cupping Therapy"].includes(m));

                if (currentPaymentMode === 'insurance' && isNetworkAllowed && !hasInsuranceBlocks) {
                    sessionCost = (remainingDeductible > 0) ? clinic.contractedRate : 35;
                }

                // Cumulative Full Plan of Care Loop Mathematics
                let totalInsuranceAccumulator = 0;
                let totalCashAccumulator = clinic.cashRate * sessionsCount;
                let activeDeductibleTracker = remainingDeductible;

                for (let i = 1; i <= sessionsCount; i++) {
                    if (!isNetworkAllowed || hasInsuranceBlocks) {
                        totalInsuranceAccumulator += clinic.cashRate;
                    } else if (activeDeductibleTracker > 0) {
                        totalInsuranceAccumulator += clinic.contractedRate;
                        activeDeductibleTracker -= clinic.contractedRate;
                        if (activeDeductibleTracker < 0) activeDeductibleTracker = 0;
                    } else {
                        totalInsuranceAccumulator += 35;
                    }
                }

                // UPGRADE 1: Generate Visual Pin Node for the Interactive Map
                let pinNode = document.createElement('div');
                pinNode.className = "absolute map-dot flex flex-col items-center z-20";
                pinNode.style.left = clinic.mapX;
                pinNode.style.top = clinic.mapY;
                pinNode.innerHTML = `
                    <div class="w-3 h-3 rounded-full ${clinic.isSponsored ? 'bg-emerald-600' : 'bg-blue-600'} border border-white shadow-md"></div>
                    <span class="text-[8px] font-black bg-white border border-slate-200 text-slate-700 px-1 rounded shadow-sm whitespace-nowrap mt-0.5">${clinic.name.split(' - ')[0]}</span>
                `;
                mapContainer.appendChild(pinNode);

                // UPGRADE 2: Calculate Visual Progress Percentages for the Cost Analysis Bars
                let maxBillingCeiling = Math.max(totalInsuranceAccumulator, totalCashAccumulator, 500);
                let insuranceBarPercent = (totalInsuranceAccumulator / maxBillingCeiling) * 100;
                let cashBarPercent = (totalCashAccumulator / maxBillingCeiling) * 100;

                // Build Clinic Directory Result Card
                let card = document.createElement('div');
                card.className = `bg-white rounded-2xl p-5 border shadow-sm flex flex-col md:flex-row justify-between gap-6 transition-all ${clinic.isSponsored ? 'premium-sponsored-card ring-2 ring-emerald-100' : 'border-slate-100'}`;

                let badgeHTML = clinic.isSponsored ? 
                    `<span class="text-[9px] uppercase font-black tracking-wider bg-emerald-600 text-white px-2 py-0.5 rounded-md">Featured Partner</span>` : 
                    `<span class="text-[9px] uppercase font-black tracking-wider bg-slate-100 text-slate-500 px-2 py-0.5 rounded-md">Verified Facility</span>`;

                let modsPillsHTML = clinic.modalities.map(m => `<span class="text-[10px] font-bold px-2 py-0.5 rounded-md bg-slate-100 text-slate-600">${m}</span>`).join(' ');

                card.innerHTML = `
                    <div class="flex-1 space-y-3">
                        <div class="flex items-center gap-2 flex-wrap">
                            ${badgeHTML}
                            <div class="flex items-center gap-0.5 text-xs text-amber-500 font-bold"><span>★</span><span>${clinic.rating.toFixed(1)}</span></div>
                            <span class="text-xs text-slate-400 font-bold">📍 Zip ${clinic.zip}</span>
                        </div>
                        <div>
                            <h4 class="text-lg font-black text-blue-950 tracking-tight">${clinic.name}</h4>
                            <p class="text-xs text-slate-500 italic">${clinic.tagline}</p>
                        </div>
                        <div class="flex flex-wrap gap-1">${modsPillsHTML}</div>
                        
                        <div class="bg-slate-50 p-3 rounded-xl border border-slate-200/50 space-y-2 mt-2">
                            <span class="text-[9px] font-bold uppercase tracking-wider text-slate-400">Total Treatment Cost Forecast Breakdown</span>
                            <div class="space-y-1.5">
                                <div class="space-y-0.5">
                                    <div class="flex justify-between text-[10px] font-bold text-slate-600"><span>Insurance Plan Pipeline</span><span>$${totalInsuranceAccumulator.toLocaleString()}</span></div>
                                    <div class="w-full bg-slate-200 h-2 rounded-full overflow-hidden"><div class="bg-blue-900 h-full transition-all duration-500" style="width: ${insuranceBarPercent}%"></div></div>
                                </div>
                                <div class="space-y-0.5">
                                    <div class="flex justify-between text-[10px] font-bold text-slate-600"><span>Self-Pay Flat Cash Package</span><span>$${totalCashAccumulator.toLocaleString()}</span></div>
                                    <div class="w-full bg-slate-200 h-2 rounded-full overflow-hidden"><div class="bg-emerald-600 h-full transition-all duration-500" style="width: ${cashBarPercent}%"></div></div>
                                </div>
                            </div>
                            <p class="text-[10px] text-slate-400 pt-0.5">${totalCashAccumulator < totalInsuranceAccumulator ? '💡 <strong>Saves Money:</strong> Self-paying cash saves you significant out-of-pocket costs over your current deductible phase.' : '🛡️ Using your insurance contracts results in the lowest out-of-pocket expenses for this package.'}</p>
                        </div>
                    </div>

                    <div class="md:w-56 flex flex-col justify-between items-stretch md:items-end border-t md:border-t-0 md:border-l border-slate-100 pt-4 md:pt-0 md:pl-5 min-w-[224px]">
                        <div class="text-left md:text-right">
                            <div class="flex items-baseline md:justify-end gap-0.5">
                                <span class="text-2xl font-black text-slate-900">$${sessionCost}</span>
                                <span class="text-xs text-slate-400 font-bold">/visit</span>
                            </div>
                            <p class="text-xs font-black text-emerald-600">${currentPaymentMode === 'cash' ? 'Flat Cash Rate' : 'Estimated Visit Cost'}</p>
                        </div>

                        ${clinic.isPremium ? 
                            `<button onclick="executeLiveBookingReferral('${clinic.name}')" class="w-full bg-blue-900 hover:bg-blue-950 text-white text-xs font-bold py-2.5 px-4 rounded-xl shadow-sm transition-all cursor-pointer text-center mt-4">Request Diagnostics Appt</button>` : 
                            `<button class="w-full bg-white border border-slate-200 text-slate-400 text-xs font-bold py-2.5 px-4 rounded-xl cursor-not-allowed text-center mt-4" disabled>Unclaimed Profile</button>`
                        }
                    </div>
                `;
                feed.appendChild(card);
            });
        }

        // Control Event Rigging Hook Listeners
        document.getElementById('horizonInput').addEventListener('input', (e) => {
            document.getElementById('horizonLabel').innerText = `${e.target.value} Sessions`;
            renderApp();
        });
        document.getElementById('deductibleInput').addEventListener('input', (e) => {
            document.getElementById('deductibleLabel').innerText = `$${parseInt(e.target.value).toLocaleString()}`;
            renderApp();
        });
        document.getElementById('locationSearchInput').addEventListener('keypress', (e) => {
            if (e.key === 'Enter') executeUniversalSearch();
        });

        // Bootstrap Core Loop On Load Execution
        renderApp();
    </script>
</body>
</html>
