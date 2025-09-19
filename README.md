<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gioco di Vocabolario Italiano</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #6a93cb 0%, #a4bfef 100%);
            min-height: 100vh;
            padding: 20px;
            color: #2c3e50;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            max-width: 1000px;
            width: 100%;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 10px;
            padding: 20px;
            background: #f8f9fa;
            flex-wrap: wrap;
        }

        .nav-btn {
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(74, 111, 165, 0.4);
        }

        .nav-btn:not(.active) {
            background: white;
            color: #666;
        }

        .nav-btn:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-1px);
        }

        .game-section {
            display: none;
            padding: 30px;
            min-height: 400px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #e8f4f2);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            border: 2px solid #4a6fa5;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .word-bank h3 {
            color: #4a6fa5;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.4em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 10px 18px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 16px;
            box-shadow: 0 3px 10px rgba(74, 111, 165, 0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(74, 111, 165, 0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 20px;
            border-left: 5px solid #4a6fa5;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }

        .question h3 {
            color: #4a6fa5;
            margin-bottom: 15px;
            font-size: 1.3em;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 8px;
            padding: 8px 12px;
            font-size: 16px;
            margin: 0 5px;
            min-width: 120px;
            transition: border-color 0.3s ease;
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .option {
            padding: 15px 20px;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .option:hover {
            border-color: #4a6fa5;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .option.selected {
            background: #4a6fa5;
            color: white;
            border-color: #4a6fa5;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-top: 20px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 15px;
            color: #4a6fa5;
            font-size: 1.2em;
        }

        .match-item {
            padding: 15px;
            margin: 8px 0;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .match-item:hover {
            border-color: #4a6fa5;
            transform: translateY(-1px);
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .match-item.selected {
            background: #e8f4f2;
            border-color: #4a6fa5;
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
        }

        .check-btn {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 20px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.3);
        }

        .check-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(74, 111, 165, 0.4);
        }

        .feedback {
            margin-top: 15px;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 15px 20px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.3);
            z-index: 1000;
        }

        .icon {
            font-size: 24px;
            margin-right: 10px;
            vertical-align: middle;
        }

        /* Vocabulary List Styles */
        .vocabulary-list {
            padding: 20px;
        }

        .vocabulary-item {
            margin-bottom: 25px;
            padding: 20px;
            border-radius: 10px;
            background: #f8f9fa;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
        }

        .vocabulary-item h3 {
            color: #4a6fa5;
            margin-bottom: 10px;
            font-size: 1.4em;
            border-bottom: 2px solid #4a6fa5;
            padding-bottom: 8px;
        }

        .vocabulary-item p {
            margin-bottom: 8px;
            line-height: 1.6;
        }

        .example {
            font-style: italic;
            color: #555;
            padding-left: 15px;
            border-left: 3px solid #4a6fa5;
            margin-top: 10px;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 20px;
            }
            
            .nav-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .nav-btn {
                width: 200px;
            }
            
            .score {
                position: static;
                margin: 20px auto;
                display: block;
                width: fit-content;
            }
        }
    </style>
</head>
<body>
    <div class="score">Punteggio: <span id="score">0</span>/12</div>
    
    <div class="container">
        <div class="header">
            <h1><i class="fas fa-book icon"></i> Gioco di Vocabolario Italiano</h1>
            <p>Metti alla prova la tua conoscenza di queste parole italiane!</p>
        </div>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showSection('vocabulary')">Lista Vocabolario</button>
            <button class="nav-btn" onclick="showSection('fill-gaps')">Completa le Frasi</button>
            <button class="nav-btn" onclick="showSection('matching')">Abbina le Definizioni</button>
            <button class="nav-btn" onclick="showSection('multiple-choice')">Scelta Multipla</button>
        </div>

        <!-- Vocabulary List Section -->
        <div id="vocabulary" class="game-section active">
            <h2 style="text-align: center; margin-bottom: 20px; color: #4a6fa5;">Spiegazioni del Vocabolario</h2>
            
            <div class="vocabulary-list">
                <div class="vocabulary-item">
                    <h3>sempre</h3>
                    <p><strong>Definizione:</strong> Avverbio che indica continuità, persistenza o regolarità.</p>
                    <p><strong>Uso:</strong> Usato per esprimere qualcosa che avviene in ogni occasione o senza eccezioni.</p>
                    <p class="example"><strong>Esempio:</strong> "Vado sempre in palestra il martedì."</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>forse</h3>
                    <p><strong>Definizione:</strong> Avverbio che esprime incertezza, dubbio o possibilità.</p>
                    <p><strong>Uso:</strong> Usato quando non si è sicuri di qualcosa o per esprimere una probabilità.</p>
                    <p class="example"><strong>Esempio:</strong> "Forse domani pioverà."</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>abbastanza</h3>
                    <p><strong>Definizione:</strong> Avverbio che indica una quantità o grado sufficiente, ma non eccedente.</p>
                    <p><strong>Uso:</strong> Usato per esprimere sufficienza o moderazione.</p>
                    <p class="example"><strong>Esempio:</strong> "Ho mangiato abbastanza, grazie."</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>porte</h3>
                    <p><strong>Definizione:</strong> Sostantivo plurale che indica gli accessi a edifici, stanze o mobili.</p>
                    <p><strong>Uso:</strong> Usato per riferirsi a entrate, uscite o aperture.</p>
                    <p class="example"><strong>Esempio:</strong> "Tutte le porte dell'hotel erano blu."</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>anni fa</h3>
                    <p><strong>Definizione:</strong> Espressione temporale che indica un momento nel passato.</p>
                    <p><strong>Uso:</strong> Usato per riferirsi a eventi accaduti in un passato non recente.</p>
                    <p class="example"><strong>Esempio:</strong> "Mi sono trasferito a Roma cinque anni fa."</p>
                </div>
            </div>
        </div>

        <!-- Fill in the Gaps Section -->
        <div id="fill-gaps" class="game-section">
            <div class="word-bank">
                <h3><i class="fas fa-list-ul icon"></i> Parole da usare:</h3>
                <div class="word-options">
                    <span class="word-option">sempre</span>
                    <span class="word-option">forse</span>
                    <span class="word-option">abbastanza</span>
                    <span class="word-option">porte</span>
                    <span class="word-option">anni fa</span>
                </div>
            </div>

            <!-- FRASI IN ITALIANO -->
            <div class="question">
                <h3>Domanda 1:</h3>
                <p>Vado <input type="text" class="fill-blank" data-answer="sempre" placeholder="risposta"> in palestra il martedì.</p>
                <div class="feedback" id="feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 2:</h3>
                <p><input type="text" class="fill-blank" data-answer="forse" placeholder="risposta"> domani pioverà, non sono sicuro.</p>
                <div class="feedback" id="feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 3:</h3>
                <p>Ho mangiato <input type="text" class="fill-blank" data-answer="abbastanza" placeholder="risposta">, non posso prendere il dessert.</p>
                <div class="feedback" id="feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 4:</h3>
                <p>Le <input type="text" class="fill-blank" data-answer="porte" placeholder="risposta"> dell'antico castello erano molto pesanti.</p>
                <div class="feedback" id="feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 5:</h3>
                <p>Mi sono laureato tre <input type="text" class="fill-blank" data-answer="anni fa" placeholder="risposta">.</p>
                <div class="feedback" id="feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 6:</h3>
                <p>Non esco <input type="text" class="fill-blank" data-answer="sempre" placeholder="risposta"> la sera, solo occasionalmente.</p>
                <div class="feedback" id="feedback-6" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 7:</h3>
                <p><input type="text" class="fill-blank" data-answer="forse" placeholder="risposta"> verrò alla tua festa, dipende dal lavoro.</p>
                <div class="feedback" id="feedback-7" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 8:</h3>
                <p>Questo vestito è <input type="text" class="fill-blank" data-answer="abbastanza" placeholder="risposta"> elegante per la cerimonia.</p>
                <div class="feedback" id="feedback-8" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 9:</h3>
                <p>Dovresti chiudere le <input type="text" class="fill-blank" data-answer="porte" placeholder="risposta"> prima di andare a dormire.</p>
                <div class="feedback" id="feedback-9" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 10:</h3>
                <p>Ho visitato Parigi molti <input type="text" class="fill-blank" data-answer="anni fa" placeholder="risposta">.</p>
                <div class="feedback" id="feedback-10" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 11:</h3>
                <p>Marco è <input type="text" class="fill-blank" data-answer="sempre" placeholder="risposta"> di buonumore al mattino.</p>
                <div class="feedback" id="feedback-11" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 12:</h3>
                <p>Quel ristorante ha chiuso due <input type="text" class="fill-blank" data-answer="anni fa" placeholder="risposta">.</p>
                <div class="feedback" id="feedback-12" style="display: none;"></div>
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Controlla Risposte</button>
        </div>

        <!-- Matching Section -->
        <div id="matching" class="game-section">
            <h2 style="text-align: center; margin-bottom: 20px; color: #4a6fa5;">Abbina le parole con le loro definizioni</h2>
            <div class="matching-container">
                <div class="match-column">
                    <h4>Parole</h4>
                    <div class="match-item" data-word="sempre" onclick="selectMatch(this)">sempre</div>
                    <div class="match-item" data-word="forse" onclick="selectMatch(this)">forse</div>
                    <div class="match-item" data-word="abbastanza" onclick="selectMatch(this)">abbastanza</div>
                    <div class="match-item" data-word="porte" onclick="selectMatch(this)">porte</div>
                    <div class="match-item" data-word="anni fa" onclick="selectMatch(this)">anni fa</div>
                </div>
                <div class="match-column">
                    <h4>Definizioni</h4>
                    <div id="meanings-container">
                        <!-- Meanings will be dynamically inserted here -->
                    </div>
                </div>
            </div>
            <div class="feedback" id="matching-feedback" style="display: none;"></div>
            <button class="check-btn" onclick="checkMatching()">Controlla Abbinamenti</button>
        </div>

        <!-- Multiple Choice Section -->
        <div id="multiple-choice" class="game-section">
            <div class="question">
                <h3>Domanda 1: Cosa significa "sempre"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Mai</div>
                    <div class="option" onclick="selectOption(this, true)">In ogni occasione, senza eccezioni</div>
                    <div class="option" onclick="selectOption(this, false)">Qualche volta</div>
                    <div class="option" onclick="selectOption(this, false)">Dopo</div>
                </div>
                <div class="feedback" id="mc-feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 2: "Forse" è usato per esprimere:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Certezza</div>
                    <div class="option" onclick="selectOption(this, true)">Incertezza o possibilità</div>
                    <div class="option" onclick="selectOption(this, false)">Una quantità sufficiente</div>
                    <div class="option" onclick="selectOption(this, false)">Un momento nel passato</div>
                </div>
                <div class="feedback" id="mc-feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 3: Cosa indica "abbastanza"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Troppo</div>
                    <div class="option" onclick="selectOption(this, true)">Una quantità o grado sufficiente</div>
                    <div class="option" onclick="selectOption(this, false)">Poco</div>
                    <div class="option" onclick="selectOption(this, false)">Niente</div>
                </div>
                <div class="feedback" id="mc-feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 4: Cosa sono le "porte"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Finestre</div>
                    <div class="option" onclick="selectOption(this, true)">Accessi a edifici, stanze o mobili</div>
                    <div class="option" onclick="selectOption(this, false)">Tipi di animali</div>
                    <div class="option" onclick="selectOption(this, false)">Vestiti</div>
                </div>
                <div class="feedback" id="mc-feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 5: "Anni fa" indica:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Il presente</div>
                    <div class="option" onclick="selectOption(this, false)">Il futuro</div>
                    <div class="option" onclick="selectOption(this, true)">Un momento nel passato</div>
                    <div class="option" onclick="selectOption(this, false)">Un'era geologica</div>
                </div>
                <div class="feedback" id="mc-feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 6: Quale parola esprime dubbio?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">sempre</div>
                    <div class="option" onclick="selectOption(this, false)">abbastanza</div>
                    <div class="option" onclick="selectOption(this, true)">forse</div>
                    <div class="option" onclick="selectOption(this, false)">porte</div>
                </div>
                <div class="feedback" id="mc-feedback-6" style="display: none;"></div>
            </div>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];
        
        // Definitions for the matching game
        const definitions = [
            { meaning: "sempre", text: "In ogni occasione, senza eccezioni" },
            { meaning: "forse", text: "Esprime incertezza o possibilità" },
            { meaning: "abbastanza", text: "Una quantità o grado sufficiente" },
            { meaning: "porte", text: "Accessi a edifici, stanze o mobili" },
            { meaning: "anni fa", text: "Un momento nel passato" }
        ];

        // Function to shuffle array (Fisher-Yates algorithm)
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // Initialize the matching game with shuffled definitions
        function initMatchingGame() {
            const meaningsContainer = document.getElementById('meanings-container');
            meaningsContainer.innerHTML = '';
            
            // Shuffle the definitions
            const shuffledDefinitions = shuffleArray([...definitions]);
            
            // Create and append the definition elements
            shuffledDefinitions.forEach(def => {
                const div = document.createElement('div');
                div.className = 'match-item';
                div.setAttribute('data-meaning', def.meaning);
                div.onclick = function() { selectMatch(this); };
                div.textContent = def.text;
                meaningsContainer.appendChild(div);
            });
        }

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.add('active');
            
            // Add active class to clicked button
            event.target.classList.add('active');
            
            // If showing matching section, reinitialize with shuffled definitions
            if (sectionId === 'matching') {
                initMatchingGame();
                // Reset matching game state
                document.querySelectorAll('.match-item').forEach(item => {
                    item.classList.remove('selected', 'matched');
                });
                selectedWord = null;
                selectedMeaning = null;
                matchedPairs = [];
                document.getElementById('matching-feedback').style.display = 'none';
            }
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank, index) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
                
                // Show feedback for each question
                const feedback = document.getElementById(`feedback-${index+1}`);
                if (userAnswer === correctAnswer) {
                    feedback.textContent = '✅ Corretto!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Sbagliato. La risposta corretta è: "${blank.dataset.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            });
            
            updateScore();
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('matching-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Abbinamento corretto!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === definitions.length) {
                    feedback.textContent = '🎉 Complimenti! Hai abbinato tutti i termini correttamente!';
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Abbinamento errato. Riprova.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
            
            updateScore();
        }

        function checkMatching() {
            const allMatches = document.querySelectorAll('.match-item[data-word]');
            let allCorrect = true;
            
            allMatches.forEach(item => {
                if (!item.classList.contains('matched')) {
                    allCorrect = false;
                }
            });
            
            const feedback = document.getElementById('matching-feedback');
            if (allCorrect) {
                feedback.textContent = '🎉 Eccellente! Tutti gli abbinamenti sono corretti!';
                feedback.className = 'feedback correct';
            } else {
                feedback.textContent = 'Continua a provare! Alcuni abbinamenti sono ancora errati.';
                feedback.className = 'feedback incorrect';
            }
            feedback.style.display = 'block';
        }

        function selectOption(element, isCorrect) {
            // Remove selection from all options in this question
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
                opt.classList.remove('correct');
                opt.classList.remove('incorrect');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            const questionNumber = element.closest('.question').querySelector('h3').textContent.match(/\d+/)[0];
            const feedback = document.getElementById(`mc-feedback-${questionNumber}`);
            
            if (isCorrect) {
                element.classList.add('correct');
                feedback.textContent = '✅ Corretto!';
                feedback.className = 'feedback correct';
            } else {
                element.classList.add('incorrect');
                feedback.textContent = '❌ Sbagliato. Riprova.';
                feedback.className = 'feedback incorrect';
                
                // Show the correct answer
                options.forEach(opt => {
                    if (opt.onclick.toString().includes('true')) {
                        opt.classList.add('correct');
                    }
                });
            }
            
            feedback.style.display = 'block';
            updateScore();
        }

        function updateScore() {
            // Calculate score for fill-in-the-blank
            let fillScore = 0;
            document.querySelectorAll('.fill-blank.correct').forEach(blank => {
                if (!blank.classList.contains('counted')) {
                    fillScore++;
                    blank.classList.add('counted');
                }
            });
            
            // Calculate score for matching (each pair is 1 point)
            const matchScore = matchedPairs.length;
            
            // Calculate score for multiple choice (each correct is 1 point)
            let mcScore = 0;
            document.querySelectorAll('.option.correct.selected').forEach(opt => {
                mcScore++;
            });
            
            // Total score
            score = fillScore + matchScore + mcScore;
            document.getElementById('score').textContent = score;
        }
        
        // Initialize the page
        window.onload = function() {
            initMatchingGame();
        }
    </script>
</body>
</html>
