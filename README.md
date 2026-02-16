

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fiche Bâtiment - ENEOR SIÈGE</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        acorus: {
                            dark: '#0f172a', // Bleu très sombre (Slate 900)
                            primary: '#0ea5e9', // Bleu ciel vif (Sky 500)
                            secondary: '#64748b', // Gris technique
                            bg: '#f8fafc' // Fond très clair
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body { font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; }
        .tab-content { display: none; animation: fadeIn 0.4s ease-in-out; }
        .tab-content.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .card-hover:hover { transform: translateY(-3px); box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1); }
        #map { height: 100%; width: 100%; border-radius: 0.75rem; z-index: 1; }
    </style>
</head>
<body class="bg-acorus-bg text-slate-800 h-screen flex flex-col overflow-hidden">

    <header class="bg-white border-b border-slate-200 h-16 flex items-center justify-between px-8 shadow-sm z-20">
        <div class="flex items-center gap-4">
            <div class="w-10 h-10 bg-acorus-primary rounded-lg flex items-center justify-center text-white font-bold text-xl">
                <i class="fa-solid fa-building"></i>
            </div>
            <div>
                <h1 class="text-xl font-bold text-acorus-dark">ENEOR SIÈGE</h1>
                <p class="text-xs text-acorus-secondary">54 Route de Sartrouville, 78230 LE PECQ</p>
            </div>
        </div>
        <div class="flex gap-6 text-sm">
            <div class="text-right">
                <p class="text-xs text-acorus-secondary">Surface</p>
                <p class="font-bold">2 650 m²</p>
            </div>
            <div class="text-right border-l pl-6">
                <p class="text-xs text-acorus-secondary">Performance</p>
                <p class="font-bold text-acorus-primary">67 kWh/m².an</p>
            </div>
        </div>
    </header>

    <div class="flex flex-1 overflow-hidden">
        <nav class="w-64 bg-white border-r border-slate-200 flex flex-col py-6">
            <p class="px-6 text-xs font-semibold text-slate-400 uppercase tracking-wider mb-4">Menu</p>
            
            <button onclick="switchTab('dashboard')" class="nav-btn active flex items-center gap-3 px-6 py-3 text-sm font-medium text-acorus-primary bg-sky-50 border-r-4 border-acorus-primary transition-all">
                <i class="fa-solid fa-chart-pie w-5"></i> Vue d'ensemble
            </button>
            <button onclick="switchTab('technical')" class="nav-btn flex items-center gap-3 px-6 py-3 text-sm font-medium text-slate-600 hover:bg-slate-50 hover:text-acorus-dark transition-all border-r-4 border-transparent">
                <i class="fa-solid fa-cogs w-5"></i> Technique & Bâti
            </button>
            <button onclick="switchTab('actions')" class="nav-btn flex items-center gap-3 px-6 py-3 text-sm font-medium text-slate-600 hover:bg-slate-50 hover:text-acorus-dark transition-all border-r-4 border-transparent">
                <i class="fa-solid fa-clipboard-check w-5"></i> Plan d'Actions
            </button>
            <button onclick="switchTab('location')" class="nav-btn flex items-center gap-3 px-6 py-3 text-sm font-medium text-slate-600 hover:bg-slate-50 hover:text-acorus-dark transition-all border-r-4 border-transparent">
                <i class="fa-solid fa-map-location-dot w-5"></i> Localisation
            </button>

            <div class="mt-auto px-6">
                <div class="bg-slate-100 p-4 rounded-xl text-xs text-slate-500">
                    <p class="font-bold mb-1">État Audit</p>
                    <p>Dernière màj : Fév 2026</p>
                    <div class="w-full bg-slate-300 h-1.5 rounded-full mt-2">
                        <div class="bg-green-500 h-1.5 rounded-full" style="width: 100%"></div>
                    </div>
                </div>
            </div>
        </nav>

        <main class="flex-1 overflow-y-auto p-8 relative">
            
            <div id="dashboard" class="tab-content active">
                <h2 class="text-2xl font-bold text-acorus-dark mb-6">Synthèse Energétique</h2>
                
                <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100 card-hover">
                        <div class="flex justify-between items-start mb-4">
                            <div class="p-2 bg-blue-50 rounded-lg text-acorus-primary"><i class="fa-solid fa-bolt"></i></div>
                            <span class="text-xs font-bold text-green-600 bg-green-50 px-2 py-1 rounded">+3% vs N-1</span>
                        </div>
                        <p class="text-slate-500 text-sm">Conso. Annuelle</p>
                        <h3 class="text-2xl font-bold text-slate-800">27 448 <span class="text-sm font-normal">kWh</span></h3>
                    </div>

                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100 card-hover">
                        <div class="flex justify-between items-start mb-4">
                            <div class="p-2 bg-orange-50 rounded-lg text-orange-500"><i class="fa-solid fa-euro-sign"></i></div>
                            <span class="text-xs font-bold text-red-600 bg-red-50 px-2 py-1 rounded">Hausse prix</span>
                        </div>
                        <p class="text-slate-500 text-sm">Coût unitaire 2025</p>
                        <h3 class="text-2xl font-bold text-slate-800">0.53 <span class="text-sm font-normal">€/kWh</span></h3>
                    </div>

                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100 card-hover">
                        <div class="flex justify-between items-start mb-4">
                            <div class="p-2 bg-purple-50 rounded-lg text-purple-500"><i class="fa-solid fa-people-group"></i></div>
                        </div>
                        <p class="text-slate-500 text-sm">Occupation</p>
                        <h3 class="text-2xl font-bold text-slate-800">17 <span class="text-sm font-normal">Personnes</span></h3>
                    </div>

                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100 card-hover">
                        <div class="flex justify-between items-start mb-4">
                            <div class="p-2 bg-green-50 rounded-lg text-green-600"><i class="fa-solid fa-leaf"></i></div>
                        </div>
                        <p class="text-slate-500 text-sm">Indice Carbone</p>
                        <h3 class="text-2xl font-bold text-slate-800">2.73 <span class="text-sm font-normal">kgCO2/m²</span></h3>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100">
                        <h3 class="font-bold text-acorus-dark mb-4">Répartition des usages (Estimé)</h3>
                        <div class="h-64 flex items-center justify-center">
                            <canvas id="usageChart"></canvas>
                        </div>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100">
                        <h3 class="font-bold text-acorus-dark mb-4">Signature Thermique</h3>
                        <div class="h-64 flex flex-col justify-center items-center text-slate-400">
                             <p class="mb-2"><i class="fa-solid fa-chart-line text-4xl"></i></p>
                             <p>Données DJU: 2003 (Ref: Paris Monsouris)</p>
                             <p class="text-sm">Sensibilité météo modérée constatée</p>
                        </div>
                    </div>
                </div>
            </div>

            <div id="technical" class="tab-content">
                <h2 class="text-2xl font-bold text-acorus-dark mb-6">Données Techniques du Bâtiment</h2>
                
                <div class="bg-white rounded-xl shadow-sm border border-slate-100 overflow-hidden mb-6">
                    <div class="bg-slate-50 px-6 py-4 border-b border-slate-100 flex justify-between items-center">
                        <h3 class="font-bold text-slate-700">Enveloppe & Structure</h3>
                        <span class="text-xs bg-slate-200 text-slate-600 px-2 py-1 rounded">Année constr. inconnue (Rénov > 2000)</span>
                    </div>
                    <div class="p-6 grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div>
                            <p class="text-xs text-slate-400 uppercase font-semibold">Structure</p>
                            <p class="font-medium text-slate-700">Béton</p>
                        </div>
                        <div>
                            <p class="text-xs text-slate-400 uppercase font-semibold">Vitrage</p>
                            <p class="font-medium text-slate-700">Double Vitrage</p>
                        </div>
                        <div>
                            <p class="text-xs text-slate-400 uppercase font-semibold">Isolation Murs</p>
                            <p class="font-medium text-slate-700">U = 0.30 W/m².K (Correct)</p>
                        </div>
                         <div>
                            <p class="text-xs text-slate-400 uppercase font-semibold">Inertie</p>
                            <p class="font-medium text-slate-700">Moyenne</p>
                        </div>
                         <div>
                            <p class="text-xs text-slate-400 uppercase font-semibold">Protections Solaires</p>
                            <p class="font-medium text-slate-700">Casquettes & Stores ext.</p>
                        </div>
                    </div>
                </div>

                <div class="bg-white rounded-xl shadow-sm border border-slate-100 overflow-hidden">
                    <div class="bg-slate-50 px-6 py-4 border-b border-slate-100">
                        <h3 class="font-bold text-slate-700">Systèmes CVC (Chauffage, Ventilation, Clim)</h3>
                    </div>
                    <div class="p-6">
                        <table class="w-full text-sm text-left text-slate-600">
                            <thead class="text-xs text-slate-400 uppercase bg-slate-50">
                                <tr>
                                    <th class="px-4 py-3 rounded-l-lg">Système</th>
                                    <th class="px-4 py-3">Détail</th>
                                    <th class="px-4 py-3 rounded-r-lg">État / Observation</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr class="border-b">
                                    <td class="px-4 py-3 font-medium text-slate-800">Production Chaud/Froid</td>
                                    <td class="px-4 py-3">PAC Électrique / Groupe Froid Split</td>
                                    <td class="px-4 py-3"><span class="text-orange-500"><i class="fa-solid fa-triangle-exclamation"></i> Pas de sous-comptage</span></td>
                                </tr>
                                <tr class="border-b">
                                    <td class="px-4 py-3 font-medium text-slate-800">Émission</td>
                                    <td class="px-4 py-3">Ventilo-convecteurs & Radiateurs élec.</td>
                                    <td class="px-4 py-3">Difficulté de régulation par zone</td>
                                </tr>
                                <tr class="border-b">
                                    <td class="px-4 py-3 font-medium text-slate-800">Ventilation</td>
                                    <td class="px-4 py-3">CTA Double Flux</td>
                                    <td class="px-4 py-3">Standard Tertiaire</td>
                                </tr>
                                <tr>
                                    <td class="px-4 py-3 font-medium text-slate-800">Éclairage</td>
                                    <td class="px-4 py-3">Majorité LED</td>
                                    <td class="px-4 py-3">Pilotage manuel (Source de gaspillage)</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div id="actions" class="tab-content">
                <div class="flex justify-between items-end mb-6">
                    <div>
                        <h2 class="text-2xl font-bold text-acorus-dark">Plan d'Action Prioritaire</h2>
                        <p class="text-slate-500 mt-1">Sélection des 3 actions à plus fort impact (Basé sur l'audit visuel)</p>
                    </div>
                    <button class="bg-acorus-primary text-white px-4 py-2 rounded-lg text-sm font-medium hover:bg-sky-600 transition shadow-sm">
                        <i class="fa-solid fa-download mr-2"></i>Exporter PDF
                    </button>
                </div>

                <div class="space-y-6">
                    <div class="bg-white border-l-4 border-green-500 rounded-r-xl shadow-sm p-6 flex flex-col md:flex-row gap-6 items-start transition hover:shadow-md">
                        <div class="flex-1">
                            <div class="flex items-center gap-3 mb-2">
                                <span class="bg-green-100 text-green-700 text-xs font-bold px-2 py-1 rounded uppercase">Priorité 1</span>
                                <h3 class="text-lg font-bold text-slate-800">Pilotage & Programmation (Horloges)</h3>
                            </div>
                            <p class="text-slate-600 text-sm mb-4">
                                <strong>Problème :</strong> Pics de consommation constatés hors occupation (nuits/week-ends) dus aux éclairages et convecteurs restés allumés.<br>
                                <strong>Solution :</strong> Installation d'horloges programmables pour l'éclairage et programmation horaire stricte des radiateurs (montée progressive/arrêt auto).
                            </p>
                            <div class="flex gap-4 text-sm">
                                <div class="flex items-center gap-2 text-slate-500"><i class="fa-solid fa-wallet text-slate-400"></i> Invest: Faible</div>
                                <div class="flex items-center gap-2 text-slate-500"><i class="fa-solid fa-clock text-slate-400"></i> Mise en œuvre: Immédiate</div>
                            </div>
                        </div>
                        <div class="bg-slate-50 p-4 rounded-lg min-w-[200px] border border-slate-100">
                            <p class="text-xs text-slate-400 uppercase font-bold mb-1">Gain Estimé</p>
                            <p class="text-2xl font-bold text-green-600">10 - 15%</p>
                            <p class="text-xs text-slate-500">sur la facture élec.</p>
                            <div class="mt-3 pt-3 border-t border-slate-200">
                                <p class="text-xs text-slate-400 uppercase font-bold mb-1">ROI</p>
                                <p class="font-bold text-slate-700">< 1 an</p>
                            </div>
                        </div>
                    </div>

                    <div class="bg-white border-l-4 border-blue-500 rounded-r-xl shadow-sm p-6 flex flex-col md:flex-row gap-6 items-start transition hover:shadow-md">
                        <div class="flex-1">
                            <div class="flex items-center gap-3 mb-2">
                                <span class="bg-blue-100 text-blue-700 text-xs font-bold px-2 py-1 rounded uppercase">Priorité 2</span>
                                <h3 class="text-lg font-bold text-slate-800">Mise en place Sous-Comptage</h3>
                            </div>
                            <p class="text-slate-600 text-sm mb-4">
                                <strong>Problème :</strong> Un seul PDL général. Impossible de distinguer la part CVC, Eclairage ou IT. "Impossible d'obtenir des données précises".<br>
                                <strong>Solution :</strong> Installation de compteurs en amont des départs : 1x Groupe Froid/CTA, 1x Eclairage, 1x Prises.
                            </p>
                            <div class="flex gap-4 text-sm">
                                <div class="flex items-center gap-2 text-slate-500"><i class="fa-solid fa-wallet text-slate-400"></i> Invest: Moyen</div>
                                <div class="flex items-center gap-2 text-slate-500"><i class="fa-solid fa-screwdriver-wrench text-slate-400"></i> Travaux élec. requis</div>
                            </div>
                        </div>
                        <div class="bg-slate-50 p-4 rounded-lg min-w-[200px] border border-slate-100">
                            <p class="text-xs text-slate-400 uppercase font-bold mb-1">Impact</p>
                            <p class="text-lg font-bold text-blue-600">Pilotage fin</p>
                            <p class="text-xs text-slate-500">Indispensable pour ISO 50001</p>
                            <div class="mt-3 pt-3 border-t border-slate-200">
                                <p class="text-xs text-slate-400 uppercase font-bold mb-1">ROI</p>
                                <p class="font-bold text-slate-700">Indir. (3-5 ans)</p>
                            </div>
                        </div>
                    </div>

                    <div class="bg-white border-l-4 border-yellow-500 rounded-r-xl shadow-sm p-6 flex flex-col md:flex-row gap-6 items-start transition hover:shadow-md">
                        <div class="flex-1">
                            <div class="flex items-center gap-3 mb-2">
                                <span class="bg-yellow-100 text-yellow-700 text-xs font-bold px-2 py-1 rounded uppercase">Priorité 3</span>
                                <h3 class="text-lg font-bold text-slate-800">Renégociation Contrat Énergie</h3>
                            </div>
                            <p class="text-slate-600 text-sm mb-4">
                                <strong>Problème :</strong> Hausse massive du prix unitaire (0,25€ -> 0,53€/kWh) en 3 ans.<br>
                                <strong>Solution :</strong> Consultation fournisseur pour optimiser le tarif bleu/jaune et adapter la puissance souscrite (30 kVA actuels).
                            </p>
                            <div class="flex gap-4 text-sm">
                                <div class="flex items-center gap-2 text-slate-500"><i class="fa-solid fa-wallet text-slate-400"></i> Invest: Nul</div>
                                <div class="flex items-center gap-2 text-slate-500"><i class="fa-solid fa-file-contract text-slate-400"></i> Administratif</div>
                            </div>
                        </div>
                        <div class="bg-slate-50 p-4 rounded-lg min-w-[200px] border border-slate-100">
                            <p class="text-xs text-slate-400 uppercase font-bold mb-1">Gain Financier</p>
                            <p class="text-2xl font-bold text-yellow-600">Immédiat</p>
                            <p class="text-xs text-slate-500">Sur OPEX annuel</p>
                            <div class="mt-3 pt-3 border-t border-slate-200">
                                <p class="text-xs text-slate-400 uppercase font-bold mb-1">ROI</p>
                                <p class="font-bold text-slate-700">Immédiat</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div id="location" class="tab-content h-full flex flex-col">
                <h2 class="text-2xl font-bold text-acorus-dark mb-4">Localisation du site</h2>
                <div class="flex-1 bg-white p-2 rounded-xl border border-slate-200 shadow-sm relative">
                    <div id="map"></div>
                    <div class="absolute bottom-6 left-6 bg-white p-4 rounded-lg shadow-lg z-[999] max-w-xs">
                        <h4 class="font-bold text-sm mb-1">Eneor Siège</h4>
                        <p class="text-xs text-slate-500">54 Route de Sartrouville<br>78230 LE PECQ</p>
                        <div class="mt-2 text-xs flex items-center gap-2">
                            <span class="w-2 h-2 rounded-full bg-green-500"></span> Zone Tertiaire
                        </div>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <script>
        // 1. Gestion des Onglets
        function switchTab(tabId) {
            // Masquer tout
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(el => {
                el.classList.remove('active', 'bg-sky-50', 'text-acorus-primary', 'border-acorus-primary');
                el.classList.add('text-slate-600', 'border-transparent');
            });

            // Afficher le bon
            document.getElementById(tabId).classList.add('active');
            
            // Style bouton actif
            const activeBtn = document.querySelector(`button[onclick="switchTab('${tabId}')"]`);
            activeBtn.classList.add('active', 'bg-sky-50', 'text-acorus-primary', 'border-acorus-primary');
            activeBtn.classList.remove('text-slate-600', 'border-transparent');

            // Si onglet map, redimensionner la carte (bug leaflet fréquent si caché)
            if(tabId === 'location') {
                setTimeout(() => { map.invalidateSize(); }, 100);
            }
        }

        // 2. Initialisation Carte (Leaflet)
        // Coordonnées approximatives pour 54 Rte de Sartrouville, Le Pecq (48.895, 2.115)
        var map = L.map('map').setView([48.8955, 2.1150], 16);

        L.tileLayer('https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png', {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors &copy; <a href="https://carto.com/attributions">CARTO</a>',
            subdomains: 'abcd',
            maxZoom: 20
        }).addTo(map);

        var marker = L.marker([48.8955, 2.1150]).addTo(map);
        marker.bindPopup("<b>ENEOR SIEGE</b><br>Site Audit").openPopup();

        // 3. Graphique Chart.js (Répartition estimée selon typologie tertiaire standard + données)
        const ctx = document.getElementById('usageChart').getContext('2d');
        new Chart(ctx, {
            type: 'doughnut',
            data: {
                labels: ['CVC (Chauffage/Clim)', 'Éclairage', 'ECS', 'Bureautique/Autres'],
                datasets: [{
                    data: [55, 20, 5, 20], // Estimation typique tertiaire année 80-90 rénové
                    backgroundColor: ['#0ea5e9', '#f59e0b', '#ef4444', '#64748b'],
                    borderWidth: 0
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: { position: 'right' }
                }
            }
        });
    </script>
</body>
</html>
