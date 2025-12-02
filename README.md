<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hub RSE & Logistique | ADR Solutions</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    
    <!-- Chosen Palette: Teal/Cyan grounded in Warm Neutrals (inspired by ADR Solutions logo) -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            light: '#ccfbf1', // Teal 100
                            DEFAULT: '#14b8a6', // Teal 500
                            dark: '#0f766e', // Teal 700
                            accent: '#f59e0b', // Amber 500 (Subtle accent)
                        },
                        neutral: {
                            bg: '#f8fafc', // Slate 50
                            card: '#ffffff',
                            text: '#334155', // Slate 700
                        }
                    }
                }
            }
        }
    </script>

    <style>
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }

        /* Chart Container Utilities */
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px; /* Prevent excessive width */
            margin-left: auto;
            margin-right: auto;
            height: 300px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }

        /* Timeline Styling */
        .timeline-line {
            position: absolute;
            left: 1rem;
            top: 0;
            bottom: 0;
            width: 2px;
            background-color: #e2e8f0;
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .active-nav {
            background-color: #ccfbf1;
            color: #0f766e;
            border-left: 4px solid #0f766e;
        }
    </style>

    <!-- Application Structure Plan: 
         Designed as a Dashboard SPA. 
         Left Sidebar (collapsible on mobile) handles navigation between 5 distinct modules.
         Main Content Area updates dynamically using JS without page reloads.
         Rationale: The course material covers distinct but related topics (Legal, Norms, Ops, Strategy). 
         A tabbed/dashboard approach allows the user to focus on one "pillar" of knowledge at a time 
         while maintaining context.
    -->

    <!-- Visualization & Content Choices:
         1. Timeline (HTML/CSS): To show the evolution of Environmental Law (Stockholm -> Paris).
         2. Doughnut Chart (Chart.js): To visualize the PDCA cycle (ISO 14001).
         3. Scatter Plot (Chart.js): To recreate the Materiality Matrix found in the source, allowing users to see how issues are prioritized.
         4. Interactive Cards: To distinguish between Norms, Labels, and Certifications without text walls.
         5. Progress/Status Meters: For ICPE regimes.
         Confirmation: NO SVG graphics used. NO Mermaid JS used.
    -->
</head>
<body class="bg-neutral-bg text-neutral-text font-sans h-screen flex overflow-hidden">

    <!-- Sidebar Navigation -->
    <aside class="w-64 bg-white border-r border-slate-200 hidden md:flex flex-col z-10 shadow-lg">
        <div class="p-6 border-b border-slate-100 flex items-center justify-center flex-col">
            <h1 class="text-2xl font-bold text-brand-dark tracking-tight">ADR Learning</h1>
            <p class="text-xs text-slate-400 mt-1 uppercase tracking-wider">Logistique & RSE</p>
        </div>
        
        <nav class="flex-1 overflow-y-auto py-4">
            <ul class="space-y-1">
                <li>
                    <button onclick="switchTab('hub')" id="nav-hub" class="w-full text-left px-6 py-3 hover:bg-slate-50 transition-colors flex items-center active-nav">
                        <span class="mr-3 text-lg">🏠</span>
                        <span class="font-medium">Hub Central</span>
                    </button>
                </li>
                <li>
                    <button onclick="switchTab('legal')" id="nav-legal" class="w-full text-left px-6 py-3 hover:bg-slate-50 transition-colors flex items-center">
                        <span class="mr-3 text-lg">⚖️</span>
                        <span class="font-medium">Cadre Juridique</span>
                    </button>
                </li>
                <li>
                    <button onclick="switchTab('normes')" id="nav-normes" class="w-full text-left px-6 py-3 hover:bg-slate-50 transition-colors flex items-center">
                        <span class="mr-3 text-lg">🏅</span>
                        <span class="font-medium">Normes & Labels</span>
                    </button>
                </li>
                <li>
                    <button onclick="switchTab('ops')" id="nav-ops" class="w-full text-left px-6 py-3 hover:bg-slate-50 transition-colors flex items-center">
                        <span class="mr-3 text-lg">🏭</span>
                        <span class="font-medium">ICPE & REP</span>
                    </button>
                </li>
                <li>
                    <button onclick="switchTab('rse')" id="nav-rse" class="w-full text-left px-6 py-3 hover:bg-slate-50 transition-colors flex items-center">
                        <span class="mr-3 text-lg">🌱</span>
                        <span class="font-medium">Stratégie RSE</span>
                    </button>
                </li>
            </ul>
        </nav>

        <div class="p-4 border-t border-slate-100 bg-slate-50">
            <div class="text-xs text-slate-500 text-center">
                Basé sur le cours de<br>
                <strong>A.L. Deligné-Rochaud</strong><br>
                2025
            </div>
        </div>
    </aside>

    <!-- Mobile Header -->
    <div class="md:hidden absolute top-0 left-0 right-0 h-16 bg-white border-b border-slate-200 flex items-center justify-between px-4 z-20">
        <h1 class="text-xl font-bold text-brand-dark">ADR Learning</h1>
        <button id="mobile-menu-btn" class="text-slate-600 focus:outline-none text-2xl">☰</button>
    </div>

    <!-- Mobile Menu Overlay -->
    <div id="mobile-menu" class="fixed inset-0 bg-white z-30 transform translate-x-full transition-transform duration-300 md:hidden flex flex-col pt-20">
        <button onclick="switchTab('hub'); toggleMobileMenu()" class="px-6 py-4 border-b text-lg font-medium">🏠 Hub Central</button>
        <button onclick="switchTab('legal'); toggleMobileMenu()" class="px-6 py-4 border-b text-lg font-medium">⚖️ Cadre Juridique</button>
        <button onclick="switchTab('normes'); toggleMobileMenu()" class="px-6 py-4 border-b text-lg font-medium">🏅 Normes & Labels</button>
        <button onclick="switchTab('ops'); toggleMobileMenu()" class="px-6 py-4 border-b text-lg font-medium">🏭 ICPE & REP</button>
        <button onclick="switchTab('rse'); toggleMobileMenu()" class="px-6 py-4 border-b text-lg font-medium">🌱 Stratégie RSE</button>
        <button onclick="toggleMobileMenu()" class="mt-auto mb-8 mx-6 py-3 bg-slate-100 rounded text-center">Fermer</button>
    </div>

    <!-- Main Content Area -->
    <main class="flex-1 overflow-y-auto pt-20 md:pt-0 bg-slate-50 relative" id="main-content">
        
        <!-- MODULE: HUB CENTRAL -->
        <section id="tab-hub" class="p-6 md:p-10 max-w-7xl mx-auto fade-in">
            <header class="mb-8">
                <h2 class="text-3xl font-bold text-slate-800 mb-2">Bienvenue sur le Hub RSE</h2>
                <p class="text-slate-600 text-lg">Introduction à la performance environnementale dans la Logistique et le Transport.</p>
            </header>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100 border-l-4 border-brand">
                    <h3 class="font-semibold text-slate-500 mb-1 text-sm uppercase">Contexte</h3>
                    <p class="text-2xl font-bold text-slate-800">Anthropocène</p>
                    <p class="text-sm text-slate-500 mt-2">L'activité humaine transforme le système terrestre. Nécessité d'adapter les modèles économiques.</p>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100 border-l-4 border-brand-accent">
                    <h3 class="font-semibold text-slate-500 mb-1 text-sm uppercase">Outil Clé</h3>
                    <p class="text-2xl font-bold text-slate-800">ISO 14001</p>
                    <p class="text-sm text-slate-500 mt-2">Le standard international pour le système de management environnemental (SME).</p>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100 border-l-4 border-brand-dark">
                    <h3 class="font-semibold text-slate-500 mb-1 text-sm uppercase">Objectif</h3>
                    <p class="text-2xl font-bold text-slate-800">Durabilité</p>
                    <p class="text-sm text-slate-500 mt-2">Concilier Économie, Social et Environnement (Les 3 piliers).</p>
                </div>
            </div>

            <div class="bg-white rounded-xl shadow-sm border border-slate-100 p-8">
                <h3 class="text-xl font-bold text-slate-800 mb-4">Pourquoi ce module ?</h3>
                <p class="text-slate-600 mb-4 leading-relaxed">
                    Ce hub synthétise les cours sur les normes, labels et réglementations environnementales (ICPE, REP) appliqués à la logistique. 
                    L'objectif est de maîtriser les outils juridiques et volontaires permettant d'intégrer la RSE au cœur de la stratégie d'entreprise.
                </p>
                <div class="flex flex-wrap gap-2 mt-4">
                    <span class="px-3 py-1 bg-slate-100 text-slate-600 rounded-full text-sm font-medium">#DéveloppementDurable</span>
                    <span class="px-3 py-1 bg-slate-100 text-slate-600 rounded-full text-sm font-medium">#Logistique</span>
                    <span class="px-3 py-1 bg-slate-100 text-slate-600 rounded-full text-sm font-medium">#Conformité</span>
                </div>
            </div>
        </section>

        <!-- MODULE: CADRE JURIDIQUE -->
        <section id="tab-legal" class="p-6 md:p-10 max-w-7xl mx-auto hidden fade-in">
            <header class="mb-8">
                <h2 class="text-3xl font-bold text-slate-800 mb-2">Le Cadre Juridique</h2>
                <p class="text-slate-600">De la prise de conscience internationale à la législation française.</p>
            </header>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <!-- Timeline Section -->
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100">
                    <h3 class="text-xl font-bold text-slate-800 mb-6 flex items-center">
                        <span class="bg-brand-light text-brand-dark rounded p-1 mr-2 text-sm">HISTOIRE</span>
                        Évolution du Droit
                    </h3>
                    
                    <div class="relative pl-8 space-y-8">
                        <div class="timeline-line bg-brand-light"></div>
                        
                        <!-- Timeline Items -->
                        <div class="relative">
                            <div class="absolute -left-10 mt-1.5 w-4 h-4 rounded-full bg-brand border-4 border-white shadow"></div>
                            <span class="text-sm font-bold text-brand-dark">1972</span>
                            <h4 class="text-lg font-bold text-slate-700">Stockholm & Club de Rome</h4>
                            <p class="text-sm text-slate-500">1er Sommet de la Terre. Rapport Meadows "Halte à la croissance".</p>
                        </div>

                        <div class="relative">
                            <div class="absolute -left-10 mt-1.5 w-4 h-4 rounded-full bg-slate-300 border-4 border-white shadow"></div>
                            <span class="text-sm font-bold text-brand-dark">1987</span>
                            <h4 class="text-lg font-bold text-slate-700">Rapport Brundtland</h4>
                            <p class="text-sm text-slate-500">Définition officielle du "Développement Durable".</p>
                        </div>

                        <div class="relative">
                            <div class="absolute -left-10 mt-1.5 w-4 h-4 rounded-full bg-slate-300 border-4 border-white shadow"></div>
                            <span class="text-sm font-bold text-brand-dark">1992</span>
                            <h4 class="text-lg font-bold text-slate-700">Sommet de Rio</h4>
                            <p class="text-sm text-slate-500">Agenda 21. Émergence de la société civile internationale.</p>
                        </div>

                        <div class="relative">
                            <div class="absolute -left-10 mt-1.5 w-4 h-4 rounded-full bg-brand border-4 border-white shadow"></div>
                            <span class="text-sm font-bold text-brand-dark">2004 (France)</span>
                            <h4 class="text-lg font-bold text-slate-700">Charte de l'Environnement</h4>
                            <p class="text-sm text-slate-500">Intégration de l'écologie dans la Constitution française.</p>
                        </div>

                        <div class="relative">
                            <div class="absolute -left-10 mt-1.5 w-4 h-4 rounded-full bg-slate-300 border-4 border-white shadow"></div>
                            <span class="text-sm font-bold text-brand-dark">2015</span>
                            <h4 class="text-lg font-bold text-slate-700">Accord de Paris</h4>
                            <p class="text-sm text-slate-500">Limiter le réchauffement climatique.</p>
                        </div>
                    </div>
                </div>

                <!-- Hierarchy Section -->
                <div class="space-y-6">
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100">
                        <h3 class="text-xl font-bold text-slate-800 mb-4">La Pyramide des Normes (Kelsen)</h3>
                        <p class="text-sm text-slate-500 mb-4">La hiérarchie juridique en France assure la conformité des textes inférieurs aux textes supérieurs.</p>
                        
                        <div class="flex flex-col space-y-2">
                            <div class="bg-brand-dark text-white p-3 rounded-lg text-center font-bold shadow-md">1. Constitutionnel (Charte 2004)</div>
                            <div class="bg-brand text-white p-3 rounded-lg text-center font-semibold shadow-md mx-4">2. International (Traités, UE)</div>
                            <div class="bg-brand-light text-brand-dark p-3 rounded-lg text-center font-medium shadow-md mx-8">3. Législatif (Lois, Code Env.)</div>
                            <div class="bg-slate-100 text-slate-600 p-3 rounded-lg text-center text-sm shadow-md mx-12">4. Réglementaire (Décrets, Arrêtés)</div>
                        </div>
                    </div>

                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100">
                        <h3 class="text-xl font-bold text-slate-800 mb-2">Principes Clés</h3>
                        <ul class="space-y-3 mt-4">
                            <li class="flex items-start">
                                <span class="text-green-500 mr-2">✓</span>
                                <div>
                                    <strong class="text-slate-700">Prévention</strong>
                                    <p class="text-xs text-slate-500">Risque connu. Agir pour éviter les dommages.</p>
                                </div>
                            </li>
                            <li class="flex items-start">
                                <span class="text-orange-500 mr-2">!</span>
                                <div>
                                    <strong class="text-slate-700">Précaution</strong>
                                    <p class="text-xs text-slate-500">Risque incertain. Mesures provisoires en cas de doute scientifique.</p>
                                </div>
                            </li>
                            <li class="flex items-start">
                                <span class="text-blue-500 mr-2">$</span>
                                <div>
                                    <strong class="text-slate-700">Pollueur-Payeur</strong>
                                    <p class="text-xs text-slate-500">Les frais de prévention et réparation incombent au responsable.</p>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>

        <!-- MODULE: NORMES & LABELS -->
        <section id="tab-normes" class="p-6 md:p-10 max-w-7xl mx-auto hidden fade-in">
            <header class="mb-8">
                <h2 class="text-3xl font-bold text-slate-800 mb-2">Normes & Labels</h2>
                <p class="text-slate-600">Outils volontaires pour structurer la démarche environnementale.</p>
            </header>

            <!-- Definitions Cards -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                <div class="bg-white p-6 rounded-xl shadow-sm border-t-4 border-blue-500">
                    <h3 class="text-lg font-bold text-slate-800">Norme</h3>
                    <p class="text-sm text-slate-500 mt-2">Document de référence (technique/qualité). Ex: ISO 14001.</p>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-sm border-t-4 border-purple-500">
                    <h3 class="text-lg font-bold text-slate-800">Certification</h3>
                    <p class="text-sm text-slate-500 mt-2">Attestation officielle par un tiers indépendant. Ex: Certification ISO.</p>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-sm border-t-4 border-green-500">
                    <h3 class="text-lg font-bold text-slate-800">Label</h3>
                    <p class="text-sm text-slate-500 mt-2">Signe distinctif mettant en valeur un engagement. Ex: Objectif CO2.</p>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <!-- ISO 14001 Visualization -->
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100">
                    <h3 class="text-xl font-bold text-slate-800 mb-2">ISO 14001 : Amélioration Continue</h3>
                    <p class="text-sm text-slate-500 mb-6">Le Système de Management Environnemental (SME) repose sur le cycle PDCA (Roue de Deming).</p>
                    
                    <div class="chart-container flex justify-center items-center">
                        <canvas id="pdcaChart"></canvas>
                    </div>
                    <div class="mt-4 text-center text-xs text-slate-400">Cliquez sur les segments pour plus de détails (simulé)</div>
                </div>

                <!-- Labels Logistique -->
                <div class="space-y-6">
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="text-xl font-bold text-slate-800">Label Objectif CO2</h3>
                            <span class="bg-green-100 text-green-800 text-xs px-2 py-1 rounded-full">Transport Routier</span>
                        </div>
                        <p class="text-slate-600 text-sm mb-4">Démarche volontaire pour réduire les émissions de GES.</p>
                        <ol class="list-decimal list-inside space-y-2 text-sm text-slate-700">
                            <li><strong>Demande</strong> : Inscription sur la plateforme.</li>
                            <li><strong>Audit</strong> : Vérification des données (conso, tonnes, km).</li>
                            <li><strong>Label</strong> : Délivrance pour 3 ans.</li>
                        </ol>
                    </div>

                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="text-xl font-bold text-slate-800">Label 6PL</h3>
                            <span class="bg-blue-100 text-blue-800 text-xs px-2 py-1 rounded-full">Entrepôts</span>
                        </div>
                        <p class="text-slate-600 text-sm mb-4">Performance logistique durable pour les sites d'entreposage.</p>
                        <div class="grid grid-cols-2 gap-2 text-xs">
                            <div class="bg-slate-50 p-2 rounded text-center">Gouvernance</div>
                            <div class="bg-slate-50 p-2 rounded text-center">Social / Santé</div>
                            <div class="bg-slate-50 p-2 rounded text-center">Environnement</div>
                            <div class="bg-slate-50 p-2 rounded text-center">Énergie & Sécurité</div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- MODULE: OPERATIONAL (ICPE & REP) -->
        <section id="tab-ops" class="p-6 md:p-10 max-w-7xl mx-auto hidden fade-in">
            <header class="mb-8">
                <h2 class="text-3xl font-bold text-slate-800 mb-2">Opérations : ICPE & REP</h2>
                <p class="text-slate-600">Contraintes réglementaires majeures pour les sites logistiques et la gestion des produits.</p>
            </header>

            <!-- ICPE Section -->
            <div class="mb-10">
                <h3 class="text-2xl font-bold text-slate-800 mb-6 border-l-4 border-brand pl-3">ICPE : Installations Classées</h3>
                <p class="text-slate-600 mb-6">Installations susceptibles de créer des risques ou nuisances. Classement selon la gravité des dangers (ex: volume de stockage de papier/carton).</p>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <!-- Declaration -->
                    <div class="bg-green-50 p-6 rounded-xl border border-green-200">
                        <div class="flex items-center mb-3">
                            <div class="w-8 h-8 rounded-full bg-green-500 text-white flex items-center justify-center font-bold mr-3">D</div>
                            <h4 class="font-bold text-green-800">Déclaration</h4>
                        </div>
                        <p class="text-sm text-green-700">Risque faible. Simple dossier administratif et respect de prescriptions générales.</p>
                    </div>
                    <!-- Enregistrement -->
                    <div class="bg-yellow-50 p-6 rounded-xl border border-yellow-200">
                        <div class="flex items-center mb-3">
                            <div class="w-8 h-8 rounded-full bg-yellow-500 text-white flex items-center justify-center font-bold mr-3">E</div>
                            <h4 class="font-bold text-yellow-800">Enregistrement</h4>
                        </div>
                        <p class="text-sm text-yellow-700">Risque intermédiaire. Procédure simplifiée (autorisation simplifiée) avec consultation publique limitée.</p>
                    </div>
                    <!-- Autorisation -->
                    <div class="bg-red-50 p-6 rounded-xl border border-red-200">
                        <div class="flex items-center mb-3">
                            <div class="w-8 h-8 rounded-full bg-red-500 text-white flex items-center justify-center font-bold mr-3">A</div>
                            <h4 class="font-bold text-red-800">Autorisation</h4>
                        </div>
                        <p class="text-sm text-red-700">Risque fort. Étude d'impact, enquête publique et arrêté préfectoral obligatoires. (Inclus SEVESO).</p>
                    </div>
                </div>
            </div>

            <!-- REP Section -->
            <div>
                <h3 class="text-2xl font-bold text-slate-800 mb-6 border-l-4 border-brand-accent pl-3">REP : Responsabilité Élargie du Producteur</h3>
                
                <div class="bg-white p-8 rounded-xl shadow-sm border border-slate-100 flex flex-col md:flex-row gap-8 items-center">
                    <div class="flex-1">
                        <h4 class="text-lg font-bold mb-3">Le Principe Pollueur-Payeur</h4>
                        <p class="text-slate-600 mb-4 text-sm leading-relaxed">
                            Le producteur (metteur sur le marché) est responsable de la fin de vie de son produit.
                            Il doit financer ou organiser la collecte et le traitement des déchets.
                        </p>
                        <h4 class="text-lg font-bold mb-3 mt-6">Loi AGEC (Anti-Gaspillage 2020)</h4>
                        <p class="text-slate-600 mb-4 text-sm leading-relaxed">
                            Objectif : sortir du "tout jetable". Favoriser l'éco-conception, la réparation et le réemploi.
                            Création de nouvelles filières (Jouets, Bâtiment, etc.).
                        </p>
                    </div>
                    
                    <div class="w-full md:w-1/3 bg-slate-50 p-6 rounded-lg text-center">
                        <div class="text-4xl mb-2">♻️</div>
                        <h5 class="font-bold text-slate-800 mb-2">Éco-organismes</h5>
                        <p class="text-xs text-slate-500 mb-4">Structures agréées (ex: Citeo, Ecosystem) qui gèrent la collecte pour le compte des producteurs.</p>
                        <div class="border-t border-slate-200 pt-4 mt-4">
                            <div class="text-2xl font-bold text-brand">72%</div>
                            <div class="text-xs text-slate-500">Taux recyclage emballages ménagers (2021)</div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- MODULE: STRATEGIE RSE -->
        <section id="tab-rse" class="p-6 md:p-10 max-w-7xl mx-auto hidden fade-in">
            <header class="mb-8">
                <h2 class="text-3xl font-bold text-slate-800 mb-2">Stratégie RSE</h2>
                <p class="text-slate-600">Intégrer le Développement Durable dans la stratégie d'entreprise (ISO 26000).</p>
            </header>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-8">
                <!-- Materiality Matrix -->
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100 col-span-1 lg:col-span-2">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-xl font-bold text-slate-800">Matrice de Matérialité</h3>
                        <div class="text-xs text-slate-500 bg-slate-100 px-2 py-1 rounded">Outil d'aide à la décision</div>
                    </div>
                    <p class="text-sm text-slate-500 mb-6">Hiérarchiser les enjeux selon leur importance pour l'entreprise (Interne) et pour les parties prenantes (Externe).</p>
                    
                    <div class="chart-container">
                        <canvas id="matrixChart"></canvas>
                    </div>
                    <div class="mt-4 flex justify-center gap-4 text-xs text-slate-500">
                        <span class="flex items-center"><span class="w-2 h-2 rounded-full bg-green-500 mr-1"></span> Environnement</span>
                        <span class="flex items-center"><span class="w-2 h-2 rounded-full bg-blue-500 mr-1"></span> Social</span>
                        <span class="flex items-center"><span class="w-2 h-2 rounded-full bg-orange-500 mr-1"></span> Économique</span>
                    </div>
                </div>

                <!-- 3 Pillars -->
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100">
                    <h3 class="text-xl font-bold text-slate-800 mb-4">Les 3 Piliers du DD</h3>
                    <div class="flex flex-col space-y-4">
                        <div class="flex items-center p-3 bg-green-50 rounded-lg">
                            <div class="w-10 h-10 rounded-full bg-green-100 text-green-600 flex items-center justify-center text-xl mr-4">🌍</div>
                            <div>
                                <h4 class="font-bold text-green-800">Environnement</h4>
                                <p class="text-xs text-slate-600">Préserver les ressources et la biodiversité.</p>
                            </div>
                        </div>
                        <div class="flex items-center p-3 bg-blue-50 rounded-lg">
                            <div class="w-10 h-10 rounded-full bg-blue-100 text-blue-600 flex items-center justify-center text-xl mr-4">👥</div>
                            <div>
                                <h4 class="font-bold text-blue-800">Social</h4>
                                <p class="text-xs text-slate-600">Équité, santé, éducation, besoins humains.</p>
                            </div>
                        </div>
                        <div class="flex items-center p-3 bg-orange-50 rounded-lg">
                            <div class="w-10 h-10 rounded-full bg-orange-100 text-orange-600 flex items-center justify-center text-xl mr-4">💰</div>
                            <div>
                                <h4 class="font-bold text-orange-800">Économique</h4>
                                <p class="text-xs text-slate-600">Création de richesse, viabilité.</p>
                            </div>
                        </div>
                    </div>
                    <div class="mt-4 p-4 bg-slate-50 rounded text-center text-sm font-medium text-slate-600">
                        Intersection = DURABLE
                    </div>
                </div>

                <!-- ISO 26000 -->
                <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-100">
                    <h3 class="text-xl font-bold text-slate-800 mb-4">ISO 26000 : 7 Questions Centrales</h3>
                    <p class="text-sm text-slate-500 mb-4">Le guide pour la RSE (non certifiable).</p>
                    <ul class="text-sm space-y-2">
                        <li class="pl-2 border-l-2 border-brand">Gouvernance de l'organisation</li>
                        <li class="pl-2 border-l-2 border-brand">Droits de l'Homme</li>
                        <li class="pl-2 border-l-2 border-brand">Relations et conditions de travail</li>
                        <li class="pl-2 border-l-2 border-brand">L'environnement</li>
                        <li class="pl-2 border-l-2 border-brand">Loyauté des pratiques</li>
                        <li class="pl-2 border-l-2 border-brand">Questions relatives aux consommateurs</li>
                        <li class="pl-2 border-l-2 border-brand">Communautés et développement local</li>
                    </ul>
                </div>
            </div>
        </section>

    </main>

    <!-- JavaScript Logic -->
    <script>
        // --- State Management ---
        let currentTab = 'hub';

        // --- Navigation Logic ---
        function switchTab(tabId) {
            // Update State
            currentTab = tabId;

            // Hide all sections
            const sections = document.querySelectorAll('main > section');
            sections.forEach(sec => sec.classList.add('hidden'));

            // Show target section
            const target = document.getElementById(`tab-${tabId}`);
            if(target) target.classList.remove('hidden');

            // Update Nav Styles (Sidebar)
            const navButtons = document.querySelectorAll('aside nav button');
            navButtons.forEach(btn => {
                btn.classList.remove('active-nav');
                if(btn.id === `nav-${tabId}`) btn.classList.add('active-nav');
            });
            
            // Scroll to top
            document.getElementById('main-content').scrollTop = 0;
        }

        function toggleMobileMenu() {
            const menu = document.getElementById('mobile-menu');
            if (menu.classList.contains('translate-x-full')) {
                menu.classList.remove('translate-x-full');
            } else {
                menu.classList.add('translate-x-full');
            }
        }

        document.getElementById('mobile-menu-btn').addEventListener('click', toggleMobileMenu);

        // --- Charts Initialization ---
        document.addEventListener('DOMContentLoaded', () => {
            initPDCAChart();
            initMatrixChart();
        });

        function initPDCAChart() {
            const ctx = document.getElementById('pdcaChart').getContext('2d');
            new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['Plan (Planifier)', 'Do (Réaliser)', 'Check (Vérifier)', 'Act (Améliorer)'],
                    datasets: [{
                        data: [25, 25, 25, 25],
                        backgroundColor: [
                            '#fcd34d', // Amber (Plan)
                            '#f87171', // Red (Do)
                            '#60a5fa', // Blue (Check)
                            '#34d399'  // Emerald (Act) - using brand related colors
                        ],
                        borderWidth: 0,
                        hoverOffset: 10
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: { font: { family: 'Inter', size: 12 } }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    const details = [
                                        'Identifier les impacts et objectifs',
                                        'Mettre en œuvre les processus',
                                        'Surveiller et mesurer',
                                        'Actions correctives et revue'
                                    ];
                                    return details[context.dataIndex];
                                }
                            }
                        }
                    }
                }
            });
        }

        function initMatrixChart() {
            const ctx = document.getElementById('matrixChart').getContext('2d');
            
            // Data extracted from PDF "Normes-Labels-Enviro-n2.pdf" page 41
            // X: Importance Interne (1-5), Y: Importance Parties Prenantes (1-5)
            const dataEnv = [
                {x: 2, y: 3, label: 'A: Changement climatique'},
                {x: 2, y: 4.5, label: 'B: Mngt Environnemental'},
                {x: 3, y: 4, label: 'C: Pollution/Déchets'},
                {x: 2, y: 3.5, label: 'D: Ressources'},
                {x: 5, y: 4.5, label: 'E: Descendance (?)'},
                {x: 4, y: 3.2, label: 'F: Conso Énergie'}
            ];
            
            const dataSoc = [
                {x: 2, y: 5.8, label: 'H: Parité'},
                {x: 2, y: 5, label: 'I: QVT'},
                {x: 1, y: 6, label: 'J: Handicap'},
                {x: 1, y: 4.5, label: 'K: Non-discrimination'},
                {x: 3, y: 5, label: 'L: Promotion Salariés'},
                {x: 4, y: 6, label: 'M: Rémunération'}
            ];

            const dataEco = [
                 {x: 5, y: 5.8, label: 'S: Gestion Risques'},
                 {x: 3, y: 4.2, label: 'T: Éco Territoriale'}
            ];

            new Chart(ctx, {
                type: 'scatter',
                data: {
                    datasets: [
                        {
                            label: 'Environnement',
                            data: dataEnv,
                            backgroundColor: '#22c55e', // Green
                            pointRadius: 6,
                            pointHoverRadius: 8
                        },
                        {
                            label: 'Social',
                            data: dataSoc,
                            backgroundColor: '#3b82f6', // Blue
                            pointRadius: 6,
                            pointHoverRadius: 8
                        },
                        {
                            label: 'Économique/Sociétal',
                            data: dataEco,
                            backgroundColor: '#f97316', // Orange
                            pointRadius: 6,
                            pointHoverRadius: 8
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            type: 'linear',
                            position: 'bottom',
                            title: { display: true, text: 'Importance Interne (Performance)' },
                            min: 0,
                            max: 6
                        },
                        y: {
                            title: { display: true, text: 'Importance Parties Prenantes' },
                            min: 0,
                            max: 7
                        }
                    },
                    plugins: {
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return context.raw.label;
                                }
                            }
                        },
                        annotation: {
                            // Note: Annotation plugin not loaded to keep simple CDN, using simple quadrants concept conceptually
                        }
                    }
                }
            });
        }
    </script>
</body>
</html>
