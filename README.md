<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MDU Pharmacy Presentation Slides</title>
    
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom font */
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            /* Background GIF: Universe Stars */
            background-image: url('https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExdGc2ZHN1d3B3MTdvdzN4NG80Zm9lY3JocjF4NTJvdTFjMWc1aGR3aiZlcF9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/2ikwSAc0a52N2/giphy.gif');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            overflow: hidden;
            position: relative;
        }

        /* Dark overlay for better text readability */
        body::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.7);
            z-index: -1;
        }

        .slide-container {
            width: 90%;
            max-width: 1200px;
            height: 90%;
            max-height: 800px;
            background-color: rgba(255, 255, 255, 0.95);
            border-radius: 1.5rem;
            box-shadow: 0 40px 60px -15px rgba(0, 0, 0, 0.7);
            position: relative;
            overflow: hidden;
        }
        
        /* --- CORE SLIDE TRANSITION STYLES (UPDATED) --- */
        .slide {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            padding: 3rem 5rem;
            
            /* Transition properties */
            transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out;
            
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            pointer-events: none; /* Default to non-interactive */

            /* Default non-active position: off-screen right */
            transform: translateX(100%);
            opacity: 0;
        }

        /* Active slide is centered and visible */
        .slide.active {
            opacity: 1;
            transform: translateX(0);
            pointer-events: auto; /* Make active slide interactive */
        }
        
        /* New class for slides that need to enter from the left or exit to the left */
        .slide.start-left {
            transform: translateX(-100%);
        }

        /* --- Wipe Out Effect Styles (Slide 1) --- */
        .wipe-item {
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275), transform 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }
        .wipe-item.revealed {
            opacity: 1;
            transform: translateY(0);
        }
        .academic-title { color: #0c4a6e; text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1); }
        
        /* --- FULL SCREEN MODAL STYLES (Transition & Dynamic Content) --- */
        #full-screen-modal {
            position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
            background-color: rgba(0, 0, 0, 0.9); display: flex;
            justify-content: center; align-items: center; z-index: 50;
            opacity: 0; visibility: hidden; transition: opacity 0.5s ease-in-out;
            cursor: pointer; 
        }
        #full-screen-modal.visible { opacity: 1; visibility: visible; }
        
        .glass-welcome, .glass-dynamic {
            background-color: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255, 255, 255, 0.3);
            transition: all 0.5s ease-in-out;
        }
        .wipe-out { opacity: 0 !important; transform: scale(0.8) !important; }
        
        .glowing-text { animation: glow 1.5s infinite alternate ease-in-out; }
        @keyframes glow {
            0% { text-shadow: 0 0 5px #f0f, 0 0 10px #0ff, 0 0 15px #ff0; }
            50% { text-shadow: 0 0 15px #0ff, 0 0 30px #f0f, 0 0 45px #0f0; }
            100% { text-shadow: 0 0 5px #ff0, 0 0 10px #f0f, 0 0 15px #0ff; }
        }
        
        /* Dynamic Content Area specific styles (for Slide 3 detail) */
        #dynamic-content-area {
            cursor: default; 
            max-height: 90vh;
            overflow-y: auto;
            text-align: left;
            padding: 3rem;
            width: 80%;
            max-width: 900px;
        }
        #dynamic-content-area b.glow {
            color: #fde047; /* Yellow glow base color */
            animation: text-glow 1.5s infinite alternate ease-in-out;
        }
        @keyframes text-glow {
            0% { text-shadow: 0 0 4px rgba(253, 224, 71, 0.5); }
            100% { text-shadow: 0 0 8px rgba(253, 224, 71, 0.8), 0 0 12px rgba(253, 224, 71, 0.3); }
        }
        .animate-pulse {
            animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
        @keyframes pulse {
            0%, 100% { 
                opacity: 1;
                transform: scale(1);
            }
            50% { 
                opacity: 0.8;
                transform: scale(1.05);
            }
        }

        /* --- LIQUID GLASS HERB INFO PANEL (Side Panel) --- */
        #herb-info-panel {
            position: fixed; top: 0; right: 0; width: 50vw; height: 100vh; 
            background-color: rgba(255, 255, 255, 0.2); 
            backdrop-filter: blur(15px); border-left: 1px solid rgba(255, 255, 255, 0.3);
            box-shadow: -10px 0 30px rgba(0, 0, 0, 0.3);
            z-index: 40; transform: translateX(100%); 
            transition: transform 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94); 
            display: flex; flex-direction: column; justify-content: center; 
            align-items: center; text-align: center; padding: 4rem;
            color: #1f2937; /* Dark text color (near black) */
        }
        #herb-info-panel.visible { transform: translateX(0); }
        
        #herb-info-panel .close-btn { 
            position: absolute; top: 2rem; right: 2rem; background: none; border: none; 
            font-size: 2.5rem; color: #1f2937; cursor: pointer; transition: color 0.3s ease; 
            line-height: 1; 
        }
        #herb-info-panel .close-btn:hover { color: black; }
        #herb-info-panel h4 { font-size: 3.5rem; }
        #herb-info-panel p { font-size: 1.8rem; }

        /* Flow Chart Styles (Slide 11) */
        .flow-step {
            min-width: 150px;
            padding: 1rem;
            border: 2px solid #3b82f6;
            background-color: #eff6ff;
            border-radius: 0.75rem;
            text-align: center;
            font-weight: 600;
            color: #1e40af;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            position: relative;
        }
        .flow-arrow {
            color: #3b82f6;
            font-size: 2rem;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 0.5rem;
            transform: translateY(0.25rem);
        }
        
        /* Responsive adjustments */
        @media (max-width: 768px) {
            .slide-container { padding: 1.5rem 2rem; }
            .slide { padding: 1.5rem 2rem; }
            .text-5xl { font-size: 2.25rem; }
            .text-3xl { font-size: 1.75rem; }
            .text-xl { font-size: 1.125rem; }
            .text-7xl { font-size: 3rem; }
            
            #herb-info-panel { width: 100vw; padding: 2rem; }
            #herb-info-panel h4 { font-size: 2.5rem; }
            #panel-herb-details { font-size: 1.2rem; }
            #herb-info-panel .close-btn { top: 1rem; right: 1rem; font-size: 2rem; }

            #dynamic-content-area { width: 95%; padding: 1.5rem; }
            #dynamic-content-area h3 { font-size: 2rem; }
            #dynamic-content-area .text-xl { font-size: 1rem; }
            #dynamic-content-area h4 { font-size: 1.5rem; }

            .flow-chart-container {
                flex-direction: column;
                align-items: center;
            }
            .flow-step { margin-bottom: 0.5rem; }
            .flow-arrow { 
                transform: rotate(90deg);
                margin: 0.25rem 0;
            }
            .responsive-table { font-size: 0.8rem; }
            .responsive-table th, .responsive-table td { padding: 0.5rem; }
        }
    </style>
