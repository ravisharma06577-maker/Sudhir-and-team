<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>MDU Pharmacy Presentation Slides - Final</title>

    <!-- Tailwind (you already used it) -->
    <script src="https://cdn.tailwindcss.com"></script>

    <!-- Three.js (r128 compatible as used earlier) -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <style>
        /* ========== keep all original styles exactly (copied + fixed minor issues where necessary) ========== */
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');

        html,body { height:100%; }
        body {
            font-family: 'Inter', sans-serif;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background-image: url('https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExdGc2ZHN1d3B3MTdvdzN4NG80Zm9lY3JocjF4NTJvdTFjMWc1aGR3aiZlcF9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/2ikwSAc0a52N2/giphy.gif');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            overflow: hidden;
            position: relative;
            margin:0;
        }
        body::before {
            content:'';
            position:absolute;
            inset:0;
            background-color: rgba(1,1,1,0.7);
            z-index:-1;
        }

        .slide-container {
            width: 90%;
            max-width: 1200px;
            height: 90%;
            max-height: 800px;
            background-color: rgba(255,255,255,0.95);
            border-radius: 1.5rem;
            box-shadow: 0 40px 60px -15px rgba(0,0,0,0.7);
            position: relative;
            overflow: hidden;
        }

        /* Core slide transition */
        .slide {
            position: absolute;
            top: 0;
            left: 0;
            width:100%;
            height:100%;
            padding:3rem 5rem;
            transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out;
            display:flex;
            flex-direction:column;
            justify-content:space-between;
            pointer-events: none;
            transform: translateX(100%);
            opacity: 0;
        }
        .slide.active { opacity:1; transform: translateX(0); pointer-events:auto; }
        .slide.start-left { transform: translateX(-100%); }

        .wipe-item { opacity:0; transform: translateY(20px); transition: opacity 0.8s, transform 0.8s; }
        .wipe-item.revealed { opacity:1; transform: translateY(0); }
        .academic-title { color: #0307f9; text-shadow:1px 1px 2px rgba(0,0,0,0.1); }

        #full-screen-modal { position: fixed; top:0; left:0; width:100vw; height:100vh; background: rgba(0,0,0,0.9); display:flex; align-items:center; justify-content:center; z-index:50; opacity:0; visibility:hidden; transition: opacity 0.3s ease; cursor:pointer; }
        #full-screen-modal.visible { opacity:1; visibility:visible; }

        .glass-welcome, .glass-dynamic {
            background-color: rgba(255,255,255,0.15);
            backdrop-filter: blur(8px);
            border:1px solid rgba(255,255,255,0.3);
        }
        .wipe-out { opacity:0 !important; transform: scale(0.8) !important; }

        .glowing-text { animation: glow 1.5s infinite alternate ease-in-out; }
        @keyframes glow { 0% { text-shadow:0 0 5px #f0f,0 0 10px rgb(0, 255, 191) } 100% { text-shadow:0 0 8px rgb(0, 255, 242),0 0 12px #f0f } }

        #dynamic-content-area { cursor: default; max-height:90vh; overflow-y:auto; text-align:left; padding:3rem; width:80%; max-width:900px; }

        #herb-info-panel {
            position: fixed; top:0; right:0; width:50vw; height:100vh;
            background-color: rgba(255,255,255,0.21); backdrop-filter: blur(15px);
            border-left:1px solid rgba(255,255,255,0.3); box-shadow:-10px 0 30px rgba(0,0,0,0.3);
            z-index:40; transform: translateX(100%); transition: transform 0.5s; display:flex; flex-direction:column; justify-content:center; align-items:center; text-align:center; padding:4rem; color: #1f2937;
        }
        #herb-info-panel.visible { transform: translateX(0); }
        #herb-info-panel .close-btn { position:absolute; top:2rem; right:2rem; background:none; border:none; font-size:2.5rem; color:#1f2937; cursor:pointer; }
        #herb-info-panel h4 { font-size:3.5rem; } #herb-info-panel p { font-size:1.8rem; }

        .flow-step { min-width:150px; padding:1rem; border:2px solid #13171e; background-color:#eff6ff; border-radius:0.75rem; text-align:center; font-weight:600; color:#e15b2f; box-shadow:0 4px 6px rgba(0,0,0,0.1); position:relative; cursor: default; }
        .flow-arrow { color:#fb2e05; font-size:2rem; display:flex; align-items:center; justify-content:center; margin:0 0.5rem; transform: translateY(0.25rem); }
        .flow-step.clickable { cursor:pointer; box-shadow: 0 8px 18px rgba(0,0,0,0.18); transform: translateY(0); transition: transform 0.18s, box-shadow 0.18s; }
        .flow-step.clickable:hover { transform: translateY(-6px); box-shadow: 0 18px 36px rgba(0,0,0,0.28); }

        .animation-modal {
            display:none;
            position:fixed;
            inset:0;
            background: rgba(0,0,0,0.9);
            z-index:80;
            align-items:center;
            justify-content:center;
        }
        .animation-modal.active { display:flex; }

        .animation-container {
            width: 92%;
            height: 92%;
            border-radius: 18px;
            background: linear-gradient(180deg, rgba(255,255,255,0.06), rgba(255,255,255,0.02));
            box-shadow: 0 40px 80px rgba(0,0,0,0.6);
            position:relative;
            overflow:hidden;
        }
        .animation-close {
            position:absolute; top:18px; right:22px; z-index:20; width:44px; height:44px; border-radius:50%; border:none; background:#e74c3c; color:white; font-size:22px; cursor:pointer;
        }
        .animation-title { position:absolute; left:20px; top:18px; z-index:20; font-weight:700; color:#fff; font-size:20px; text-shadow:0 2px 6px rgba(0,0,0,0.6); }
        #canvas3d { width:100%; height:100%; display:block; }

        /* Slide 19 fireworks container style */
        #slide19 {
            display:flex;
            flex-direction:column;
            align-items:center;
            justify-content:center;
        }
        #fireworks-canvas { 
    width:100%; 
    height:50vh; 
    max-height:400px; 
    border-radius: 12px; 
    display:block; 
}
        /* Responsive */
        @media (max-width: 768px) {
            .slide { padding: 1.5rem 2rem; }
            #herb-info-panel { width:100vw; padding:2rem; }
            #herb-info-panel h4 { font-size:2.5rem; }
            .flow-chart-container { flex-direction: column; align-items:center; }
            .flow-step { margin-bottom:0.5rem; }
            .flow-arrow { transform: rotate(90deg); margin: 0.25rem 0; }
        }
        /* Polyherbal Interactive Animation Styles */
/* Polyherbal Interactive Animation Styles */
.rotating-container {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 700px;
    height: 700px;
    z-index: 100;
    pointer-events: none;
}

.message-box {
    position: absolute;
    width: 260px;
    padding: 1.8rem;
    background: linear-gradient(135deg, rgba(255,255,255,0.15), rgba(255,255,255,0.05));
    backdrop-filter: blur(12px);
    border: 1px solid rgba(255,255,255,0.3);
    border-radius: 1rem;
    box-shadow: 0 8px 32px rgba(0,0,0,0.3);
    color: white;
    cursor: pointer;
    pointer-events: auto;
    opacity: 0;
    transform: scale(0.3);
    transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.message-box.visible {
    opacity: 1;
    transform: scale(1);
}

.message-box:hover {
    transform: scale(1.05);
    box-shadow: 0 12px 48px rgba(0,0,0,0.5);
    border-color: rgba(255,255,255,0.5);
}

.message-box h4 {
    font-size: 1.3rem;
    font-weight: 700;
    margin-bottom: 0.8rem;
    color: #ffd700;
    text-shadow: 0 2px 8px rgba(0,0,0,0.4);
}

.message-box p {
    font-size: 1rem;
    line-height: 1.5;
    color: rgba(255,255,255,0.95);
}

.message-box-1 { top: -20px; left: 50%; transform: translateX(-50%) scale(0.3); }
.message-box-2 { right: -20px; top: 50%; transform: translateY(-50%) scale(0.3); }
.message-box-3 { bottom: -20px; left: 50%; transform: translateX(-50%) scale(0.3); }
.message-box-4 { left: -20px; top: 50%; transform: translateY(-50%) scale(0.3); }

.message-box-1.visible { transform: translateX(-50%) scale(1); }
.message-box-2.visible { transform: translateY(-50%) scale(1); }
.message-box-3.visible { transform: translateX(-50%) scale(1); }
.message-box-4.visible { transform: translateY(-50%) scale(1); }

.poly-overlay {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.85);
    z-index: 99;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s ease;
}

.poly-overlay.active {
    opacity: 1;
    visibility: visible;
}

@media (max-width: 768px) {
    .rotating-container {
        width: 100vw;
        height: 100vh;
        padding: 1rem;
    }
    .message-box {
        width: 85vw;
        padding: 1.5rem;
        position: static !important;
        margin: 1rem auto;
        transform: scale(1) !important;
    }
    .message-box-1, .message-box-2, .message-box-3, .message-box-4 {
        position: static !important;
        transform: scale(0.3) !important;
    }
    .message-box.visible {
        transform: scale(1) !important;
    }
}

    </style>
</head>
<body onclick="hideHerbInfo()">

    <!-- ========== SLIDE CONTAINER ========== -->
    <div class="slide-container">

        <!-- ========== Slide 1 ... Slide 18 (kept exactly same content as provided) - truncated in commentary but included in final file ========== -->
        <!-- I'll paste your slides unchanged here — the content you gave earlier is preserved exactly.
             For brevity of explanation I'll include them directly (they are identical to your last provided code),
             while adding only the animation hooks for Slide 11 and the new Slide 19 at the end.
        -->

        <!-- Slide 1 -->
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
                     style="background: linear-gradient(135deg, #1d4ed8 0%, #FF4500 100%);">
                   
                    <h2 class="text-5xl font-extrabold text-white leading-tight drop-shadow-lg">
                        POLYHERBAL Syrup
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
                        <li class="hover:text-sky-600 transition duration-150"><span class="font-semibold">Ritik</span> (8060)</li>
                        <li class="hover:text-sky-600 transition duration-150"><span class="font-semibold">Nischal</span> (8065)</li>
                        <li class="hover:text-sky-600 transition duration-150"><span class="font-semibold">Ravi</span> (8069)</li>
                        <li class="hover:text-sky-600 transition duration-150"><span class="font-semibold">Santosh</span> (8070)</li>
                        <li class="hover:text-sky-600 transition duration-150"><span class="font-semibold">Dinesh</span> (8081)</li>
                    
                    </ul>
                </div>

                <div id="supervisor-block" class=" md:w-1/2 md:text-right">
                    <h4 class="text-xl font-bold text-sky-700 mb-2 border-b-2 border-sky-300 pb-1 inline-block md:float-right">
                        PRESENTED TO:
                    </h4>
                    <p class="clear-both text-gray-900 text-2xl font-bold mt-2">
                        Prof. Vandana Garg
                    </p>
                    <p class="text-gray-600 text-lg">
                        (Department of Pharmaceutical Sciences)
                    </p>
                </div>
            </div>
        </div>

        <!-- Slide 2 -->
        <div id="slide2" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Introduction to Polyherbal syrup</h2>
            <div class="flex-grow overflow-y-auto pr-4">
                <div class="space-y-6 text-gray-800 text-lg">
                    <p>
    <span class="font-bold text-sky-800 cursor-pointer hover:text-yellow-500 transition-all duration-200 hover:scale-105 inline-block" 
          onclick="showPolyherbalAnimation(event)">
        Polyherbal Syrup:
    </span> 
    A novel herbal formulation for enhanced immune modulation and controlled delivery. In the current era of stress, pollution and unhealthy lifestyles, immune health has become a major concern. Herbal medicines, especially polyherbal formulations, play a vital role in enhancing immunity by combining the therapeutic benefits of multiple medicinal plants.
</p><p>The present study focuses on developing a polyherbal immunomodulatory syrup containing Ashwagandha, Shatavari, Safed Musli, and Triphala. These herbs are well-documented for their adaptogenic, antioxidant, and immune-boosting effects.</p>
         <p>To improve stability and targeted delivery, the active herbal extracts are entrapped into sodium alginate-based pearls and enteric-coated to protect the phytoconstituents from gastric degradation. This ensures controlled release in the intestinal region, where maximum absorption and immune stimulation occur.</p>
     <h3 class="text-xl font-bold text-sky-800 mt-6 mb-3">Synergistic Blend of Medicinal Herbs:</h3>
                    <ul class="space-y-3 pl-4 list-none">
                        <li id="herb-Triphla" onclick="showHerbInfo(event, '🌿 Triphla (250–500 mg)', 'Triphala acts as an immunomodulator by enhancing antioxidant defense, stimulating macrophage and lymphocyte activity, balancing cytokine levels, supporting gut immunity, and reducing inflammation..')" class="cursor-pointer text-lg font-semibold text-gray-900 hover:text-green-600 transition duration-150">🌿 Triphla</li>
                        <li id="herb-Shatavari" onclick="showHerbInfo(event, '🌺 Shatavari (300–600 mg)', 'Shatavari acts as an immunomodulator by enhancing antibody production, stimulating macrophage activity, balancing Th1/Th2 cytokine response, reducing oxidative stress, and promoting overall immune resilience..')" class="cursor-pointer text-lg font-semibold text-gray-900 hover:text-green-600 transition duration-150">🌺 Shatavari</li>
                        <li id="herb-Safed Musli" onclick="showHerbInfo(event, '🌼 Safed Musli (300–600 mg)', 'Safed Musli acts as an immunomodulator by enhancing macrophage and lymphocyte activity, stimulating antibody production, increasing natural killer (NK) cell response, reducing oxidative stress, and improving overall immune resistance and vitality..')" class="cursor-pointer text-lg font-semibold text-gray-900 hover:text-green-600 transition duration-150">🌼 Safed Musli</li>
                        <li id="herb-ashwagandha" onclick="showHerbInfo(event, '🌱 Ashwagandha (300–600 mg)', 'A well-known adaptogen that improves resilience to stress and reduces cortisol and anxiety levels. Enhances energy, mood stability, and immune function.')" class="cursor-pointer text-lg font-semibold text-gray-900 hover:text-green-600 transition duration-150">🌱 Ashwagandha</li>
                    </ul>
                </div>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 2 / 18</div>
        </div>

        <!-- Slide 3 -->
        <div id="slide3" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Why Use Enteric-Coated Sodium Alginate Pearls in a Polyherbal Syrup?</h2>
            <div class="flex-grow overflow-y-auto pr-4 text-gray-800 space-y-5 text-lg">
                <p>Traditional herbal syrups often face challenges like poor stability, low bioavailability, and degradation in acidic gastric conditions.
                </p>
                <p>Many active phytoconstituents from herbs such as Ashwagandha, Shatavari, Safed Musli, and Triphala are sensitive to stomach acid and lose potency before absorption.
                </p>
                <p>Unlike conventional single-herb preparations, a polyherbal elixir utilizes the synergistic action of different herbs, enhancing both efficacy and stability.</p>
                <p>In this formulation, the active herbal extracts — Triphla ,Shatavari, Safed Musli and Ashwagandha — are incorporated within sodium alginate pearls, which serve as biocompatible polymeric carriers. These pearls allow 
                    <span class="font-bold text-sky-800 cursor-pointer hover:text-red-500 transition duration-200" onclick="showControlledReleaseDetails(event)">Controlled and Targeted Release</span>
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
                    <li>Protection of actives from gastric acid.</li>
                    <li>Ensures better patient acceptability due to its liquid form, pleasant taste, and easy administration.</li>
                    <li>Combines traditional Ayurvedic wisdom with modern pharmaceutical technology, marking a step forward toward scientific herbal drug standardization.</li>
                </ul>
                <p class="mt-6 font-semibold text-sky-800">Thus, the polyherbal syrup not only enhances immune strength and overall wellness but also sets a benchmark in novel herbal drug delivery systems through the incorporation of sodium alginate-based enteric pearls.</p>       
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 3 / 18</div>
        </div>

        <!-- Slide 4 -->
        <div id="slide4" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">The Role of Sodium Alginate in Drug Delivery</h2>
            <div class="flex-grow overflow-y-auto pr-4 text-gray-800 space-y-5 text-lg">
                <p><span class="font-bold text-sky-800">Sodium Alginate</span> , a natural polysaccharide derived from brown seaweed, forms a gel matrix in the presence of calcium ions (Ca²⁺), entrapping the herbal actives inside.
                <p>Upon oral administration, the syrup containing pearls passes through the stomach without disintegration due to the enteric polymer coating (e.g., Eudragit or cellulose acetate phthalate).</p>
                <p>The coating resists acidic pH (1.2–3.0) of the stomach and dissolves only in the intestinal pH (6.8–7.4).</p>
                <p>Once the coating dissolves, the alginate matrix swells and slowly releases the herbal actives, ensuring sustained and site-specific delivery.</p>
                <p>This leads to improved bioavailability, enhanced immunomodulatory action, and reduced dosing frequency.</p>
                 <h3 class="text-xl font-bold text-sky-800 mt-6 mb-3">Dual Role in the Polyherbal Syrup:</h3>
                        <ol class="list-decimal pl-8 space-y-3">
                            <li><span class="font-bold text-gray-900">Encapsulation Agent:</span> Traps herbal actives (Triphla ,Shatavari ,Safed Musli and Ashwagandha) inside pearls to <span class="font-semibold text-sky-700">protect them from degradation</span>.</li>
                            <li><span class="font-bold text-gray-900">Controlled Release Polymer:</span> Regulates the diffusion of active ingredients, enabling <span class="font-semibold text-sky-700">sustained and prolonged release</span> in the intestine.</li>
                        </ol>
                        <p>The porous gel network of alginate maintains drug stability, prevents premature leakage, and allows gradual swelling and dissolution at intestinal pH.</p>
                        <p class="mt-4">Sodium alginate is non-toxic, biodegradable, and compatible with herbal actives, making it an ideal polymer for polyherbal syrup-based drug delivery.</p>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 4 / 18</div>
        </div>

        <!-- Slide 5 -->
        <div id="slide5" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Advantages of Polyherbal Syrup Formulations</h2>
            <div class="grid md:grid-cols-2 gap-8 flex-grow overflow-y-auto pr-4">
                <div class="p-4 bg-sky-50 border border-sky-200 rounded-xl shadow-md transition duration-300 hover:shadow-lg">
                    <h3 class="text-xl font-bold text-sky-800 mb-2 flex items-center"><span class="mr-2 text-2xl">🤝</span> 1. Synergistic Therapeutic Effect</h3>
                    <p class="text-gray-700 text-lg">Combines multiple herbal extracts (Triphla ,Shatavari ,Safed Musli and Ashwagandha) to provide enhanced efficacy compared to single-herb formulations.</p>
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
                        <p class="text-gray-700 text-lg">Being a liquid dosage form, the syrup is easy to administer, palatable, and suitable for a wide range of patients.</p>
                    </div>
                </div>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 5 / 18</div>
        </div>

        <!-- Slide 6 -->
        <div id="slide6" class="slide">
            <marquee><h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Aim & Objectives of the Research</h2></marquee>
            <div class="flex flex-col gap-10 flex-grow overflow-y-auto pr-4">
                <div>
                    <h3 class="text-2xl font-extrabold text-sky-800 mb-3 p-3 bg-sky-100 rounded-lg shadow-inner flex items-center"><span class="mr-3 text-3xl">🎯</span> AIM</h3>
                    <p class="text-xl text-gray-800 pl-4 border-l-4 border-sky-400 font-medium">To develop a polyherbal syrup containing sodium alginate pearls with enteric-coated herbal actives for enhanced immunomodulator action, ensuring controlled, targeted, and sustained release.</p>
                </div>
                <div>
                    <h3 class="text-2xl font-extrabold text-sky-800 mb-4 p-3 bg-sky-100 rounded-lg shadow-inner flex items-center"><span class="mr-3 text-3xl">✅</span> OBJECTIVES</h3>
                    <ol class="space-y-4 text-gray-800 text-lg">
                        <li class="p-3 border border-sky-200 rounded-lg bg-white shadow-sm hover:bg-sky-50 transition"><span class="font-bold text-sky-700">1. Formulation:</span> To formulate sodium alginate pearls encapsulating herbal extracts (Triphla,Shatavari,Safed Musli, Ashwagandha).</li>
                        <li class="p-3 border border-sky-200 rounded-lg bg-white shadow-sm hover:bg-sky-50 transition"><span class="font-bold text-sky-700">2. Coating:</span> To apply enteric coating on pearls for protection against gastric degradation and targeted intestinal release.</li>
                        <li class="p-3 border border-sky-200 rounded-lg bg-white shadow-sm hover:bg-sky-50 transition"><span class="font-bold text-sky-700">3. Efficacy Enhancement:</span> To enhance bioavailability and therapeutic efficacy of the herbal actives.</li>
                        <li class="p-3 border border-sky-200 rounded-lg bg-white shadow-sm hover:bg-sky-50 transition"><span class="font-bold text-sky-700">4. Evaluation:</span> To evaluate the stability, release profile, and quality of the final polyherbal elixir formulation.</li>
                        <li class="p-3 border border-sky-200 rounded-lg bg-white shadow-sm hover:bg-sky-50 transition"><span class="font-bold text-sky-700">5. Dosage Form Development:</span> To develop a patient-friendly liquid dosage form suitable for immune support.</li>
                    </ol>
                </div>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 6 / 18</div>
        </div>

        <!-- Slide 7 -->
        <div id="slide7" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Composition and API Selection</h2>
            <div class="flex-grow flex flex-col md:flex-row gap-10">
                <div class="md:w-3/5">
                    <h3 class="text-2xl font-extrabold text-sky-800 mb-4">Active Pharmaceutical Ingredients (APIs)</h3>
                    <div class="space-y-4 text-lg">
                        <div class="p-3 border-l-4 border-green-500 bg-green-50 rounded-lg shadow-sm">
                            <span class="text-xl font-bold text-green-700">🌿 Triphla(500–1000 mg):</span> Immunomodulatory (gallic acid), antioxidant (ellagic acid), hepatoprotective (ascorbic acid – vitamin C).
                        </div>
                        <div class="p-3 border-l-4 border-red-500 bg-red-50 rounded-lg shadow-sm">
                            <span class="text-xl font-bold text-red-700">🌺 Shatavari (300–600 mg):</span>Immunomodulatory (shatavarins), antioxidant (saponins), adaptogenic and rejuvenating (steroidal saponins).
                        </div>
                        <div class="p-3 border-l-4 border-yellow-500 bg-yellow-50 rounded-lg shadow-sm">
                            <span class="text-xl font-bold text-yellow-700">🌼 Safed Musli (300–600 mg):</span> Immunomodulatory (shatavarins), antioxidant (saponins), adaptogenic and rejuvenating (steroidal saponins).
                        </div>
                        <div class="p-3 border-l-4 border-purple-500 bg-purple-50 rounded-lg shadow-sm">
                            <span class="text-xl font-bold text-purple-700">🌱 Ashwagandha (300–600 mg):</span> Immunomodulatory (withanolides) , adaptogenic and rejuvenating (withaferin A).
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

        <!-- Slide 8 -->
        <div id="slide8" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Selection of Diabetic-Friendly Herbs</h2>
            <div class="flex-grow overflow-y-auto pr-4 text-gray-800 space-y-6 text-xl">
                <p class="p-4 border-l-4 border-red-400 bg-red-50 rounded-lg shadow-sm font-semibold">The formulation is specifically designed to be safe for diabetic patients by focusing on herbs that do not elevate blood sugar and avoiding high glycemic excipients in the syrup base.</p>
                <h3 class="text-2xl font-extrabold text-sky-800 mb-4 mt-6">Safety Profile of Selected Herbs:</h3>
                <div class="grid md:grid-cols-2 gap-6">
                    <div class="p-4 bg-white border border-gray-200 rounded-xl shadow-md">
                        <span class="font-bold text-purple-700 text-xl block mb-1">🌱 Ashwagandha</span>
                        <p class="text-gray-700">Known to enhance immune function by modulating cytokine activity and increasing white blood cell count, thereby improving the body’s resistance to stress and infections.</p>
                    </div>
                    <div class="p-4 bg-white border border-gray-200 rounded-xl shadow-md">
                        <span class="font-bold text-green-700 text-xl block mb-1">🌿 Triphla & 🌼 Safed Musli</span>
                        <p class="text-gray-700">Primary actions are immunomodulatory and rejuvenating. They help enhance the body’s defense mechanisms, support immune balance, and are generally considered safe for diabetics without affecting glucose levels.</p>
                    </div>
                    <div class="p-4 bg-white border border-gray-200 rounded-xl shadow-md md:col-span-2">
                        <span class="font-bold text-red-700 text-xl block mb-1">🌺 Shatavari</span>
                        <p class="text-gray-700">Primarily included for blood pressure regulation and has minimal glycemic impact, making it suitable for patients managing both stress and hypertension associated with diabetes.</p>
                    </div>
                </div>
                <p class="mt-6 font-bold text-sky-800 border-t pt-4">Goal: Ensure the syrup is safe for diabetic patients while maintaining efficacy in immune support.</p>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 8 / 18</div>
        </div>

        <!-- Slide 9 -->
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

        <!-- Slide 10 -->
        <div id="slide10" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Incorporation of Pearls in syrup Base</h2>
            <div class="flex-grow overflow-y-auto pr-4 text-gray-800">
                <h3 class="text-2xl font-extrabold text-sky-800 mb-4">Preparation Steps for Final syrup:</h3>
                <ol class="space-y-6 text-lg list-none pl-0">
                    <li class="p-5 bg-sky-100 rounded-xl shadow-lg border-l-4 border-sky-500">
                        <span class="text-2xl font-bold text-sky-700 block mb-2">1. Prepare syrup Base</span>
                        <p class="text-gray-700">The syrup base is prepared using appropriate non-glycemic sweeteners (e.g.,sorbitol, sucralose ,erythritol), thickeners, stabilizers, and natural flavoring agents (e.g., mint or berry) to ensure palatability and stability.</p>
                    </li>
                    <li class="p-5 bg-sky-100 rounded-xl shadow-lg border-l-4 border-sky-500">
                        <span class="text-2xl font-bold text-sky-700 block mb-2">2. Incorporate Coated Pearls</span>
                        <p class="text-gray-700">The dried, enteric-coated sodium alginate pearls are slowly incorporated into the pre-prepared syrup base under gentle stirring to prevent damage to the coating.</p>
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

        <!-- Slide 11 (FLOWCHART) - we will add click handlers to 3 specific flow-step cards -->
        <div id="slide11" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Manufacturing Process Flow Chart </h2>
            <div class="flex-grow overflow-x-auto overflow-y-hidden py-8">
                <div class="flow-chart-container flex items-center justify-start min-w-[1200px] h-full">
                    <div class="flow-step">1. Herbal Extracts (Maceration)</div>
                    <span class="flow-arrow">&rarr;</span>

                    <div class="flow-step">2. Mix with Na Alginate Solution</div>
                    <span class="flow-arrow">&rarr;</span>

                    <!-- 3: pearls formation (clickable) -->
                    <div class="flow-step bg-yellow-200 border-yellow-500 text-yellow-900 clickable" data-anim="pearl">
                        3. Dropwise into CaCl2 Bath<br>(Pearl Formation)
                    </div>
                    <span class="flow-arrow">&rarr;</span>

                    <div class="flow-step">4. Drying Pearls</div>
                    <span class="flow-arrow">&rarr;</span>

                    <!-- 5: enteric coating -->
                    <div class="flow-step bg-green-200 border-green-500 text-green-900 clickable" data-anim="coating">
                        5. Enteric Coating Application<br>(Targeted Release)
                    </div>
                    <span class="flow-arrow">&rarr;</span>

                    <!-- 6: base prep -->
                    <div class="flow-step clickable" data-anim="base">
                        6. Prepare syrup Base
                    </div>
                    <span class="flow-arrow">&rarr;</span>

                    <div class="flow-step bg-purple-200 border-purple-500 text-purple-900">7. Incorporation & QC<br>(Final syrup)</div>
                </div>
            </div>
            <marquee><p class="text-center text-lg text-gray-600 font-semibold pt-4">The flow highlights the crucial steps of Ionotropic Gelation (Pearl Formation) and Enteric Coating for controlled release.</p></marquee>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 11 / 18</div>
        </div>

        <!-- Slide 12 -->
        <div id="slide12" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">Optimization Parameters</h2>
            <div class="flex-grow overflow-y-auto pr-4">
                <p class="text-lg text-gray-700 mb-6 font-semibold">Key parameters optimized to ensure stable, effective, and patient-friendly syrup with high pearl efficiency and proper drug release:</p>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 text-lg">
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">1. Alginate Concentration</span><p class="text-gray-700">Affects pearl formation, viscosity of the mix, and final gel strength (stability).</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">2. Herbal Extract Load</span><p class="text-gray-700">Ensures therapeutic dose without compromising pearl integrity and encapsulation efficiency.</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">3. Droplet Size / Needle Gauge</span><p class="text-gray-700">Controls final pearl size and uniformity, critical for consistent dosing.</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">4. Calcium Chloride Concentration</span><p class="text-gray-700">Influences crosslinking degree and the hardness/rigidity of the hydrogel pearls.</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">5. Drying Conditions</span><p class="text-gray-700">Prevents cracking, deformation, and ensures stability of acid-sensitive actives during processing.</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500"><span class="font-bold text-amber-800 text-xl block mb-1">6. Enteric Coating Thickness</span><p class="text-gray-700">The primary factor guaranteeing stomach acid protection and proper intestinal release time.</p></div>
                    <div class="p-4 bg-white rounded-xl shadow-lg border-l-4 border-amber-500 md:col-span-2"><span class="font-bold text-amber-800 text-xl block mb-1">7. Syrup pH & Viscosity</span><p class="text-gray-700">Maintains chemical stability of the overall liquid formulation and prevents pearl sedimentation or aggregation.</p></div>
                </div>
            </div>
            <div class="text-center text-sm text-gray-500 pt-4">Slide 12 / 18</div>
        </div>

        <!-- Slide 13 -->
        <div id="slide13" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">
                Evaluation Parameters of Syrup 
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

        <!-- Slide 14 -->
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
                            </span> (e.g., Gallic acid ,Ellagic acid , Withanolides).
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
                These tests confirm the potency and uniformity of the Polyherbal Syrup.
            </p>

            <div class="text-center text-sm text-gray-500 pt-4">Slide 14 / 18</div>
        </div>

        <!-- Slide 15 -->
        <div id="slide15" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">
                Flow Properties of Pearls 
            </h2>

            <div class="flex-grow flex flex-col gap-6 overflow-y-auto pr-4 text-gray-800 text-lg">
                
                <p class="text-xl p-3 bg-sky-100 border-l-4 border-sky-500 rounded-lg font-semibold">
                    Importance: Ensures uniform dosing and smooth, lump-free incorporation into the viscous syrup base.
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

        <!-- Slide 16 -->
        <div id="slide16" class="slide">
            <h2 class="text-3xl font-bold text-sky-700 mb-6 border-b-2 border-sky-300 pb-2">
                Stability Studies (ICH Guidelines) 
            </h2>

            <div class="flex-grow overflow-y-auto pr-4 text-gray-800 space-y-6 text-xl">
                
                <p class="font-semibold text-sky-800 p-3 bg-sky-100 rounded-lg border-l-4 border-sky-400">
                    Objective: To assess the physical, chemical, and microbial integrity of the Polyherbal Syrup over the intended shelf life under various environmental conditions.
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
                    <li>Chemical: pH, API Content (Gallic acid, Withanolides), and total phenolic content.</li>
                    <li>Microbial: Total viable count and absence of specific pathogens.</li>
                </ul>

                <p class="mt-6 font-bold text-green-700 p-3 bg-green-50 rounded-lg shadow-inner">
                    Outcome: The formulation remains stable, palatable, and effective for its intended shelf life, demonstrating robust encapsulation.
                </p>
            </div>

            <div class="text-center text-sm text-gray-500 pt-4">Slide 16 / 18</div>
        </div>

        <!-- Slide 17 -->
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
                    <span class="text-lg block mt-1">Pearls show good flow (Angle of Repose <25 Θ), uniformity, and stability in the syrup base without aggregation.</span>
                </div>
                
                <div class="p-4 bg-purple-50 border-l-4 border-purple-400 rounded-lg shadow-sm">
                    <span class="font-bold text-purple-700">4. Physical & Chemical Stability:</span> 
                    <span class="text-lg block mt-1">Syrup retains pH, color, and required active content throughout stability testing (up to 6 months accelerated).</span>
                </div>
                
                <div class="p-4 bg-purple-50 border-l-4 border-purple-400 rounded-lg shadow-sm">
                    <span class="font-bold text-purple-700">5. Patient Compliance:</span> 
                    <span class="text-lg block mt-1">Liquid dosage form with pleasant taste and smooth pearl suspension improves acceptability, especially for geriatric or pediatric patients.</span>
                </div>
            </div>

            <div class="text-center text-sm text-gray-500 pt-4">Slide 17 / 18</div>
        </div>

        <!-- Slide 18 -->
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
                                <th class="p-3 border border-sky-300 w-1/3">Polyherbal Syrup (with Pearls)</th>
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
                        </tbody>
                    </table>
                </div>

                <h3 class="text-2xl font-extrabold text-sky-800 p-3 bg-sky-100 rounded-lg shadow-inner mt-6">
                    Conclusion:
                </h3>
                <p class="text-xl font-medium text-gray-800 pl-4 border-l-4 border-sky-600">
                    The Polyherbal Syrup with sodium alginate pearls offers superior efficacy, stability, and patient acceptability compared to conventional herbal syrups, representing a validated step forward in modern phytopharmaceuticals.
                </p>
               </div>

            <div class="text-center text-sm text-gray-500 pt-4">Slide 18 / 18</div>
        </div>

        <!-- ========== NEW Slide 19: THANK YOU + FIREWORKS ========== -->
<div id="slide19" class="slide" style="padding: 1rem 2rem !important;">
    <div style="flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:1rem;">
        <canvas id="fireworks-canvas" style="width:100%; height:50vh; max-height:400px; border-radius:12px;"></canvas>
        <div style="text-align:center;">
           <marquee> <h1 class="text-4xl md:text-5xl font-extrabold text-sky-800">Thank You! 🌿</h1></marquee>
        </div>
    </div>
    <div class="text-center text-sm text-gray-500 pt-2">Slide 19 / 19</div>
</div>

        <!-- Navigation Buttons -->
        <div class="absolute bottom-6 left-10 right-10 flex justify-between z-10">
            <button id="prev-btn" onclick="jumpToSlide(currentSlide - 1)" class="px-4 py-2 bg-sky-600 text-white rounded-full shadow-lg hover:bg-sky-700 transition duration-200 disabled:opacity-50">← Previous</button>
            <button id="next-btn" onclick="jumpToSlide(currentSlide + 1)" class="px-4 py-2 bg-sky-600 text-white rounded-full shadow-lg hover:bg-sky-700 transition duration-200">Next →</button>
        </div>
    </div>

    <!-- Full screen modal used previously for dynamic content -->
    <div id="full-screen-modal" onclick="hideDynamicModal()">
        <div id="welcome-message" class="glass-welcome p-16 rounded-3xl text-center text-white transition-all duration-700 ease-in-out">
            <p class="text-7xl font-extrabold tracking-wider">WELCOME</p>
            <p class="text-3xl mt-4">To the Practice School Project</p>
        </div>
        <div id="begin-message" class="absolute text-center opacity-0 transition-opacity duration-700 ease-in-out">
            <p class="text-7xl font-extrabold tracking-wider text-white glowing-text">LET'S BEGIN</p>
        </div>
        <div id="dynamic-content-area" class="glass-dynamic p-8 rounded-3xl text-white transition-opacity duration-300 hidden" onclick="event.stopPropagation()">
             <button id="dynamic-close-btn" onclick="hideDynamicModal(event)" class="absolute top-4 right-4 text-4xl opacity-70 hover:opacity-100 transition text-red-400 font-light">&times;</button>
             <h3 id="dynamic-title" class="text-4xl font-extrabold mb-6 text-yellow-300 glowing-text"></h3>
             <div id="dynamic-text-content" class="text-xl space-y-4 font-normal"></div>
        </div>
    </div>

    <!-- Animation modal for 3D visualizations -->
    <div id="animationModal" class="animation-modal" onclick="closeAnimationModal()">
        <div class="animation-container" onclick="event.stopPropagation()">
            <button class="animation-close" onclick="closeAnimationModal()">×</button>
            <div class="animation-title" id="animTitle"></div>
            <div id="canvas3d"></div>
        </div>
    </div>

    <!-- Herb info panel -->
    <div id="herb-info-panel">
        <button class="close-btn" onclick="hideHerbInfo(event)">&times;</button>
        <h4 id="panel-herb-name"></h4>
        <p id="panel-herb-details"></p>
    </div>
    <!-- Polyherbal Animation Overlay -->
<div id="poly-overlay" class="poly-overlay" onclick="closePolyherbalAnimation()"></div>
<div id="rotating-container" class="rotating-container" style="display:none;">
    <div class="message-box message-box-1" onclick="closeMessageBox(event, this)">
        <h4>📖 Definition</h4>
        <p id="msg-1"></p>
    </div>
    <div class="message-box message-box-2" onclick="closeMessageBox(event, this)">
        <h4>🌿 Herbal Combination</h4>
        <p id="msg-2"></p>
    </div>
    <div class="message-box message-box-3" onclick="closeMessageBox(event, this)">
        <h4>🤝 Synergistic Action</h4>
        <p id="msg-3"></p>
    </div>
    <div class="message-box message-box-4" onclick="closeMessageBox(event, this)">
        <h4>💚 Holistic Health Benefits</h4>
        <p id="msg-4"></p>
    </div>
</div>

    <script>
    /*******************************
     * Presentation & Animation JS *
     *******************************/

    // Slide state
    let currentSlide = 1;
    const totalSlides = 19; // updated to include Slide 19 fireworks
    let typingInterval = null;

    // Grab DOM references used across functions
    const prevBtn = document.getElementById('prev-btn');
    const nextBtn = document.getElementById('next-btn');
    /*************************************************************************
 * Polyherbal Interactive Animation System
 *************************************************************************/

let polyTypingIntervals = [];
let polyAnimationActive = false;

// Messages content
const polyMessages = {
    1: "A Polyherbal Syrup is a liquid pharmaceutical formulation that combines multiple medicinal herbs to provide synergistic therapeutic benefits, offering enhanced efficacy compared to single-herb preparations.",
    2: "This formulation combines four powerful herbs: Ashwagandha (adaptogenic), Shatavari (immunomodulatory), Safed Musli (vitality enhancer), and Triphala (antioxidant-rich), creating a comprehensive wellness solution.",
    3: "Synergistic Action occurs when multiple herbs work together to produce effects greater than the sum of their individual actions, enhancing bioavailability, therapeutic efficacy, and overall patient outcomes.",
    4: "Holistic Health Benefits include comprehensive immune modulation, stress resilience, enhanced vitality, antioxidant protection, digestive support, and improved overall wellness through multi-targeted herbal action."
};

function showPolyherbalAnimation(event) {
    event.stopPropagation();
    
    if (polyAnimationActive) return;
    polyAnimationActive = true;
    
    const overlay = document.getElementById('poly-overlay');
    const container = document.getElementById('rotating-container');
    
    overlay.classList.add('active');
    container.style.display = 'block';
    
    // Show boxes with staggered delay
    const boxes = container.querySelectorAll('.message-box');
    boxes.forEach((box, index) => {
        setTimeout(() => {
            box.classList.add('visible');
            // Start typing animation for this box
            const msgId = `msg-${index + 1}`;
            typePolyMessage(msgId, polyMessages[index + 1]);
        }, index * 300);
    });
    
    // Rotate container
   // Gentle wobble animation (2-3 degrees)
    let rotation = 0;
    let rotationDirection = 1;
    const rotateInterval = setInterval(() => {
        if (!polyAnimationActive) {
            clearInterval(rotateInterval);
            return;
        }
        
        // Oscillate between -2 and +2 degrees
        rotation += 0.05 * rotationDirection;
        
        // Reverse direction at limits
        if (rotation >= 2) rotationDirection = -1;
        if (rotation <= -2) rotationDirection = 1;
        
        container.style.transform = `translate(-50%, -50%) rotate(${rotation}deg)`;
    }, 50);
    
    // Store interval for cleanup
    polyTypingIntervals.push(rotateInterval);
}

function typePolyMessage(elementId, text) {
    const element = document.getElementById(elementId);
    if (!element) return;
    
    element.textContent = '';
    let charIndex = 0;
    
    const interval = setInterval(() => {
        if (charIndex < text.length) {
            element.textContent += text[charIndex];
            charIndex++;
        } else {
            clearInterval(interval);
        }
    }, 20); // 20ms per character for smooth typing
    
    polyTypingIntervals.push(interval);
}

function closeMessageBox(event, boxElement) {
    event.stopPropagation();
    boxElement.classList.remove('visible');
    
    // Check if all boxes are closed
    setTimeout(() => {
        const container = document.getElementById('rotating-container');
        const visibleBoxes = container.querySelectorAll('.message-box.visible');
        if (visibleBoxes.length === 0) {
            closePolyherbalAnimation();
        }
    }, 400);
}

function closePolyherbalAnimation() {
    if (!polyAnimationActive) return;
    
    polyAnimationActive = false;
    
    // Clear all typing intervals
    polyTypingIntervals.forEach(interval => clearInterval(interval));
    polyTypingIntervals = [];
    
    const overlay = document.getElementById('poly-overlay');
    const container = document.getElementById('rotating-container');
    const boxes = container.querySelectorAll('.message-box');
    
    boxes.forEach(box => box.classList.remove('visible'));
    
    setTimeout(() => {
        overlay.classList.remove('active');
        container.style.display = 'none';
        container.style.transform = 'translate(-50%, -50%) rotate(0deg)';
        
        // Reset message text
        for (let i = 1; i <= 4; i++) {
            const msg = document.getElementById(`msg-${i}`);
            if (msg) msg.textContent = '';
        }
    }, 400);
}

    // --- Typing animation helper (used by existing dynamic modal) ---
    function typeText(elementId, htmlContent) {
        const targetElement = document.getElementById(elementId);
        if (!targetElement) return;
        targetElement.innerHTML = '';
        if (typingInterval) clearInterval(typingInterval);

        const parser = new DOMParser();
        const doc = parser.parseFromString(htmlContent, 'text/html');
        const nodes = doc.body.childNodes;
        let contentArray = [];

        function buildContentArray(node, array) {
            if (node.nodeType === 3) {
                for (const char of node.textContent) array.push(char);
            } else if (node.nodeType === 1) {
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
                currentContent += contentArray[charIndex++];
                targetElement.innerHTML = currentContent;
            } else {
                clearInterval(typingInterval);
                typingInterval = null;
            }
        }, 5);
    }

    // --- Dynamic modal show/hide (existing content modal) ---
    function showDynamicModal(title, contentHtml) {
        const modal = document.getElementById('full-screen-modal');
        const dynamicArea = document.getElementById('dynamic-content-area');
        const welcomeMsg = document.getElementById('welcome-message');
        const beginMsg = document.getElementById('begin-message');

        welcomeMsg.style.display = 'none';
        beginMsg.style.display = 'none';
        document.getElementById('dynamic-title').textContent = title;
        dynamicArea.classList.remove('hidden');
        modal.classList.add('visible');
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

    // -------------------------
    // Slide detail helper functions (kept same text behavior)
    // -------------------------
    function showControlledReleaseDetails(event) {
        event.stopPropagation();
        const title = "Controlled and Targeted Release";
        const contentHtml = `
            <h4 class="text-3xl font-bold mb-3 text-sky-300">Controlled Release:</h4>
            <p>Controlled release refers to the regulated delivery of active ingredients at a predetermined rate over an extended period.</p>
            <p>&rarr; Example: In this polyherbal Syrup, sodium alginate pearls gradually release herbal actives over time, maintaining <b class="glow">steady therapeutic levels</b> in the body.</p>
            <div class="h-px bg-white/30 my-5"></div>
            <h4 class="text-3xl font-bold mb-3 text-sky-300">Targeted Release:</h4>
            <p>Targeted release ensures that the active ingredients are delivered to a specific site in the body for maximum absorption.</p>
            <p>&rarr; Example: The enteric coating prevents release in the stomach and allows herbal actives to be released in the intestine, where <b class="glow">absorption is higher</b>.</p>
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
                <li><b class="glow">Enables targeted intestinal release</b> by resisting stomach pH (1.2)</li>
            </ul>
            <div class="h-px bg-white/30 my-5"></div>
            <h3 class="text-3xl font-bold mb-3 text-sky-300">2️⃣ Bioavailability:</h3>
            <p>Bioavailability refers to the fraction of an administered dose that reaches the <b class="glow">systemic circulation in an active form</b>.</p>
            <p><b class="glow">Our Approach:</b> By protecting the APIs from acid and targeting release to the intestine (pH 6.8), we significantly increase absorption.</p>
        `;
        showDynamicModal(title, contentHtml);
    }

    function showReleaseDetails(event) {
        event.stopPropagation();
        const title = "Sustained vs. Prolonged Release";
        const contentHtml = `
            <h3 class="text-3xl font-bold mb-3 text-sky-300">1️⃣ Sustained Release:</h3>
            <p>Designed to maintain a <b class="glow">constant concentration of drug</b> in the bloodstream by releasing it gradually over an extended period.</p>
            <div class="h-px bg-white/30 my-5"></div>
            <h3 class="text-3xl font-bold mb-3 text-sky-300">2️⃣ Prolonged Release:</h3>
            <p>This formulation delays the initial release (via enteric coating) and then extends the duration of its therapeutic effect (via the alginate matrix).</p>
        `;
        showDynamicModal(title, contentHtml);
    }

    function showEvaluationDetail(event, title, content) {
        event.stopPropagation();
        showDynamicModal(title, content);
    }
    function showChemicalDetail(event, title, content) {
        event.stopPropagation();
        showDynamicModal(title, content);
    }
    function showResultDetail(event, title, content) {
        event.stopPropagation();
        showDynamicModal(title, content);
    }

    // -------------------------
    // Navigation logic (fixed issues)
    // -------------------------
    function updateNavigationButtons() {
        const prev = document.getElementById('prev-btn');
        const next = document.getElementById('next-btn');
        prev.disabled = currentSlide === 1;
        next.disabled = currentSlide === totalSlides;

        // update footer label on each slide if present
        for (let i = 1; i <= totalSlides; i++) {
            const slide = document.getElementById(`slide${i}`);
            if (slide) {
                const footer = slide.querySelector('.text-center.text-sm.text-gray-500.pt-4');
                if (footer) footer.textContent = `Slide ${i} / ${totalSlides}`;
            }
        }
    }

    function showSlide(n) {
        // bounds
        if (n < 1) n = 1;
        if (n > totalSlides) n = totalSlides;

        const prevSlideEl = document.getElementById(`slide${currentSlide}`);
        const nextSlideEl = document.getElementById(`slide${n}`);
        const direction = n > currentSlide ? 'forward' : 'backward';

        if (prevSlideEl) {
            prevSlideEl.classList.remove('active');
            if (direction === 'forward') prevSlideEl.classList.add('start-left');
            else prevSlideEl.classList.remove('start-left');
        }
        if (nextSlideEl) {
            nextSlideEl.classList.add('active');
            nextSlideEl.classList.remove('start-left');
        }

        currentSlide = n;
        updateNavigationButtons();

        // trigger fireworks when reaching slide 19
        if (currentSlide === 19) {
            startFireworks();
        } else {
            stopFireworks();
        }
    }

    function jumpToSlide(slideNumber) {
        if (slideNumber < 1 || slideNumber > totalSlides || slideNumber === currentSlide) return;
        showSlide(slideNumber);
    }

    function changeSlide(dir) {
        showSlide(currentSlide + dir);
    }

    // Keyboard nav
    document.addEventListener('keydown', (e) => {
        if (e.key === 'ArrowLeft') changeSlide(-1);
        else if (e.key === 'ArrowRight') changeSlide(1);
    });

    // --- Slide 1 reveal logic (fixed typo) ---
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
                setTimeout(revealNextElement, mainSlideDelay);
            }
        }
    }

    // --- Click title logic unchanged (but timing fixed) ---
    function handleTitleClick(event) {
        event.stopPropagation();
        const modal = document.getElementById('full-screen-modal');
        const welcomeMsg = document.getElementById('welcome-message');
        const beginMsg = document.getElementById('begin-message');

        document.getElementById('dynamic-content-area').classList.add('hidden');

        modal.classList.add('visible');
        welcomeMsg.classList.remove('wipe-out');
        welcomeMsg.style.display = 'flex';
        beginMsg.style.display = 'block';
        beginMsg.classList.remove('opacity-100');

        setTimeout(() => {
            welcomeMsg.classList.add('wipe-out');
        }, 3000);

        setTimeout(() => {
            welcomeMsg.style.display = 'none';
            beginMsg.classList.add('opacity-100');
        }, 3700);

        setTimeout(() => {
            modal.classList.remove('visible');
            welcomeMsg.classList.remove('wipe-out');
            beginMsg.classList.remove('opacity-100');
            showSlide(2);
        }, 3700 + 800);
    }

    // Herb panel
    function showHerbInfo(event, name, details) {
        event.stopPropagation();
        document.getElementById('panel-herb-name').textContent = name;
        document.getElementById('panel-herb-details').textContent = details;
        document.getElementById('herb-info-panel').classList.add('visible');
    }
    function hideHerbInfo(event) {
        const panel = document.getElementById('herb-info-panel');
        if (event && panel.contains(event.target) && !event.target.classList.contains('close-btn')) return;
        panel.classList.remove('visible');
    }

    // Initial setup
    window.onload = function() {
        setTimeout(revealNextElement, 500);
        updateNavigationButtons();
        showSlide(1);
        attachFlowStepHandlers(); // attach handlers for step cards on Slide 11
    };

    /*************************************************************************
     * 3D Animations (Slide 11) - pearl formation, syrup base, enteric coating
     * Each animation runs in the #canvas3d element inside #animationModal
     * After closing an animation modal we navigate to Slide 19 (Thank You) *
     *************************************************************************/

    let animScene, animCamera, animRenderer, animId;
    let activeAnimType = null;

    // Utility: clear animation canvas and dispose three.js renderer
    function disposeAnimation() {
        if (animId) {
            cancelAnimationFrame(animId);
            animId = null;
        }
        const container = document.getElementById('canvas3d');
        while (container && container.firstChild) container.removeChild(container.firstChild);
        if (animRenderer) {
            try {
                animRenderer.dispose();
            } catch (e) { /* ignore */ }
            animRenderer = null;
        }
        animScene = null;
        animCamera = null;
    }

    // Show animation modal
    function showAnimationModal(title, type) {
        const modal = document.getElementById('animationModal');
        const titleEl = document.getElementById('animTitle');
        titleEl.textContent = title;
        modal.classList.add('active');
        activeAnimType = type;

        // Slight delay to allow modal to be visible then initialize
        setTimeout(() => {
            if (type === 'pearl') initPearlAnimation();
            else if (type === 'base') initBaseAnimation();
            else if (type === 'coating') initCoatingAnimation();
        }, 80);
    }

    // Close animation modal (called by close button or clicking overlay)
    function closeAnimationModal() {
        const modal = document.getElementById('animationModal');
        modal.classList.remove('active');
        disposeAnimation();
        // After closing animation, go to Slide 19 with fireworks
        
    }

    // If user clicks on overlay outside container, close it
    function closeAnimationModalOverlay(e) {
        // handler attached to overlay div via onclick attribute; nothing needed here as closeAnimationModal called in markup
    }

    // Attach click handlers to the three clickable flow-step cards
    function attachFlowStepHandlers() {
        const steps = document.querySelectorAll('#slide11 .flow-step.clickable');
        steps.forEach(step => {
            const animType = step.dataset.anim; // 'pearl' | 'coating' | 'base'
            step.addEventListener('click', (e) => {
                e.stopPropagation();
                let title = '';
                if (animType === 'pearl') title = 'Pearl Formation (Ionotropic Gelation)';
                else if (animType === 'coating') title = 'Enteric Coating - Layering';
                else if (animType === 'base') title = 'Syrup Base Preparation - Mixing';
                showAnimationModal(title, animType);
            });
        });
    }

    // ---------- Pearl Animation (realistic droplet -> bead formation) ----------
    function initPearlAnimation() {
        const container = document.getElementById('canvas3d');
        animScene = new THREE.Scene();
        animScene.background = new THREE.Color(0x0b1220);

        animCamera = new THREE.PerspectiveCamera(50, container.clientWidth / container.clientHeight, 0.1, 1000);
        animCamera.position.set(0, 6, 14);
        animCamera.lookAt(0, 0, 0);

        animRenderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        animRenderer.setPixelRatio(window.devicePixelRatio);
        animRenderer.setSize(container.clientWidth, container.clientHeight);
        container.appendChild(animRenderer.domElement);

        // lights
        const hemi = new THREE.HemisphereLight(0xffffff, 0x444444, 0.6);
        animScene.add(hemi);
        const dir = new THREE.DirectionalLight(0xffffff, 0.9);
        dir.position.set(5, 10, 7);
        animScene.add(dir);

        // syringe (long thin)
        const syringeMat = new THREE.MeshStandardMaterial({ color: 0x88ccff, metalness:0.05, roughness:0.25, transparent:true, opacity:0.9 });
        const syringeGeo = new THREE.CylinderGeometry(0.28, 0.28, 8, 24);
        const syringe = new THREE.Mesh(syringeGeo, syringeMat);
        syringe.position.set(0, 8, -1.8);
        syringe.rotation.x = Math.PI * 0.05;
        animScene.add(syringe);

        // CaCl2 beaker - transparent cylinder (liquid visible)
        const beakerGeo = new THREE.CylinderGeometry(4.0, 4.0, 3.4, 36, 1, true);
        const beakerMat = new THREE.MeshStandardMaterial({ color:0x333333, transparent:true, opacity:0.15, side: THREE.DoubleSide });
        const beaker = new THREE.Mesh(beakerGeo, beakerMat);
        beaker.position.set(0, -1.4, 0);
        animScene.add(beaker);

        // liquid inside beaker (slightly glossy)
        const liquidGeo = new THREE.CylinderGeometry(3.75, 3.75, 2.2, 36);
        const liquidMat = new THREE.MeshStandardMaterial({ color: 0x66ccff, transparent:true, opacity:0.7, metalness:0.1, roughness:0.35 });
        const liquid = new THREE.Mesh(liquidGeo, liquidMat);
        liquid.position.set(0, -1.0, 0);
        animScene.add(liquid);

        // surface ripple (a subtle oscillation)
        let rippleTime = 0;

        // pearls array
        const pearls = [];
        const pearlGeo = new THREE.SphereGeometry(0.35, 28, 28);

        // splash particle system (small ephemeral particles)
        const splashes = [];

        // droplet timer
        let dropTicker = 0;

        function createDroplet() {
    const dropMat = new THREE.MeshStandardMaterial({ color:0xff9a66, metalness: 0.2, roughness:0.3, emissive:0x330000, emissiveIntensity:0.02 });
    const drop = new THREE.Mesh(new THREE.SphereGeometry(0.18, 16, 16), dropMat);
    // Drop from syringe needle tip (bottom of syringe)
    drop.position.set(0, 3.8, -1.8); // Aligned with syringe needle opening
    drop.userData = { 
        vy: 0.0, 
        state: 'falling',
        horizontalDrift: (Math.random() - 0.5) * 0.08 // Random horizontal movement
    };
    animScene.add(drop);
    return drop;
}

        // create initial droplets array
        const droplets = [];

        // ground plane (subtle)
        const groundGeo = new THREE.CircleGeometry(6, 64);
        const groundMat = new THREE.MeshStandardMaterial({ color:0x0b1220, roughness:1.0, metalness:0.0 });
        const ground = new THREE.Mesh(groundGeo, groundMat);
        ground.rotation.x = - Math.PI / 2;
        ground.position.y = -3.2;
        ground.visible = false;
        animScene.add(ground);

        // camera slight orbit
        let t = 0;

        function animate() {
            animId = requestAnimationFrame(animate);
            t += 0.016;
            rippleTime += 0.02;

            // periodic droplet creation
            dropTicker += 1;
            if (dropTicker % 40 === 0 && droplets.length < 18) {
                droplets.push(createDroplet());
            }

            // update droplets
      // update droplets
for (let i = droplets.length - 1; i >= 0; i--) {
    const d = droplets[i];
    d.userData.vy -= 0.018; // gravity
    d.position.y += d.userData.vy;
    // Add horizontal drift for natural spreading
    d.position.x += d.userData.horizontalDrift;
    d.position.z += (Math.random() - 0.5) * 0.01;
    d.rotation.x += 0.02;
    d.rotation.y += 0.01;
    // collision with liquid surface (y around -0.2)
    if (d.userData.state === 'falling' && d.position.y <= -0.2) {
        // create pearl (bigger) with spread-out positioning
        const pMat = new THREE.MeshStandardMaterial({ color:0xffcc66, metalness:0.2, roughness:0.25 });
        const p = new THREE.Mesh(pearlGeo, pMat);
        // Spread pearls across beaker bottom with random offset
        const spreadX = d.position.x + (Math.random() - 0.5) * 0.6;
        const spreadZ = d.position.z + (Math.random() - 0.5) * 0.6;
        p.position.set(spreadX, -0.5, spreadZ);
        p.scale.set(0.01,0.01,0.01);
        p.userData.settleX = spreadX; // Store final position
        p.userData.settleZ = spreadZ;
        animScene.add(p);
        pearls.push({ mesh: p, grow: 0.02 });     
                    // splash particles
                    for (let s = 0; s < 12; s++) {
                        const part = new THREE.Mesh(new THREE.SphereGeometry(0.04,8,8), new THREE.MeshStandardMaterial({ color:0x66ccff }));
                        part.position.copy(d.position);
                        const angle = Math.random() * Math.PI * 2;
                        const speed = Math.random() * 0.08 + 0.02;
                        part.userData = { vx: Math.cos(angle) * speed, vz: Math.sin(angle) * speed, vy: Math.random() * 0.08 + 0.02, life: 45 };
                        animScene.add(part);
                        splashes.push(part);
                    }

                    // remove droplet
                    animScene.remove(d);
                    droplets.splice(i,1);
                }

                // out of bounds safety
                if (d.position.y < -4 || Math.abs(d.position.x) > 12) {
                    try { animScene.remove(d); } catch(e){}
                    droplets.splice(i,1);
                }
            }

            // pearls growth and settling animation
            // pearls growth and settling animation
// pearls growth and settling animation with collision avoidance
for (let j = pearls.length - 1; j >= 0; j--) {
    const obj = pearls[j];
    if (obj.mesh.scale.x < 1.0) {
        const s = Math.min(1.0, obj.mesh.scale.x + obj.grow);
        obj.mesh.scale.set(s,s,s);
        obj.mesh.position.y = -0.5 - (1 - s) * 0.02;
        // Initialize movement velocities when pearl is fully grown
        if (s >= 1.0 && !obj.mesh.userData.vx) {
            obj.mesh.userData.vx = (Math.random() - 0.5) * 0.04;
            obj.mesh.userData.vz = (Math.random() - 0.5) * 0.04;
            obj.mesh.userData.targetY = -0.5;
        }
    } else {
        // Initialize velocities if not set
        if (!obj.mesh.userData.vx) {
            obj.mesh.userData.vx = (Math.random() - 0.5) * 0.04;
            obj.mesh.userData.vz = (Math.random() - 0.5) * 0.04;
            obj.mesh.userData.targetY = -0.5;
        }
        
        // Continuous random movement in solution
        obj.mesh.userData.vx += (Math.random() - 0.5) * 0.003;
        obj.mesh.userData.vz += (Math.random() - 0.5) * 0.003;
        
        // Damping to prevent excessive speed
        obj.mesh.userData.vx *= 0.98;
        obj.mesh.userData.vz *= 0.98;
        
        // Apply velocity
        obj.mesh.position.x += obj.mesh.userData.vx;
        obj.mesh.position.z += obj.mesh.userData.vz;
        
        // Keep pearls within beaker bounds (radius ~3.5)
        const distFromCenter = Math.sqrt(obj.mesh.position.x**2 + obj.mesh.position.z**2);
        if (distFromCenter > 3.2) {
            // Bounce back from wall
            const angle = Math.atan2(obj.mesh.position.z, obj.mesh.position.x);
            obj.mesh.position.x = Math.cos(angle) * 3.2;
            obj.mesh.position.z = Math.sin(angle) * 3.2;
            obj.mesh.userData.vx *= -0.7;
            obj.mesh.userData.vz *= -0.7;
        }
        
        // Collision avoidance with other pearls
        for (let k = 0; k < pearls.length; k++) {
            if (k === j) continue;
            const other = pearls[k].mesh;
            if (other.scale.x < 1.0) continue; // Skip growing pearls
            
            const dx = obj.mesh.position.x - other.position.x;
            const dz = obj.mesh.position.z - other.position.z;
            const dist = Math.sqrt(dx*dx + dz*dz);
            const minDist = 0.85; // Minimum distance between pearls
            
            if (dist < minDist && dist > 0) {
                // Push pearls apart
                const pushForce = (minDist - dist) * 0.15;
                const pushX = (dx / dist) * pushForce;
                const pushZ = (dz / dist) * pushForce;
                
                obj.mesh.position.x += pushX;
                obj.mesh.position.z += pushZ;
                obj.mesh.userData.vx += pushX * 0.3;
                obj.mesh.userData.vz += pushZ * 0.3;
            }
        }
        
        // Vertical bobbing motion
        obj.mesh.position.y = -0.5 + Math.sin(t * 2 + j * 0.5) * 0.015;
        
        // Rotation
        obj.mesh.rotation.y += 0.01;
        obj.mesh.rotation.x = Math.sin(t + j) * 0.05;
    }
}

            // update splashes
            for (let k = splashes.length - 1; k >= 0; k--) {
                const sp = splashes[k];
                sp.userData.vy -= 0.012;
                sp.position.x += sp.userData.vx;
                sp.position.z += sp.userData.vz;
                sp.position.y += sp.userData.vy;
                sp.userData.life--;
                sp.material.opacity = Math.max(0, sp.userData.life/45);
                if (sp.userData.life <= 0) {
                    animScene.remove(sp);
                    splashes.splice(k,1);
                }
            }

            // liquid surface subtle oscillation:
            liquid.position.y = -1.0 + Math.sin(rippleTime) * 0.006;

            // camera orbital motion
            animCamera.position.x = Math.sin(t * 0.12) * 12;
            animCamera.position.z = Math.cos(t * 0.12) * 12;
            animCamera.lookAt(0, 0, 0);

            animRenderer.render(animScene, animCamera);
        }

        animate();

        // resize handling
        window.addEventListener('resize', onAnimResize);

        function onAnimResize() {
            if (!animRenderer) return;
            const container = document.getElementById('canvas3d');
            animCamera.aspect = container.clientWidth / container.clientHeight;
            animCamera.updateProjectionMatrix();
            animRenderer.setSize(container.clientWidth, container.clientHeight);
        }
    }

    // ---------- Base (syrup) Animation ----------
    function initBaseAnimation() {
        const container = document.getElementById('canvas3d');
        animScene = new THREE.Scene();
        animScene.background = new THREE.Color(0x071428);

        animCamera = new THREE.PerspectiveCamera(50, container.clientWidth / container.clientHeight, 0.1, 1000);
        animCamera.position.set(10, 6, 14);
        animCamera.lookAt(0, 0, 0);

        animRenderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        animRenderer.setPixelRatio(window.devicePixelRatio);
        animRenderer.setSize(container.clientWidth, container.clientHeight);
        container.appendChild(animRenderer.domElement);

        // lights
        const ambient = new THREE.AmbientLight(0xffffff, 0.6);
        animScene.add(ambient);
        const dir = new THREE.DirectionalLight(0xffffff, 0.8);
        dir.position.set(10, 10, 5);
        animScene.add(dir);

        // beaker (transparent)
        const beaker = new THREE.Mesh(new THREE.CylinderGeometry(3.0, 3.0, 5.0, 48, 1, true), new THREE.MeshStandardMaterial({ color:0xffffff, transparent:true, opacity:0.1, side:THREE.DoubleSide }));
        beaker.position.set(0, 0.5, 0);
        animScene.add(beaker);

        // syrup inside: using a slightly wavy plane approximation (rotating cylinder)
        const syrup = new THREE.Mesh(new THREE.CylinderGeometry(2.6, 2.6, 3.6, 64), new THREE.MeshStandardMaterial({ color:0xffa84d, transparent:true, opacity:0.85, roughness:0.3, metalness:0.1 }));
        syrup.position.set(0, -0.2, 0);
        animScene.add(syrup);

        // stirrer
        const stir = new THREE.Mesh(new THREE.CylinderGeometry(0.08, 0.08, 6, 12), new THREE.MeshStandardMaterial({ color:0x222222 }));
        stir.position.set(0, 1.4, 0.3);
        animScene.add(stir);

        // ingredient particles swirling (visual)
        const particles = [];
        const pGeo = new THREE.SphereGeometry(0.09, 8, 8);
        const colors = [0x2ecc71,0xe74c3c,0xf39c12,0x9b59b6,0x3498db];
        for (let i=0;i<45;i++){
            const pMat = new THREE.MeshStandardMaterial({ color: colors[i % colors.length] });
            const p = new THREE.Mesh(pGeo, pMat);
            const ang = Math.random()*Math.PI*2;
            const r = 1.0 + Math.random()*1.4;
            p.position.set(Math.cos(ang)*r, -0.4 + Math.random()*0.8, Math.sin(ang)*r);
            p.userData = { angle: ang, radius: r, speed: 0.01 + Math.random()*0.03 }
            animScene.add(p);
            particles.push(p);
        }

        // coated pearls dropping into syrup
        const pearls = [];
       

        let tt = 0;

        function animate() {
            animId = requestAnimationFrame(animate);
            tt += 0.016;

            // stirrer rotation
            stir.rotation.y += 0.08;

            // particles spiral
            particles.forEach(p => {
                p.userData.angle += p.userData.speed;
                p.position.x = Math.cos(p.userData.angle) * p.userData.radius;
                p.position.z = Math.sin(p.userData.angle) * p.userData.radius;
                p.position.y = -0.4 + Math.sin(tt*2 + p.userData.angle) * 0.2;
            });

            // pearls fall and settle
           

            // camera slow orbit
            animCamera.position.x = Math.sin(tt * 0.1) * 10;
            animCamera.position.z = Math.cos(tt * 0.1) * 10;
            animCamera.lookAt(0, 0, 0);

            animRenderer.render(animScene, animCamera);
        }
        animate();

        window.addEventListener('resize', onAnimResizeBase);
        function onAnimResizeBase(){
            if (!animRenderer) return;
            const c = document.getElementById('canvas3d');
            animCamera.aspect = c.clientWidth / c.clientHeight;
            animCamera.updateProjectionMatrix();
            animRenderer.setSize(c.clientWidth, c.clientHeight);
        }
    }

    // ---------- Coating Animation (layer-by-layer) ----------
    function initCoatingAnimation() {
        const container = document.getElementById('canvas3d');
        animScene = new THREE.Scene();
        animScene.background = new THREE.Color(0x081018);

        animCamera = new THREE.PerspectiveCamera(50, container.clientWidth / container.clientHeight, 0.1, 1000);
        animCamera.position.set(0, 1.6, 6);
        animCamera.lookAt(0, 0, 0);

        animRenderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        animRenderer.setPixelRatio(window.devicePixelRatio);
        animRenderer.setSize(container.clientWidth, container.clientHeight);
        container.appendChild(animRenderer.domElement);

        // lights
        const amb = new THREE.AmbientLight(0xffffff, 0.6);
        animScene.add(amb);
        const key = new THREE.DirectionalLight(0xffffff, 0.8);
        key.position.set(5,8,5);
        animScene.add(key);

        // core pearl
        const coreGeo = new THREE.SphereGeometry(1.1, 64, 64);
        const coreMat = new THREE.MeshStandardMaterial({ color: 0xffcc66, roughness:0.25, metalness:0.1 });
        const core = new THREE.Mesh(coreGeo, coreMat);
        animScene.add(core);

        // coating layers array
        const layers = [];
        let addLayerTick = 0;

        // simulate spraying particles that deposit
        const sprayGroup = new THREE.Group();
        animScene.add(sprayGroup);

        let time = 0;

        function addCoatingLayer() {
            const radius = 1.1 + layers.length * 0.12;
            const layerGeo = new THREE.SphereGeometry(radius, 64, 64);
            const layerMat = new THREE.MeshStandardMaterial({ color: 0x9966ff, transparent:true, opacity:0.0, roughness:0.2, metalness:0.15 });
            const layer = new THREE.Mesh(layerGeo, layerMat);
            layer.scale.set(0.01,0.01,0.01);
            animScene.add(layer);
            layers.push(layer);
        }

        // start with zero layers
        addCoatingLayer();

        function animate() {
            animId = requestAnimationFrame(animate);
            time += 0.016;
            addLayerTick++;

            // occasionally add new layer (slower)
            if (addLayerTick % 160 === 0 && layers.length < 6) {
                addCoatingLayer();
            }

            // grow latest layer and fade in
            layers.forEach((layer, idx) => {
                const targetScale = 1;
                if (layer.scale.x < targetScale) {
                    const s = Math.min(targetScale, layer.scale.x + 0.015);
                    layer.scale.set(s,s,s);
                    layer.material.opacity = Math.min(0.6, layer.material.opacity + 0.02);
                } else {
                    // slight breathe
                    const breathe = 1 + Math.sin(time*2 + idx) * 0.005;
                    layer.scale.set(breathe,breathe,breathe);
                }
            });

            // spray particle effect near one side (visual)
            if (Math.random() < 0.35) {
                const part = new THREE.Mesh(new THREE.SphereGeometry(0.02,6,6), new THREE.MeshStandardMaterial({ color:0x9966ff }));
                part.position.set(Math.random()*0.6 - 0.3, 1.8, Math.random()*0.6 - 0.3);
                part.userData = { vx: (Math.random()-0.5)*0.02, vy: -0.03 - Math.random()*0.02, life: 40 };
                sprayGroup.add(part);
            }

            // update spray particles
            for (let i = sprayGroup.children.length -1; i >=0; i--) {
                const p = sprayGroup.children[i];
                p.position.x += p.userData.vx;
                p.position.y += p.userData.vy;
                p.userData.life--;
                if (p.userData.life <=0) sprayGroup.remove(p);
            }

            // rotate core slowly to show layering
            core.rotation.y += 0.006;

            animCamera.position.x = Math.sin(time*0.2) * 1.8;
            animCamera.lookAt(0, 0, 0);

            animRenderer.render(animScene, animCamera);
        }

        animate();

        window.addEventListener('resize', onAnimResizeCoating);
        function onAnimResizeCoating(){
            if (!animRenderer) return;
            const c = document.getElementById('canvas3d');
            animCamera.aspect = c.clientWidth / c.clientHeight;
            animCamera.updateProjectionMatrix();
            animRenderer.setSize(c.clientWidth, c.clientHeight);
        }
    }

    // -----------------------
    // Flow-step click wiring already attached earlier (attachFlowStepHandlers)
    // -----------------------

    // -----------------------
    // Fireworks for Slide 19
    // -----------------------
    let fwRenderer, fwScene, fwCamera, fwId, fwParticles = [];
    let fwRunning = false;

    function startFireworks() {
        if (fwRunning) return;
        fwRunning = true;

        const canvas = document.getElementById('fireworks-canvas');
        canvas.width = canvas.clientWidth;
        canvas.height = canvas.clientHeight;

        // create basic Canvas 2D fireworks (more robust cross-browser)
        const ctx = canvas.getContext('2d');

        // particle arrays
        const bursts = [];

        function randomColor() {
            const palette = ['#ff4757','#ffa502','#ff6b81','#7bed9f','#70a1ff','#ffee93'];
            return palette[Math.floor(Math.random()*palette.length)];
        }

        function spawnBurst() {
            const x = Math.random() * canvas.width * 0.9 + canvas.width*0.05;
            const y = Math.random() * canvas.height * 0.6 + canvas.height*0.1;
            const color = randomColor();
            const count = 30 + Math.floor(Math.random()*40);
            const burst = { x, y, color, particles: [], life: 80 };
            for (let i=0;i<count;i++){
                const angle = Math.random()*Math.PI*2;
                const speed = 1 + Math.random()*4;
                burst.particles.push({
                    x, y,
                    vx: Math.cos(angle)*speed,
                    vy: Math.sin(angle)*speed,
                    alpha:1,
                    size: 1 + Math.random()*2
                });
            }
            bursts.push(burst);
        }

        function animateFW() {
            fwId = requestAnimationFrame(animateFW);
            ctx.clearRect(0,0,canvas.width,canvas.height);

            // occasional spawns
            if (Math.random() < 0.04) spawnBurst();

            for (let i=bursts.length-1;i>=0;i--){
                const b = bursts[i];
                b.life--;
                b.particles.forEach(p=>{
                    p.vy += 0.06; // gravity down
                    p.x += p.vx;
                    p.y += p.vy;
                    p.alpha *= 0.995;
                    // draw
                    ctx.globalAlpha = Math.max(0, p.alpha);
                    ctx.fillStyle = b.color;
                    ctx.beginPath();
                    ctx.arc(p.x, p.y, p.size, 0, Math.PI*2);
                    ctx.fill();
                });
                if (b.life <=0) bursts.splice(i,1);
            }
            ctx.globalAlpha = 1;
        }

        // handle canvas resize
        function onFWResize() {
            canvas.width = canvas.clientWidth;
            canvas.height = canvas.clientHeight;
        }
        window.addEventListener('resize', onFWResize);
        animateFW();
        // store small cleanup reference
        fwParticles = { cancel: () => { cancelAnimationFrame(fwId); window.removeEventListener('resize', onFWResize); } };
    }

    function stopFireworks() {
        if (!fwRunning) return;
        fwRunning = false;
        if (fwParticles && fwParticles.cancel) fwParticles.cancel();
        const canvas = document.getElementById('fireworks-canvas');
        const ctx = canvas.getContext('2d');
        ctx && ctx.clearRect && ctx.clearRect(0,0,canvas.width,canvas.height);
    }

    // Close animation modal (overlay click handler)
    function closeAnimation() {
        closeAnimationModal();
    }

    // Expose to HTML onclick via window (the markup calls closeAnimationModal())
    window.closeAnimationModal = closeAnimationModal;

    // ensure clicking animation modal overlay closes it
    const animModalEl = document.getElementById('animationModal');
    if (animModalEl) {
        animModalEl.addEventListener('click', () => {
            if (animModalEl.classList.contains('active')) closeAnimationModal();
        });
    }

    </script>
</body>
</html>