</head>
<body onclick="hideHerbInfo()">

    <div class="slide-container">
        
        <!-- SLIDE 1: TITLE SLIDE -->
        <div id="slide1" class="slide active">
            
            <div id="header-block" class="wipe-item text-center">
                <h1 class="text-3xl font-bold academic-title mb-1">
                    MAHARSHI DAYANAND UNIVERSITY, ROHTAK
                </h1>
                <p class="text-xl text-gray-700">
                    Department of Pharmaceutical Sciences
                </p>
                <p class="text-lg text-gray-500 font-semibold mt-2">
                    B. Pharmacy (7th Semester) Project
                </p>
             </div>

            <div id="title-block" class=" text-center py-8">
                
                <div id="clickable-title" onclick="handleTitleClick(event)" 
                     class="inline-block p-8 rounded-2xl shadow-xl cursor-pointer transition-all duration-300 transform hover:scale-[1.03] active:scale-[0.98]" 
                     style="background: linear-gradient(135deg, #1d4ed8 0%, #3b82f6 100%);">
                   
                    <h2 class="text-5xl font-extrabold text-white leading-tight drop-shadow-lg">
                        POLYHERBAL ELIXIR
                    </h2>
                    <h3 class="text-3xl font-semibold text-sky-100 mt-3">
                        with  Enteric Coated Pearls
                    </h3>
                </div>
            </div>

            <div class= "flex flex-col md:flex-row justify-between pt-8 border-t border-sky-200">
                <div id="presenters-block" class=" mb-6 md:mb-0 md:w-1/2">
                    <h4 class="text-xl font-bold text-sky-700 mb-2 border-b-2 border-sky-300 pb-1 inline-block">
                        PRESENTED BY:
                    </h4>
                    <ul class="text-gray-800 space-y-1 text-lg list-none pl-0 mt-2">
                        <li class="hover:text-sky-600 transition duration-150"><span class="font-semibold">Gayatri</span> (8028)</li>
                        <li class="hover:text-sky-600 transition duration-150"><span class="font-semibold">Shivanshu</span> (8043)</li>
                        <li class="hover:text-sky-600 transition duration-150"><span class="font-semibold">Sudhir</span> (8044)</li>
                        <li class="hover:text-sky-600 transition duration-150"><span class="font-semibold">Vishakha</span> (8048)</li>
                    </ul>
                </div>

                <div id="supervisor-block" class=" md:w-1/2 md:text-right">
                    <h4 class="text-xl font-bold text-sky-700 mb-2 border-b-2 border-sky-300 pb-1 inline-block md:float-right">
                        PRESENTED TO:
                    </h4>
                    <p class="clear-both text-gray-900 text-2xl font-bold mt-2">
                        Prof. Anju Dhiman
                    </p>
                    <p class="text-gray-600 text-lg">
                        (Department of Pharmaceutical Sciences)
                    </p>
                </div>
            </div>
            </div>
        <!-- END SLIDE 1 -->

        
        <!-- SLIDE 2 - 12 (PREVIOUSLY GENERATED SLIDES) -->
        <div id="slide2" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Introduction to Polyherbal Elixir</h2>
            <div class="flex-grow overflow-y-auto pr-4">
                <div class="space-y-6 text-gray-800 text-lg">
                    <p><span class="font-bold text-sky-800">Polyherbal Elixir</span> is a novel herbal formulation designed to deliver multiple herbal actives in a single dosage form for enhanced therapeutic efficacy.</p>
                    <p>In this formulation, the Active Pharmaceutical Ingredients (APIs) are encapsulated inside sodium alginate pearls, providing a controlled and targeted release of the herbal constituents.</p>
                    <p>Each pearl is enteric coated to ensure that the active ingredients are protected from gastric acid and are released specifically in the intestine, where absorption is optimal.</p>
                    <h3 class="text-xl font-bold text-sky-800 mt-6 mb-3">Synergistic Blend of Medicinal Herbs:</h3>
                    <ul class="space-y-3 pl-4 list-none">
                        <li id="herb-rosemary" onclick="showHerbInfo(event, '🌿 Rosemary (600–800 mg)', 'Enhances memory, focus, and cognitive performance through antioxidant and neuroprotective actions. Contains rosmarinic acid, which helps reduce cortisol levels and mental fatigue.')" class="cursor-pointer text-lg font-semibold text-gray-900 hover:text-green-600 transition duration-150">🌿 Rosemary</li>
                        <li id="herb-hibiscus" onclick="showHerbInfo(event, '🌺 Hibiscus (200–400 mg)', 'Rich in anthocyanins and flavonoids, helps lower blood pressure and oxidative stress. Promotes mental calmness and supports heart health under stress.')" class="cursor-pointer text-lg font-semibold text-gray-900 hover:text-green-600 transition duration-150">🌺 Hibiscus</li>
                        <li id="herb-chamomile" onclick="showHerbInfo(event, '🌼 Chamomile (250–500 mg)', 'Contains apigenin, which binds to GABA receptors in the brain to reduce anxiety and insomnia. Known for its soothing and antidepressant-like effects.')" class="cursor-pointer text-lg font-semibold text-gray-900 hover:text-green-600 transition duration-150">🌼 Chamomile</li>
                        <li id="herb-ashwagandha" onclick="showHerbInfo(event, '🌱 Ashwagandha (300–600 mg)', 'A well-known adaptogen that improves resilience to stress and reduces cortisol and anxiety levels. Enhances energy, mood stability, and immune function.')" class="cursor-pointer text-lg font-semibold text-gray-900 hover:text-green-600 transition duration-150">🌱 Ashwagandha</li>
                    </ul>
                </div>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 2 / 18</div>
        </div>
        <div id="slide3" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Importance of Polyherbal Elixir</h2>
            <div class="flex-grow overflow-y-auto pr-4 text-gray-800 space-y-5 text-lg">
                <p>The Polyherbal Elixir represents a significant advancement in herbal drug formulation, integrating multiple herbal extracts with modern controlled-release technology to achieve improved therapeutic performance.</p>
                <p>Unlike conventional single-herb preparations, a polyherbal elixir utilizes the synergistic action of different herbs, enhancing both efficacy and stability.</p>
                <p>In this formulation, the active herbal extracts — Rosemary, Hibiscus, Chamomile, and Ashwagandha — are incorporated within sodium alginate pearls, which serve as biocompatible polymeric carriers. These pearls allow 
                    <span class="font-bold text-sky-800 cursor-pointer hover:text-yellow-500 transition duration-200" onclick="showControlledReleaseDetails(event)">Controlled and Targeted Release</span>
                    of actives, minimizing degradation in gastric conditions and ensuring maximum absorption in the intestinal environment.
                </p>
                <p>The 
                    <span class="font-bold text-sky-800 cursor-pointer hover:text-yellow-500 transition duration-200" onclick="showBioavailabilityDetails(event)">enteric coating</span> 
                    further protects the formulation from stomach acid, enabling site-specific release of phytoconstituents, thereby increasing 
                    <span class="font-bold text-sky-800 cursor-pointer hover:text-yellow-500 transition duration-200" onclick="showBioavailabilityDetails(event)">bioavailability</span> 
                    and therapeutic efficiency.
                </p>
                <p>The concepts of 
                    <span class="font-bold text-sky-800 cursor-pointer hover:text-yellow-500 transition duration-200" onclick="showReleaseDetails(event)">Sustained and Prolonged Release</span>
                    are key to maximizing the therapeutic window and minimizing dose frequency for the patient.
                </p>
                <h3 class="text-xl font-bold text-sky-800 mt-6 mb-3">Key Advantages:</h3>
                <ul class="list-disc pl-8 space-y-2">
                    <li>Provides multidimensional health benefits such as stress reduction, antioxidant protection, and immune enhancement.</li>
                    <li>Ensures better patient acceptability due to its liquid form, pleasant taste, and easy administration.</li>
                    <li>Combines traditional Ayurvedic wisdom with modern pharmaceutical technology, marking a step forward toward scientific herbal drug standardization.</li>
                </ul>
                <p class="mt-6 font-semibold text-sky-800">Thus, the polyherbal elixir not only supports mental and physical wellness but also sets a benchmark in novel herbal drug delivery systems through the use of sodium alginate-based enteric pearls.</p>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 3 / 18</div>
        </div>
        <div id="slide4" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">The Role of Sodium Alginate in Drug Delivery</h2>
            <div class="flex-grow overflow-y-auto pr-4 text-gray-800 space-y-5 text-lg">
                <p><span class="font-bold text-sky-800">Sodium Alginate</span> is a natural polysaccharide obtained from brown seaweed, widely used as a biopolymer in controlled and targeted drug delivery systems.</p>
                <p>It forms hydrogel beads (pearls) through ionotropic gelation when mixed with calcium ions (Ca2+), providing a biocompatible matrix for drug encapsulation.</p>
                <h3 class="text-xl font-bold text-sky-800 mt-6 mb-3">Dual Role in the Polyherbal Elixir:</h3>
                <ol class="list-decimal pl-8 space-y-3">
                    <li><span class="font-bold text-gray-900">Encapsulation Agent:</span> Traps herbal actives (Rosemary, Hibiscus, Chamomile, and Ashwagandha) inside pearls to <span class="font-semibold text-sky-700">protect them from degradation</span>.</li>
                    <li><span class="font-bold text-gray-900">Controlled Release Polymer:</span> Regulates the diffusion of active ingredients, enabling <span class="font-semibold text-sky-700">sustained and prolonged release</span> in the intestine.</li>
                </ol>
                <p>The porous gel network of alginate maintains drug stability, prevents premature leakage, and allows gradual swelling and dissolution at intestinal pH.</p>
                <p class="mt-4">Sodium alginate is non-toxic, biodegradable, and compatible with herbal actives, making it an ideal polymer for polyherbal elixir-based drug delivery.</p>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 4 / 18</div>
        </div>
        <div id="slide5" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Advantages of Polyherbal Elixir Formulations</h2>
            <div class="grid md:grid-cols-2 gap-8 flex-grow overflow-y-auto pr-4">
                <div class="p-4 bg-sky-50 border border-sky-200 rounded-xl shadow-md transition duration-300 hover:shadow-lg">
                    <h3 class="text-xl font-bold text-sky-800 mb-2 flex items-center"><span class="mr-2 text-2xl">🤝</span> 1. Synergistic Therapeutic Effect</h3>
                    <p class="text-gray-700 text-lg">Combines multiple herbal extracts (Rosemary, Hibiscus, Chamomile, Ashwagandha) to provide enhanced efficacy compared to single-herb formulations.</p>
                </div>
                <div class="p-4 bg-sky-50 border border-sky-200 rounded-xl shadow-md transition duration-300 hover:shadow-lg">
                    <h3 class="text-xl font-bold text-sky-800 mb-2 flex items-center"><span class="mr-2 text-2xl">🎯</span> 2. Controlled & Targeted Delivery</h3>
                    <p class="text-gray-700 text-lg">Sodium alginate pearls and enteric coating ensure site-specific release in the intestine, improving bioavailability and therapeutic efficiency.</p>
                </div>
                <div class="p-4 bg-sky-50 border border-sky-200 rounded-xl shadow-md transition duration-300 hover:shadow-lg">
                    <h3 class="text-xl font-bold text-sky-800 mb-2 flex items-center"><span class="mr-2 text-2xl">⏳</span> 3. Sustained & Prolonged Action</h3>
                    <p class="text-gray-700 text-lg">Gradual release of actives maintains steady plasma concentrations, reducing frequency of dosing and improving patient compliance.</p>
                </div>
                <div class="p-4 bg-sky-50 border border-sky-200 rounded-xl shadow-md transition duration-300 hover:shadow-lg">
                    <h3 class="text-xl font-bold text-sky-800 mb-2 flex items-center"><span class="mr-2 text-2xl">🛡️</span> 4. Protection of Sensitive Herbal Actives</h3>
                    <p class="text-gray-700 text-lg">Enteric coating prevents degradation of acid-sensitive compounds in the stomach, preserving their potency and stability.</p>
                </div>
                <div class="md:col-span-2 grid md:grid-cols-2 gap-8">
                    <div class="p-4 bg-sky-50 border border-sky-200 rounded-xl shadow-md transition duration-300 hover:shadow-lg">
                        <h3 class="text-xl font-bold text-sky-800 mb-2 flex items-center"><span class="mr-2 text-2xl">🧘‍♀️</span> 5. Holistic Health Benefits</h3>
                        <p class="text-gray-700 text-lg">Provides stress relief, cognitive support, antioxidant protection, and immune modulation in a single formulation.</p>
                    </div>
                    <div class="p-4 bg-sky-50 border border-sky-200 rounded-xl shadow-md transition duration-300 hover:shadow-lg">
                        <h3 class="text-xl font-bold text-sky-800 mb-2 flex items-center"><span class="mr-2 text-2xl">💖</span> 6. Patient-Friendly Form</h3>
                        <p class="text-gray-700 text-lg">Being a liquid dosage form, the elixir is easy to administer, palatable, and suitable for a wide range of patients.</p>
                    </div>
                </div>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 5 / 18</div>
        </div>
        <div id="slide6" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Aim & Objectives of the Research</h2>
            <div class="flex flex-col gap-10 flex-grow overflow-y-auto pr-4">
                <div>
                    <h3 class="text-2xl font-extrabold text-sky-800 mb-3 p-3 bg-sky-100 rounded-lg shadow-inner flex items-center"><span class="mr-3 text-3xl">🎯</span> AIM</h3>
                    <p class="text-xl text-gray-800 pl-4 border-l-4 border-sky-400 font-medium">To develop a polyherbal elixir containing sodium alginate pearls with enteric-coated herbal actives for enhanced stress and anxiety management, ensuring controlled, targeted, and sustained release.</p>
                </div>
                <div>
                    <h3 class="text-2xl font-extrabold text-sky-800 mb-4 p-3 bg-sky-100 rounded-lg shadow-inner flex items-center"><span class="mr-3 text-3xl">✅</span> OBJECTIVES</h3>
                    <ol class="space-y-4 text-gray-800 text-lg">
                        <li class="p-3 border border-sky-200 rounded-lg bg-white shadow-sm hover:bg-sky-50 transition"><span class="font-bold text-sky-700">1. Formulation:</span> To formulate sodium alginate pearls encapsulating herbal extracts (Rosemary, Hibiscus, Chamomile, Ashwagandha).</li>
                        <li class="p-3 border border-sky-200 rounded-lg bg-white shadow-sm hover:bg-sky-50 transition"><span class="font-bold text-sky-700">2. Coating:</span> To apply enteric coating on pearls for protection against gastric degradation and targeted intestinal release.</li>
                        <li class="p-3 border border-sky-200 rounded-lg bg-white shadow-sm hover:bg-sky-50 transition"><span class="font-bold text-sky-700">3. Efficacy Enhancement:</span> To enhance bioavailability and therapeutic efficacy of the herbal actives.</li>
                        <li class="p-3 border border-sky-200 rounded-lg bg-white shadow-sm hover:bg-sky-50 transition"><span class="font-bold text-sky-700">4. Evaluation:</span> To evaluate the stability, release profile, and quality of the final polyherbal elixir formulation.</li>
                        <li class="p-3 border border-sky-200 rounded-lg bg-white shadow-sm hover:bg-sky-50 transition"><span class="font-bold text-sky-700">5. Dosage Form Development:</span> To develop a patient-friendly liquid dosage form suitable for stress, anxiety, and immune support.</li>
                    </ol>
                </div>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 6 / 18</div>
        </div>
        <div id="slide7" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Composition and API Selection</h2>
            <div class="flex-grow flex flex-col md:flex-row gap-10">
                <div class="md:w-3/5">
                    <h3 class="text-2xl font-extrabold text-sky-800 mb-4">Active Pharmaceutical Ingredients (APIs)</h3>
                    <div class="space-y-4 text-lg">
                        <div class="p-3 border-l-4 border-green-500 bg-green-50 rounded-lg shadow-sm">
                            <span class="text-xl font-bold text-green-700">🌿 Rosemary (600–800 mg):</span> Antioxidant, cognitive enhancer.
                        </div>
                        <div class="p-3 border-l-4 border-red-500 bg-red-50 rounded-lg shadow-sm">
                            <span class="text-xl font-bold text-red-700">🌺 Hibiscus (200–400 mg):</span> Cardioprotective, stress reducer.
                        </div>
                        <div class="p-3 border-l-4 border-yellow-500 bg-yellow-50 rounded-lg shadow-sm">
                            <span class="text-xl font-bold text-yellow-700">🌼 Chamomile (250–500 mg):</span> Anti-anxiety, calming agent.
                        </div>
                        <div class="p-3 border-l-4 border-purple-500 bg-purple-50 rounded-lg shadow-sm">
                            <span class="text-xl font-bold text-purple-700">🌱 Ashwagandha (300–600 mg):</span> Adaptogen, immunomodulator.
                        </div>
                    </div>
                </div>
                <div class="md:w-2/5">
                    <h3 class="text-2xl font-extrabold text-sky-800 mb-4">Rationale for Selection</h3>
                    <ul class="list-disc pl-5 text-lg space-y-3 text-gray-700">
                        <li>Synergistic effect of multiple herbs for enhanced stress and immune support.</li>
                        <li>Compatibility with sodium alginate pearls for controlled release.</li>
                        <li>Diabetic-friendly profile: No high glycemic excipients used in the formulation.</li>
                        <li>Targeted action on the CNS and immune system to address holistic health.</li>
                    </ul>
                </div>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 7 / 18</div>
        </div>
        <div id="slide8" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Selection of Diabetic-Friendly Herbs</h2>
            <div class="flex-grow overflow-y-auto pr-4 text-gray-800 space-y-6 text-xl">
                <p class="p-4 border-l-4 border-red-400 bg-red-50 rounded-lg shadow-sm font-semibold">The formulation is specifically designed to be safe for diabetic patients by focusing on herbs that do not elevate blood sugar and avoiding high glycemic excipients in the elixir base.</p>
                <h3 class="text-2xl font-extrabold text-sky-800 mb-4 mt-6">Safety Profile of Selected Herbs:</h3>
                <div class="grid md:grid-cols-2 gap-6">
                    <div class="p-4 bg-white border border-gray-200 rounded-xl shadow-md">
                        <span class="font-bold text-purple-700 text-xl block mb-1">🌱 Ashwagandha</span>
                        <p class="text-gray-700">Known to support stress relief without negatively affecting glucose levels. Some studies even suggest mild hypoglycemic effects.</p>
                    </div>
                    <div class="p-4 bg-white border border-gray-200 rounded-xl shadow-md">
                        <span class="font-bold text-green-700 text-xl block mb-1">🌿 Rosemary & 🌼 Chamomile</span>
                        <p class="text-gray-700">Primary actions are antioxidant and calming. They are generally considered safe for diabetics and do not contain significant carbohydrates.</p>
                    </div>
                    <div class="p-4 bg-white border border-gray-200 rounded-xl shadow-md md:col-span-2">
                        <span class="font-bold text-red-700 text-xl block mb-1">🌺 Hibiscus</span>
                        <p class="text-gray-700">Primarily included for blood pressure regulation and has minimal glycemic impact, making it suitable for patients managing both stress and hypertension associated with diabetes.</p>
                    </div>
                </div>
                <p class="mt-6 font-bold text-sky-800 border-t pt-4">Goal: Ensure the elixir is safe for diabetic patients while maintaining efficacy in stress and immune support.</p>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 8 / 18</div>
        </div>
        <div id="slide9" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Preparation of Sodium Alginate Pearls </h2>
            <div class="flex-grow overflow-y-auto pr-4">
                <ol class="space-y-6 text-lg list-none pl-0">
                    <li class="flex items-start p-4 bg-blue-50 rounded-xl shadow-md border-l-4 border-blue-400">
                        <span class="text-3xl font-extrabold text-blue-600 mr-4">1</span>
                        <div><span class="font-bold text-sky-800 text-xl block mb-1">Step 1: Herbal Extraction (Maceration)</span>
                            <p class="text-gray-700">Herbs are macerated in a suitable solvent (e.g., hydroalcoholic solution) to obtain concentrated extracts containing the APIs.</p>
                        </div>
                    </li>
                    <li class="flex items-start p-4 bg-blue-50 rounded-xl shadow-md border-l-4 border-blue-400">
                        <span class="text-3xl font-extrabold text-blue-600 mr-4">2</span>
                        <div><span class="font-bold text-sky-800 text-xl block mb-1">Step 2: Alginate Solution Preparation</span>
                            <p class="text-gray-700">Dissolve sodium alginate powder in distilled water under stirring to form a viscous polymer solution (the matrix).</p>
                        </div>
                    </li>
                    <li class="flex items-start p-4 bg-blue-50 rounded-xl shadow-md border-l-4 border-blue-400">
                        <span class="text-3xl font-extrabold text-blue-600 mr-4">3</span>
                        <div><span class="font-bold text-sky-800 text-xl block mb-1">Step 3: Pearl Formation (Ionotropic Gelation)</span>
                            <ul class="list-disc pl-5 text-gray-700 space-y-1">
                                <li>Herbal extracts are mixed into the alginate solution.</li>
                                <li>Using an injection needle (or specialized nozzle), droplets are added dropwise into the calcium chloride (CaCl2) solution.</li>
                                <li>Ionotropic gelation instantly forms spherical pearls encapsulating the herbal actives.</li>
                            </ul>
                        </div>
                    </li>
                    <li class="flex items-start p-4 bg-blue-50 rounded-xl shadow-md border-l-4 border-blue-400">
                        <span class="text-3xl font-extrabold text-blue-600 mr-4">4</span>
                        <div><span class="font-bold text-sky-800 text-xl block mb-1">Step 4: Drying and Coating</span>
                            <p class="text-gray-700">Pearls are dried at a controlled temperature to maintain integrity, followed by the application of the enteric coating (e.g., Eudragit,CAP) to ensure targeted intestinal release.</p>
                        </div>
                    </li>
                </ol>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 9 / 18</div>
        </div>
        <div id="slide10" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Incorporation of Pearls in Elixir Base</h2>
            <div class="flex-grow overflow-y-auto pr-4 text-gray-800">
                <h3 class="text-2xl font-extrabold text-sky-800 mb-4">Preparation Steps for Final Elixir:</h3>
                <ol class="space-y-6 text-lg list-none pl-0">
                    <li class="p-5 bg-sky-100 rounded-xl shadow-lg border-l-4 border-sky-500">
                        <span class="text-2xl font-bold text-sky-700 block mb-2">1. Prepare Elixir Base</span>
                        <p class="text-gray-700">The elixir base is prepared using appropriate non-glycemic sweeteners (e.g., sucralose or erythritol), thickeners, stabilizers, and natural flavoring agents (e.g., mint or berry) to ensure palatability and stability.</p>
                    </li>
                    <li class="p-5 bg-sky-100 rounded-xl shadow-lg border-l-4 border-sky-500">
                        <span class="text-2xl font-bold text-sky-700 block mb-2">2. Incorporate Coated Pearls</span>
                        <p class="text-gray-700">The dried, enteric-coated sodium alginate pearls are slowly incorporated into the pre-prepared elixir base under gentle stirring to prevent damage to the coating.</p>
                    </li>
                    <li class="p-5 bg-sky-100 rounded-xl shadow-lg border-l-4 border-sky-500">
                        <span class="text-2xl font-bold text-sky-700 block mb-2">3. Ensure Homogeneous Distribution</span>
                        <p class="text-gray-700">Mixing time and speed are crucial to ensure homogeneous distribution of the pearls throughout the liquid, guaranteeing a consistent dose in every serving.</p>
                    </li>
                    <li class="p-5 bg-sky-100 rounded-xl shadow-lg border-l-4 border-sky-500">
                        <span class="text-2xl font-bold text-sky-700 block mb-2">4. Final Quality Check</span>
                        <ul class="list-disc pl-5 text-gray-700 space-y-1">
                            <li>Evaluate Pearl Integrity (visual check for cracking or swelling).</li>
                            <li>Measure pH and Viscosity (must be within specification for stability and patient compliance).</li>
                            <li>Check for Total API Content uniformity.</li>
                        </ul>
                    </li>
                </ol>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 10 / 18</div>
        </div>
        <div id="slide11" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Manufacturing Process Flow Chart </h2>
            <div class="flex-grow overflow-x-auto overflow-y-hidden py-8">
                <div class="flow-chart-container flex items-center justify-start min-w-[1200px] h-full">
                    <div class="flow-step">1. Herbal Extracts (Maceration)</div>
                    <span class="flow-arrow">&rarr;</span>
                    <div class="flow-step">2. Mix with Na Alginate Solution</div>
                    <span class="flow-arrow">&rarr;</span>
                    <div class="flow-step bg-yellow-200 border-yellow-500 text-yellow-900">3. Dropwise into CaCl2 Bath<br>(Pearl Formation)</div>
                    <span class="flow-arrow">&rarr;</span>
                    <div class="flow-step">4. Drying Pearls</div>
                    <span class="flow-arrow">&rarr;</span>
                    <div class="flow-step bg-green-200 border-green-500 text-green-900">5. Enteric Coating Application<br>(Targeted Release)</div>
                    <span class="flow-arrow">&rarr;</span>
                    <div class="flow-step">6. Prepare Elixir Base</div>
                    <span class="flow-arrow">&rarr;</span>
                    <div class="flow-step bg-purple-200 border-purple-500 text-purple-900">7. Incorporation & QC<br>(Final Elixir)</div>
                </div>
            </div>
            <p class="text-center text-lg text-gray-600 font-semibold pt-4">The flow highlights the crucial steps of Ionotropic Gelation (Pearl Formation) and Enteric Coating for controlled release.</p>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 11 / 18</div>
        </div>
        <div id="slide12" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Optimization Parameters</h2>
            <div class="flex-grow overflow-y-auto pr-4">
                <p class="text-lg text-gray-700 mb-6 font-semibold">Key parameters optimized to ensure stable, effective, and patient-friendly elixir with high pearl efficiency and proper drug release:</p>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 text-lg">
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">1. Alginate Concentration</span><p class="text-gray-700">Affects pearl formation, viscosity of the mix, and final gel strength (stability).</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">2. Herbal Extract Load</span><p class="text-gray-700">Ensures therapeutic dose without compromising pearl integrity and encapsulation efficiency.</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">3. Droplet Size / Needle Gauge</span><p class="text-gray-700">Controls final pearl size and uniformity, critical for consistent dosing.</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">4. Calcium Chloride Concentration</span><p class="text-gray-700">Influences crosslinking degree and the hardness/rigidity of the hydrogel pearls.</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">5. Drying Conditions</span><p class="text-gray-700">Prevents cracking, deformation, and ensures stability of acid-sensitive actives during processing.</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">6. Enteric Coating Thickness</span><p class="text-gray-700">The primary factor guaranteeing stomach acid protection and proper intestinal release time.</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500 md:col-span-2"><span class="font-bold text-amber-800 text-xl block mb-1">7. Elixir pH & Viscosity</span><p class="text-gray-700">Maintains chemical stability of the overall liquid formulation and prevents pearl sedimentation or aggregation.</p></div>
                </div>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 12 / 18</div>
        </div>
        <!-- END PREVIOUSLY GENERATED SLIDES -->

        <!-- --- SLIDE 13: EVALUATION PARAMETERS OF ELIXIR --- -->
        <div id="slide13" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">
               <marquee> Evaluation Parameters of Elixir</marquee> 
            </h2>

            <div class="flex-grow overflow-y-auto pr-4 space-y-5 text-gray-800 text-xl">
                
                <div class="p-4 bg-blue-50 border-l-4 border-blue-400 rounded-lg shadow-sm">
                    <span class="font-bold text-blue-700 cursor-pointer hover:text-yellow-600 transition duration-200"
                          onclick="showEvaluationDetail(event, 'Organoleptic Properties', 'These are sensory characteristics evaluated visually and manually: Color (uniformity), Odor (natural herbal fragrance), Taste (palatability), and Appearance (clarity, absence of precipitation). Crucial for patient acceptance.')">
                        1. Organoleptic Properties:
                    </span>
                    <span class="text-lg block mt-1">Color, odor, taste, and appearance.</span>
                </div>
                
                <div class="p-4 bg-blue-50 border-l-4 border-blue-400 rounded-lg shadow-sm">
                    <span class="font-bold text-blue-700">2. pH Measurement:</span> 
                    <span class="text-lg block mt-1">Ensures stability and compatibility of the liquid base with alginate pearls.</span>
                </div>
                
                <div class="p-4 bg-blue-50 border-l-4 border-blue-400 rounded-lg shadow-sm">
                    <span class="font-bold text-blue-700">3. Viscosity & Flow:</span> 
                    <span class="text-lg block mt-1">Determines pourability, ease of administration, and smooth suspension of pearls.</span>
                </div>
                
                <div class="p-4 bg-blue-50 border-l-4 border-blue-400 rounded-lg shadow-sm">
                    <span class="font-bold text-blue-700">4. Content Uniformity:</span> 
                    <span class="text-lg block mt-1">Confirms consistent herbal API distribution throughout the elixir base.</span>
                </div>
                
                <div class="p-4 bg-blue-50 border-l-4 border-blue-400 rounded-lg shadow-sm">
                    <span class="font-bold text-blue-700 cursor-pointer hover:text-yellow-600 transition duration-200"
                          onclick="showEvaluationDetail(event, 'In-vitro Release Studies (The Core Test)', 'Performed using USP Dissolution Apparatus in sequential media: 2 hours in Simulated Gastric Fluid (pH 1.2) to check coating integrity, followed by release in Simulated Intestinal Fluid (pH 6.8). This confirms controlled and targeted release.')">
                        5. In-vitro Release Studies:
                    </span>
                    <span class="text-lg block mt-1">Checks controlled and targeted release from alginate pearls.</span>
                </div>
                
                <div class="p-4 bg-blue-50 border-l-4 border-blue-400 rounded-lg shadow-sm">
                    <span class="font-bold text-blue-700">6. Microbial Load & Safety:</span> 
                    <span class="text-lg block mt-1">Ensures product complies with pharmacopeial standards (e.g., absence of pathogens).</span>
                </div>
            </div>

            <div class="text-center text-sm text-gray-500 pt-4">Slide 13 / 18</div>
        </div>
        <!-- --- END SLIDE 13 --- -->

        <!-- --- SLIDE 14: PHYSICAL & CHEMICAL EVALUATION --- -->
        <div id="slide14" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">
                Physical & Chemical Evaluation 
            </h2>

            <div class="grid md:grid-cols-2 gap-8 flex-grow overflow-y-auto pr-4 text-gray-800 text-lg">
                
                <!-- Physical Tests -->
                <div class="p-5 bg-yellow-50 border border-yellow-300 rounded-xl shadow-lg">
                    <h3 class="text-2xl font-extrabold text-yellow-800 mb-3 border-b pb-2">Physical Tests</h3>
                    <ul class="list-disc pl-5 space-y-2">
                        <li>Clarity, color, taste, and sedimentation in elixir base.</li>
                        <li>Pearl integrity (visual inspection under microscopy).</li>
                        <li>Shape retention and resistance to breakage in syrup.</li>
                        <li>Sedimentation volume and ease of redispersibility.</li>
                    </ul>
                </div>

                <!-- Chemical Tests -->
                <div class="p-5 bg-green-50 border border-green-300 rounded-xl shadow-lg">
                    <h3 class="text-2xl font-extrabold text-green-800 mb-3 border-b pb-2">Chemical Tests</h3>
                    <ul class="list-disc pl-5 space-y-2">
                        <li>
                            <span class="font-bold text-green-700 cursor-pointer hover:text-yellow-600 transition duration-200"
                                  onclick="showChemicalDetail(event, 'Active Marker Quantification', 'Key marker compounds like Rosmarinic acid (Rosemary) and Withanolides (Ashwagandha) are quantified using High-Performance Liquid Chromatography (HPLC) to ensure accurate therapeutic dosing.')">
                                Active marker quantification
                            </span> (e.g., Rosmarinic acid, Withanolides).
                        </li>
                        <li>pH stability over time.</li>
                        <li>
                            <span class="font-bold text-green-700 cursor-pointer hover:text-yellow-600 transition duration-200"
                                  onclick="showChemicalDetail(event, 'Assay for Total Bioactives', 'Total flavonoids and phenolics (polyphenols) are measured using spectroscopic methods (e.g., Folin-Ciocalteu assay and Aluminum chloride colorimetric assay) to assess overall antioxidant potential and batch consistency.')">
                                Assay for total flavonoids and phenolics.
                            </span>
                        </li>
                        <li>Assay for total solids content.</li>
                    </ul>
                </div>
            </div>

            <p class="text-center text-xl font-semibold text-sky-800 mt-6">
                These tests confirm the potency and uniformity of the Polyherbal Elixir.
            </p>

            <div class="text-center text-sm text-gray-500 pt-4">Slide 14 / 18</div>
        </div>
        <!-- --- END SLIDE 14 --- -->

        <!-- --- SLIDE 15: FLOW PROPERTIES OF PEARLS --- -->
        <div id="slide15" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">
                Flow Properties of Pearls 
            </h2>

            <div class="flex-grow flex flex-col gap-6 overflow-y-auto pr-4 text-gray-800 text-lg">
                
                <p class="text-xl p-3 bg-sky-100 border-l-4 border-sky-500 rounded-lg font-semibold">
                    Importance: Ensures uniform dosing and smooth, lump-free incorporation into the viscous elixir base.
                </p>

                <h3 class="text-2xl font-extrabold text-sky-800 mb-3 mt-4">Parameters Evaluated:</h3>
                
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse responsive-table">
                        <thead>
                            <tr class="bg-sky-200 text-sky-900">
                                <th class="p-3 border border-sky-300 w-1/4">Parameter</th>
                                <th class="p-3 border border-sky-300 w-1/4">Measurement</th>
                                <th class="p-3 border border-sky-300 w-1/2">Significance</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr class="bg-white hover:bg-gray-50 transition">
                                <td class="p-3 border border-sky-300 font-bold">Angle of Repose </td>
                                <td class="p-3 border border-sky-300">Height of cone formed by pearls</td>
                                <td class="p-3 border border-sky-300">Measures the friction and cohesion between particles (flowability). Lower angle indicates better flow.</td>
                            </tr>
                            <tr class="bg-gray-100 hover:bg-gray-50 transition">
                                <td class="p-3 border border-sky-300 font-bold">Carr’s Index (CI)</td>
                                <td class="p-3 border border-sky-300">Calculated from tapped and bulk density</td>
                                <td class="p-3 border border-sky-300">Predicts compressibility; lower CI (below 15) indicates excellent flow.</td>
                            </tr>
                            <tr class="bg-white hover:bg-gray-50 transition">
                                <td class="p-3 border border-sky-300 font-bold">Hausner Ratio (HR)</td>
                                <td class="p-3 border border-sky-300">Ratio of tapped density to bulk density</td>
                                <td class="p-3 border border-sky-300">Indicates inter-particle friction; HR close to 1.0 indicates good packing and flow.</td>
                            </tr>
                        </tbody>
                    </table>
                </div>

                <p class="text-xl font-bold text-green-700 p-3 mt-4 bg-green-50 rounded-lg shadow-inner">
                    Result: Due to their spherical shape and uniform size, the sodium alginate pearls show excellent flow properties, minimizing clumping during mixing.
                </p>
            </div>

            <div class="text-center text-sm text-gray-500 pt-4">Slide 15 / 18</div>
        </div>
        <!-- --- END SLIDE 15 --- -->

        <!-- --- SLIDE 16: STABILITY STUDIES --- -->
        <div id="slide16" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">
                Stability Studies (ICH Guidelines) 
            </h2>

            <div class="flex-grow overflow-y-auto pr-4 text-gray-800 space-y-6 text-xl">
                
                <p class="font-semibold text-sky-800 p-3 bg-sky-100 rounded-lg border-l-4 border-sky-400">
                    Objective: To assess the physical, chemical, and microbial integrity of the Polyherbal Elixir over the intended shelf life under various environmental conditions.
                </p>

                <h3 class="text-2xl font-extrabold text-sky-800 mb-4 mt-6">Conditions Tested (ICH):</h3>
                
                <div class="grid md:grid-cols-2 gap-6">
                    <div class="p-4 bg-white border border-gray-200 rounded-xl shadow-md">
                        <span class="font-bold text-gray-900 text-xl block mb-1">Long-term / Real-time Conditions</span>
                        <p class="text-gray-700">Storage at (25℃±2℃) for up to 12 months (or the proposed shelf life).</p>
                    </div>
                    <div class="p-4 bg-white border border-gray-200 rounded-xl shadow-md">
                        <span class="font-bold text-gray-900 text-xl block mb-1">Accelerated Conditions</span>
                        <p class="text-gray-700">Storage at (40℃±2℃) and (75℃±5℃) for 6 months to predict shelf life.</p>
                    </div>
                </div>

                <h3 class="text-2xl font-extrabold text-sky-800 mb-4 mt-6">Parameters Monitored Regularly:</h3>
                
                <ul class="list-disc pl-8 space-y-2 text-lg">
                    <li>Physical: Appearance, color, viscosity, sedimentation, and pearl integrity.</li>
                    <li>Chemical: pH, API Content (Rosmarinic acid, Withanolides), and total phenolic content.</li>
                    <li>Microbial: Total viable count and absence of specific pathogens.</li>
                </ul>

                <p class="mt-6 font-bold text-green-700 p-3 bg-green-50 rounded-lg shadow-inner">
                    Outcome: The formulation remains stable, palatable, and effective for its intended shelf life, demonstrating robust encapsulation.
                </p>
            </div>

            <div class="text-center text-sm text-gray-500 pt-4">Slide 16 / 18</div>
        </div>
        <!-- --- END SLIDE 16 --- -->

        <!-- --- SLIDE 17: DISCUSSION OF RESULTS --- -->
        <div id="slide17" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">
                Discussion of Key Results 
            </h2>

            <div class="flex-grow overflow-y-auto pr-4 space-y-6 text-gray-800 text-xl">
                
                <div class="p-4 bg-purple-50 border-l-4 border-purple-400 rounded-lg shadow-sm">
                    <span class="font-bold text-purple-700 cursor-pointer hover:text-yellow-600 transition duration-200"
                          onclick="showResultDetail(event, 'Controlled & Targeted Release Confirmed', 'The dissolution profile shows less than 10% drug release in Simulated Gastric Fluid (pH 1.2), confirming the effectiveness of the enteric coating. The remaining actives are fully released over 8-10 hours in Simulated Intestinal Fluid (pH 6.8), validating the sustained and targeted intestinal release.')">
                        1. Controlled & Targeted Release:
                    </span>
                    <span class="text-lg block mt-1">Enteric-coated pearls successfully release actives in the intestine, enhancing bioavailability.</span>
                </div>
                
                <div class="p-4 bg-purple-50 border-l-4 border-purple-400 rounded-lg shadow-sm">
                    <span class="font-bold text-purple-700 cursor-pointer hover:text-yellow-600 transition duration-200"
                          onclick="showResultDetail(event, 'Enhanced Therapeutic Efficacy', 'The combination of Rosemary (cognitive/antioxidant), Hibiscus (cardioprotective), Chamomile (anxiolytic), and Ashwagandha (adaptogenic) demonstrates a synergistic effect. This simultaneous, sustained delivery addresses the holistic spectrum of stress, anxiety, and immune health more effectively than single-herb treatments.')">
                        2. Therapeutic Efficacy:
                    </span>
                    <span class="text-lg block mt-1">Herbs maintain synergistic effect for stress, anxiety, and immune support upon release.</span>
                </div>
                
                <div class="p-4 bg-purple-50 border-l-4 border-purple-400 rounded-lg shadow-sm">
                    <span class="font-bold text-purple-700">3. Flow & Incorporation:</span> 
                    <span class="text-lg block mt-1">Pearls show good flow (Angle of Repose <25 Θ), uniformity, and stability in the elixir base without aggregation</span>]
                </div>
                
                <div class="p-4 bg-purple-50 border-l-4 border-purple-400 rounded-lg shadow-sm">
                    <span class="font-bold text-purple-700">4. Physical & Chemical Stability:</span> 
                    <span class="text-lg block mt-1">Elixir retains pH, color, and required active content throughout stability testing (up to 6 months accelerated).</span>
                </div>
                
                <div class="p-4 bg-purple-50 border-l-4 border-purple-400 rounded-lg shadow-sm">
                    <span class="font-bold text-purple-700">5. Patient Compliance:</span> 
                    <span class="text-lg block mt-1">Liquid dosage form with pleasant taste and smooth pearl suspension improves acceptability, especially for geriatric or pediatric patients.</span>
                </div>
            </div>

            <div class="text-center text-sm text-gray-500 pt-4">Slide 17 / 18</div>
        </div>
        <!-- --- END SLIDE 17 --- -->

        <!-- --- SLIDE 18: COMPARATIVE ANALYSIS --- -->
        <div id="slide18" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">
                Comparative Analysis with Conventional Formulations and QC Testing
            </h2>

            <div class="flex-grow flex flex-col gap-6 overflow-y-auto pr-4 text-gray-800 text-lg">
                
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse responsive-table">
                        <thead>
                            <tr class="bg-sky-200 text-sky-900">
                                <th class="p-3 border border-sky-300 w-1/4">Parameter</th>
                                <th class="p-3 border border-sky-300 w-1/3">Polyherbal Elixir (with Pearls)</th>
                                <th class="p-3 border border-sky-300 w-1/3">Conventional Herbal Syrup</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr class="bg-white hover:bg-gray-50 transition">
                                <td class="p-3 border border-sky-300 font-bold">Release Profile</td>
                                <td class="p-3 border border-sky-300">Controlled & targeted intestinal release</td>
                                <td class="p-3 border border-sky-300">Immediate release in stomach</td>
                            </tr>
                            <tr class="bg-gray-100 hover:bg-gray-50 transition">
                                <td class="p-3 border border-sky-300 font-bold">Bioavailability</td>
                                <td class="p-3 border border-sky-300">Enhanced due to enteric-coated pearls</td>
                                <td class="p-3 border border-sky-300">Reduced due to gastric degradation</td>
                            </tr>
                            <tr class="bg-white hover:bg-gray-50 transition">
                                <td class="p-3 border border-sky-300 font-bold">Stability</td>
                                <td class="p-3 border border-sky-300">High, active protected by alginate matrix</td>
                                <td class="p-3 border border-sky-300">Moderate, sensitive to pH and heat</td>
                            </tr>
                            <tr class="bg-gray-100 hover:bg-gray-50 transition">
                                <td class="p-3 border border-sky-300 font-bold">Patient Compliance</td>
                                <td class="p-3 border border-sky-300">High, smooth, easy to take liquid form</td>
                                <td class="p-3 border border-sky-300">Moderate, sometimes bitter or prone to sedimentation</td>
                            </tr>
                            <tr class="bg-white hover:bg-gray-50 transition">
                                <td class="p-3 border border-sky-300 font-bold">Therapeutic Efficiency</td>
                                <td class="p-3 border border-sky-300">Synergistic, sustained effect</td>
                                <td class="p-3 border border-sky-300">Limited to immediate action</td>
                            </tr>
                             <div id="slide18" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">
                Comparative Analysis with Conventional Formulations
            </h2>

            <div class="flex-grow flex flex-col gap-6 overflow-y-auto pr-4 text-gray-800 text-lg">
                
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse responsive-table">
                        <thead>
                            <tr class="bg-sky-200 text-sky-900">
                                <th class="p-3 border border-sky-300 w-1/4">Tests</th>
                                <th class="p-3 border border-sky-300 w-1/3">Purpose and method</th>
                                <th class="p-3 border border-sky-300 w-1/3">Acceptance criteria</th>
                            </tr>
                        </thead>
                    </table>
                        </tbody><table>
                         </tr><tr  >
                                <td class="p-3 border border-sky-300 font-bold">PH</td>
                                <td class="p-3 border border-sky-300">To ensure stability,preserving efficiency and palatability.Measured by using a caliberated PH meter at 25℃</td>
                                <td class="p-3 border border-sky-300">3.0-6.5</td>
                            </tr>
                             <tr>
                                <td class="p-3 border border-sky-300 font-bold">Heavy metal test</td>
                                <td class="p-3 border border-sky-300">To detect toxic metal contamination from raw herbs.Determined by atomic absorption spectroscopy(AAS)</td>
                                <td class="p-3 border border-sky-300">Pb≤10ppm,Cd≤0.3ppm,As≤3ppm,Hg≤1ppm</td>
                            </tr>
                            <tr>
                                <td class="p-3 border border-sky-300 font-bold">Specific gravity/Relative density </td>
                                <td class="p-3 border border-sky-300">Ensure batch to batch consistency of syrup base by using pycnometer</td>
                                <td class="p-3 border border-sky-300">±2% of Batch mean</td>
                            </tr>
                            <tr>
                                <td class="p-3 border border-sky-300 font-bold">Enteric coating Check </td>
                                <td class="p-3 border border-sky-300">Verify coated pearls resist 0.1N HCl and release in pH 6.8 buffer. Method: incubate pearls 2 h in 0.1N HCl (observe/no disintegration) then transfer to pH 6.8 phosphate buffer and observe release (visual or by assaying media).</td>
                                <td class="p-3 border border-sky-300">No disintegration in 0.1N HCl (2 h); complete release within defined time in pH 6.8 (e.g., within 1–2 h — set exact target)</td>
                            </tr>
                            <tr>
                                <td class="p-3 border border-sky-300 font-bold">Weight Variation of Pearls </td>
                                <td class="p-3 border border-sky-300">	Check uniformity of each pearl (dose unit). Method: weigh 20–30 pearls individually on analytical balance → calculate mean & %RSD</td>
                                <td class="p-3 border border-sky-300">%RSD ≤ 6–8% (or meet pharmacopeial guideline)</td>
                            </tr>
                           
                    </table>
                </div>

                <h3 class="text-2xl font-extrabold text-sky-800 p-3 bg-sky-100 rounded-lg shadow-inner mt-6">
                    Conclusion:
                </h3>
                <p class="text-xl font-medium text-gray-800 pl-4 border-l-4 border-sky-600">
                    The Polyherbal Elixir with sodium alginate pearls offers superior efficacy, stability, and patient acceptability compared to conventional herbal syrups, representing a validated step forward in modern phytopharmaceuticals.
                </p>
              
            </div>

            <div class="text-center text-sm text-gray-500 pt-4">Slide 18 / 18</div>
        </div>
        <!-- --- END SLIDE 18 --- -->
<!-- --- SLIDE 19: THANK YOU --- -->
       <!-- --- SLIDE 19: THANK YOU --- -->
        <div id="slide19" class="slide">
            <div class="flex-grow flex items-center justify-center">
                <h1 class="text-9xl font-extrabold text-sky-700 animate-pulse">
                    THANK YOU
                </h1>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 19 / 19</div>
        </div>
        <!-- --- END SLIDE 19 --- -->
        

        <!-- --- NAVIGATION BUTTONS --- -->
        <div class="absolute bottom-6 left-10 right-10 flex justify-between z-10">
            <button id="prev-btn" onclick="jumpToSlide(currentSlide - 1)" 
                    class="px-4 py-2 bg-sky-600 text-white rounded-full shadow-lg hover:bg-sky-700 transition duration-200 disabled:opacity-50" disabled>
                &larr; Previous
            </button>
            <button id="next-btn" onclick="jumpToSlide(currentSlide + 1)" 
                    class="px-4 py-2 bg-sky-600 text-white rounded-full shadow-lg hover:bg-sky-700 transition duration-200">
                Next &rarr;
            </button>
        </div>

    </div>
    

    <!-- --- FULL SCREEN ANIMATION MODAL (LIQUID GLASS DISPLAY) --- -->
    <div id="full-screen-modal" onclick="hideDynamicModal()">
        
        <!-- 1. Transition Message (Used for Slide 1 -> 2) -->
        <div id="welcome-message" 
             class="glass-welcome p-16 rounded-3xl text-center text-white transition-all duration-700 ease-in-out">
            <p class="text-7xl font-extrabold tracking-wider">WELCOME</p>
            <p class="text-3xl mt-4">To the Practice School Project</p>
        </div>
        <div id="begin-message" 
             class="absolute text-center opacity-0 transition-opacity duration-700 ease-in-out">
            <p class="text-7xl font-extrabold tracking-wider text-white glowing-text">LET'S BEGIN</p>
        </div>
        <!-- End of Transition Message -->

        <!-- 2. Dynamic Content Area (Used for Slide 3, 13, 14, 17 Details) -->
        <div id="dynamic-content-area" 
             class="glass-dynamic p-8 rounded-3xl text-white transition-opacity duration-300 hidden"
             onclick="event.stopPropagation()">
             
             <button id="dynamic-close-btn" onclick="hideDynamicModal(event)" 
                     class="absolute top-4 right-4 text-4xl opacity-70 hover:opacity-100 transition text-red-400 font-light">&times;</button>
             
             <h3 id="dynamic-title" class="text-4xl font-extrabold mb-6 text-yellow-300 glowing-text"></h3>
             <div id="dynamic-text-content" class="text-xl space-y-4 font-normal">
                 <!-- Content is inserted here by JavaScript's typing animation -->
             </div>
        </div>
        <!-- End of Dynamic Content Area -->
        
    </div>
    <!-- --- END FULL SCREEN ANIMATION MODAL --- -->

    <!-- --- LIQUID GLASS HERB INFO PANEL (Side Panel) --- -->
    <div id="herb-info-panel">
        <button class="close-btn" onclick="hideHerbInfo(event)">&times;</button>
        <h4 id="panel-herb-name"></h4>
        <p id="panel-herb-details"></p>
    </div>
    
    <script>
        // UPDATED totalSlides to 19
        let currentSlide = 1;
        const totalSlides = 19; 
        let typingInterval = null; 

        // --- Utility function for Typing Animation ---
        function typeText(elementId, htmlContent) {
            const targetElement = document.getElementById(elementId);
            if (!targetElement) return;

            // Clear previous content and animation
            targetElement.innerHTML = '';
            if (typingInterval) clearInterval(typingInterval);

            const parser = new DOMParser();
            const doc = parser.parseFromString(htmlContent, 'text/html');
            const nodes = doc.body.childNodes;
            
            let contentArray = [];
            
            // Recursive function to build the character array, pushing tags as single items
            function buildContentArray(node, array) {
                if (node.nodeType === 3) { // Text node
                    for (const char of node.textContent) {
                        array.push(char);
                    }
                } else if (node.nodeType === 1) { // Element node
                    const tagName = node.tagName.toLowerCase();
                    const attributes = Array.from(node.attributes).map(a => `${a.name}="${a.value}"`).join(' ');
                    const tagStart = `<${tagName}${attributes ? ' ' + attributes : ''}>`;
                    const tagEnd = `</${tagName}>`;
                    
                    array.push(tagStart);
                    Array.from(node.childNodes).forEach(child => buildContentArray(child, array));
                    array.push(tagEnd);
                }
            }
            
            Array.from(nodes).forEach(node => buildContentArray(node, contentArray));

            let charIndex = 0;
            let currentContent = '';
            
            typingInterval = setInterval(() => {
                if (charIndex < contentArray.length) {
                    const nextItem = contentArray[charIndex];
                    
                    if (nextItem.startsWith('<') && nextItem.endsWith('>')) {
                        // It's a tag, consume it whole
                        currentContent += nextItem;
                    } else {
                        // It's a single character
                        currentContent += nextItem;
                    }

                    targetElement.innerHTML = currentContent;
                    charIndex++;
                } else {
                    clearInterval(typingInterval);
                    typingInterval = null;
                }
            }, 5); 
        }
        
        // --- Modal Display Logic for Dynamic Content ---
        function showDynamicModal(title, contentHtml) {
            const modal = document.getElementById('full-screen-modal');
            const dynamicArea = document.getElementById('dynamic-content-area');
            const welcomeMsg = document.getElementById('welcome-message');
            const beginMsg = document.getElementById('begin-message');
            
            // Hide all other modal elements
            welcomeMsg.style.display = 'none';
            beginMsg.style.display = 'none';
            
            // Set content
            document.getElementById('dynamic-title').textContent = title;
            dynamicArea.classList.remove('hidden');

            // Show the modal
            modal.classList.add('visible');
            
            // Start typing animation on the content
            typeText('dynamic-text-content', contentHtml);
        }
        
        function hideDynamicModal(event) {
             const modal = document.getElementById('full-screen-modal');
             const dynamicArea = document.getElementById('dynamic-content-area');
             
             if (typingInterval) clearInterval(typingInterval);
             
             modal.classList.remove('visible');
             
             dynamicArea.classList.add('hidden');
             document.getElementById('welcome-message').style.display = 'flex'; 
             document.getElementById('begin-message').style.display = 'block'; 
        }
        
        // --- Slide 3 Detailed Content Functions ---
        function showControlledReleaseDetails(event) {
            event.stopPropagation();
            const title = "Controlled and Targeted Release";
            const contentHtml = `
                <h4 class="text-3xl font-bold mb-3 text-sky-300">Controlled Release:</h4>
                <p>
                    Controlled release refers to the regulated delivery of active ingredients at a predetermined rate over an extended period.
                </p>
                <p>
                    &rarr; Example: In this polyherbal elixir, sodium alginate pearls gradually release herbal actives (like ashwagandha and chamomile extracts) over time, maintaining <b class="glow">steady therapeutic levels</b> in the body and reducing the need for frequent dosing.
                </p>
                <div class="h-px bg-white/30 my-5"></div>
                <h4 class="text-3xl font-bold mb-3 text-sky-300">Targeted Release:</h4>
                <p>
                    Targeted release ensures that the active ingredients are delivered to a specific site in the body for maximum absorption and minimal side effects.
                </p>
                <p>
                    &rarr; Example: The enteric coating on alginate pearls prevents release in the stomach and allows herbal actives to be released in the intestine, where <b class="glow">absorption and bioavailability are higher</b>.
                </p>
            `;
            showDynamicModal(title, contentHtml);
        }

        function showBioavailabilityDetails(event) {
            event.stopPropagation();
            const title = "Enteric Coating & Bioavailability";
            const contentHtml = `
                <h3 class="text-3xl font-bold mb-3 text-sky-300">1️⃣ Enteric Coating:</h3>
                <h4 class="text-2xl font-semibold mt-3 mb-2 text-white/90">Purpose:</h4>
                <ul class="list-disc pl-8 space-y-1 text-lg">
                    <li><b class="glow">Prevents drug degradation</b> in stomach acid.</li>
                    <li><b class="glow">Enables targeted intestinal release</b> by resisting stomach pH (1.2).</li>
                </ul>
                <div class="h-px bg-white/30 my-5"></div>
                <h3 class="text-3xl font-bold mb-3 text-sky-300">2️⃣ Bioavailability:</h3>
                <h4 class="text-2xl font-semibold mt-3 mb-2 text-white/90">Definition:</h4>
                <p>
                    Bioavailability refers to the fraction of an administered dose that reaches the <b class="glow">systemic circulation in an active and usable form</b>.
                </p>
                <p>
                    <b class="glow">Our Approach:</b> By protecting the APIs from acid and targeting the release to the intestine (pH 6.8), we significantly increase the amount of active herb that gets absorbed, thus improving therapeutic efficiency.
                </p>
            `;
            showDynamicModal(title, contentHtml);
        }

        function showReleaseDetails(event) {
            event.stopPropagation();
            const title = "Sustained vs. Prolonged Release";
            const contentHtml = `
                <h3 class="text-3xl font-bold mb-3 text-sky-300">1️⃣ Sustained Release:</h3>
                <p>Designed to maintain a <b class="glow">constant concentration of drug</b> in the bloodstream by releasing it gradually over an extended period. This means fewer doses are needed.</p>
                <div class="h-px bg-white/30 my-5"></div>
                <h3 class="text-3xl font-bold mb-3 text-sky-300">2️⃣ Prolonged Release:</h3>
                <p>This formulation delays the initial release (via enteric coating) and then extends the duration of its therapeutic effect (via the alginate matrix), providing <b class="glow">longer duration of action</b> for stress relief.</p>
            `;
            showDynamicModal(title, contentHtml);
        }
        
        // --- Slide 13 Detailed Content Functions ---
        function showEvaluationDetail(event, title, content) {
            event.stopPropagation();
            showDynamicModal(title, content);
        }
        
        // --- Slide 14 Detailed Content Functions ---
        function showChemicalDetail(event, title, content) {
            event.stopPropagation();
            showDynamicModal(title, content);
        }
        
        // --- Slide 17 Detailed Content Functions ---
        function showResultDetail(event, title, content) {
            event.stopPropagation();
            showDynamicModal(title, content);
        }

        // --- Slide 1 Wipe Effect Logic ---
        const elementsToReveal = [
            document.getElementById('header-block'),
            document.getElementById('title-block'),
            document.getElementById('presenters-block'),
            document.getElementById('supervisor-block')
        ];
        let revealIndex = 0;
        const mainSlideDelay = 1000;

        function revealNextElement() {
            if (currentSlide === 1 && revealIndex < elementsToReveal.length) {
                const element = elementsToReveal[revealIndex];
                if (element) {
                    element.classList.add('revealed');
                    revealIndex++;
                    setTimeout(revealNextElement, mainSlidedelay);
                }
            }
        }
        
        // --- Navigation Logic ---
        function jumpToSlide(slideNumber) {
            if (slideNumber < 1 || slideNumber > totalSlides || slideNumber === currentSlide) return;
            
            const prevSlide = document.getElementById(`slide${currentSlide}`);
            const nextSlide = document.getElementById(`slide${slideNumber}`);
            const direction = slideNumber > currentSlide ? 'forward' : 'backward';

            if (direction === 'backward') {
                nextSlide.classList.add('start-left');
            } else {
                nextSlide.classList.remove('start-left');
            }
            
            requestAnimationFrame(() => {
                if (prevSlide) {
                    prevSlide.classList.remove('active');
                    if (direction === 'backward') {
                        prevSlide.classList.remove('start-left');
                    } else {
                        prevSlide.classList.add('start-left');
                    }
                }

                nextSlide.classList.add('active');
                nextSlide.classList.remove('start-left');
            });
            
            currentSlide = slideNumber;
            updateNavigationButtons();
            
            if (currentSlide === 1) {
                revealIndex = 0;
                elementsToReveal.forEach(el => el.classList.remove('revealed'));
                setTimeout(revealNextElement, 500); 
            }
            
            hideHerbInfo();
            hideDynamicModal(); 
        }

        function updateNavigationButtons() {
            document.getElementById('prev-btn').disabled = currentSlide === 1;
            document.getElementById('next-btn').disabled = currentSlide === totalSlides;
            
            // Update the footer for all slides dynamically
            for (let i = 1; i <= totalSlides; i++) {
                const slide = document.getElementById(`slide${i}`);
                if (slide) {
                    const footer = slide.querySelector('.text-center.text-sm.text-gray-500.pt-4');
                    if (footer) {
                        footer.textContent = `Slide ${i} / ${totalSlides}`;
                    }
                }
            }
        }

        // --- Clickable Title Animation Logic (Slide 1 to Slide 2 Transition) ---
        function handleTitleClick(event) {
            event.stopPropagation();
            const modal = document.getElementById('full-screen-modal');
            const welcomeMsg = document.getElementById('welcome-message');
            const beginMsg = document.getElementById('begin-message');
            
            document.getElementById('dynamic-content-area').classList.add('hidden');

            // 1. STAGE 1: Show Modal and Welcome Message
            modal.classList.add('visible');
            welcomeMsg.classList.remove('wipe-out');
            welcomeMsg.style.display = 'flex';
            beginMsg.style.display = 'block';
            beginMsg.classList.remove('opacity-100');

            // 2. STAGE 2: Wipe Out Welcome Message
            setTimeout(() => {
                welcomeMsg.classList.add('wipe-out');
            }, 3000); 

            // 3. STAGE 3: Show "Let's Begin"
            setTimeout(() => {
                welcomeMsg.style.display = 'none'; 
                beginMsg.classList.add('opacity-100');
            }, 3700);

            // 4. STAGE 4: Hide the entire modal and jump to Slide 2
            setTimeout(() => {
                modal.classList.remove('visible');
                welcomeMsg.classList.remove('wipe-out');
                beginMsg.classList.remove('opacity-100');
                
                jumpToSlide(2);
            }, 3700 + 2500);
        }
        
        // --- LIQUID GLASS HERB INFO PANEL Logic ---
        function showHerbInfo(event, name, details) {
            event.stopPropagation();
            const panel = document.getElementById('herb-info-panel');
            
            document.getElementById('panel-herb-name').textContent = name;
            document.getElementById('panel-herb-details').textContent = details;
            
            panel.classList.add('visible');
        }

        function hideHerbInfo(event) {
            const panel = document.getElementById('herb-info-panel');
            
            if (event && panel.contains(event.target) && !event.target.classList.contains('close-btn')) {
                return;
            }
            
            panel.classList.remove('visible');
        }

        // Initial setup on load
        window.onload = function() {
            setTimeout(revealNextElement, 500); 
            updateNavigationButtons();
            jumpToSlide(1); 
        };
    </script>

</body>
</html>
