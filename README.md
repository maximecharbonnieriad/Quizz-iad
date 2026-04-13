<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Agent Immobilier iad - Maxime Charbonnier</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        
        .quiz-container {
            background: white;
            border-radius: 16px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            max-width: 650px;
            width: 100%;
            padding: 40px;
        }
        
        .header {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .header h1 {
            font-size: 28px;
            color: #1a202c;
            margin-bottom: 10px;
        }
        
        .header p {
            font-size: 15px;
            color: #718096;
            line-height: 1.6;
        }
        
        .progress-bar {
            width: 100%;
            height: 6px;
            background: #e2e8f0;
            border-radius: 3px;
            margin-bottom: 30px;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea, #764ba2);
            transition: width 0.3s ease;
        }
        
        .question-section {
            margin-bottom: 25px;
            display: none;
        }
        
        .question-section.active {
            display: block;
            animation: fadeIn 0.3s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .question-number {
            font-size: 12px;
            color: #a0aec0;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 8px;
        }
        
        .question-text {
            font-size: 18px;
            font-weight: 600;
            color: #1a202c;
            margin-bottom: 20px;
            line-height: 1.5;
        }
        
        .options {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        
        .option {
            position: relative;
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 14px;
            background: #f7fafc;
            border: 1.5px solid #e2e8f0;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .option:hover {
            background: #edf2f7;
            border-color: #cbd5e0;
        }
        
        .option input[type="radio"] {
            cursor: pointer;
            width: 18px;
            height: 18px;
            accent-color: #667eea;
        }
        
        .option label {
            flex: 1;
            cursor: pointer;
            font-size: 15px;
            color: #2d3748;
        }
        
        .option input[type="radio"]:checked ~ label {
            font-weight: 600;
            color: #667eea;
        }
        
        .option input[type="radio"]:checked {
            accent-color: #667eea;
        }
        
        .form-section {
            background: #f7fafc;
            padding: 20px;
            border-radius: 8px;
            margin-bottom: 25px;
            display: none;
        }
        
        .form-section.active {
            display: block;
        }
        
        .form-section h3 {
            font-size: 16px;
            font-weight: 600;
            color: #1a202c;
            margin-bottom: 15px;
        }
        
        .form-group {
            margin-bottom: 12px;
        }
        
        .form-group label {
            display: block;
            font-size: 13px;
            color: #4a5568;
            margin-bottom: 6px;
            font-weight: 500;
        }
        
        .form-group input {
            width: 100%;
            padding: 10px 12px;
            font-size: 14px;
            border: 1px solid #cbd5e0;
            border-radius: 6px;
            font-family: inherit;
        }
        
        .form-group input:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }
        
        .button-group {
            display: flex;
            gap: 12px;
            margin-bottom: 15px;
        }
        
        button {
            flex: 1;
            padding: 12px;
            font-size: 15px;
            font-weight: 600;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .btn-secondary {
            background: #e2e8f0;
            color: #2d3748;
        }
        
        .btn-secondary:hover {
            background: #cbd5e0;
        }
        
        .btn-primary {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
        }
        
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
        }
        
        .btn-primary:active {
            transform: translateY(0);
        }
        
        .hidden {
            display: none !important;
        }
        
        .results-section {
            background: #f0f4ff;
            border: 2px solid #667eea;
            border-radius: 12px;
            padding: 30px;
            text-align: center;
            margin-bottom: 20px;
            display: none;
        }
        
        .results-section.active {
            display: block;
        }
        
        .results-section h2 {
            font-size: 24px;
            font-weight: 700;
            color: #1a202c;
            margin-bottom: 12px;
        }
        
        .score-badge {
            display: inline-block;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 10px 24px;
            border-radius: 20px;
            font-weight: 600;
            margin: 12px 0;
            font-size: 16px;
        }
        
        .results-section p {
            font-size: 15px;
            color: #4a5568;
            line-height: 1.7;
            margin-bottom: 12px;
        }
        
        .cta-box {
            background: #48bb78;
            color: white;
            padding: 16px;
            border-radius: 8px;
            margin: 20px 0;
            font-weight: 600;
        }
        
        .cta-box a {
            color: white;
            text-decoration: underline;
        }
        
        .step-indicator {
            font-size: 12px;
            color: #a0aec0;
            text-align: center;
            margin-top: 10px;
        }
        
        .loading {
            text-align: center;
            color: #667eea;
            font-weight: 600;
        }
        
        .error {
            background: #fed7d7;
            color: #c53030;
            padding: 12px;
            border-radius: 6px;
            margin-bottom: 15px;
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <div class="header">
            <h1>Rejoindre le réseau immobilier N°1</h1>
            <p style="font-size: 12px; color: #a0aec0; margin-bottom: 8px;">IAD Immobilier</p>
            <p>Réponds à ce quizz pour que j'en sache un peu plus sur toi 😊</p>
        </div>
        
        <div class="progress-bar">
            <div class="progress-fill" id="progressFill"></div>
        </div>
        
        <div id="error" class="error hidden"></div>
        
        <form id="quizForm">
            <!-- QUESTIONS SUR L'ÉTAT D'ESPRIT & MOTIVATION -->
            
            <div class="question-section active" data-question="1">
                <div class="question-number">Question 1/12</div>
                <div class="question-text">Pourquoi veux-tu devenir mandataire immobilier ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q1" value="3">
                        <label style="margin: 0;">Liberté financière et indépendance totale</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q1" value="2">
                        <label style="margin: 0;">Gagner plus tout en restant flexible</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q1" value="1">
                        <label style="margin: 0;">J'hésite encore, c'est une opportunité parmi d'autres</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="2">
                <div class="question-number">Question 2/12</div>
                <div class="question-text">Quel est ton état d'esprit face aux défis et à l'échec ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q2" value="3">
                        <label style="margin: 0;">J'apprends de mes échecs et je persévère</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q2" value="2">
                        <label style="margin: 0;">J'essaie mais j'ai besoin de soutien</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q2" value="1">
                        <label style="margin: 0;">Je préfère éviter les risques et l'incertitude</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="3">
                <div class="question-number">Question 3/12</div>
                <div class="question-text">Qu'est-ce qui te motive vraiment au travail ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q3" value="3">
                        <label style="margin: 0;">Créer quelque chose de personnel et être responsable</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q3" value="2">
                        <label style="margin: 0;">Gagner de l'argent avec un bon équilibre</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q3" value="1">
                        <label style="margin: 0;">Avoir un travail sympa sans trop de stress</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="4">
                <div class="question-number">Question 4/12</div>
                <div class="question-text">Comment vois-tu le rapport entre travail et revenus ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q4" value="3">
                        <label style="margin: 0;">Plus j'en fais, plus je gagne - j'aime ce lien</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q4" value="2">
                        <label style="margin: 0;">Oui mais j'aime aussi la sécurité d'une base</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q4" value="1">
                        <label style="margin: 0;">Je préfère un salaire fixe indépendant de ma performance</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="5">
                <div class="question-number">Question 5/12</div>
                <div class="question-text">Quel type d'environnement te rend le plus performant ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q5" value="3">
                        <label style="margin: 0;">Seul avec mes objectifs clairs et ma vision</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q5" value="2">
                        <label style="margin: 0;">Un mix entre autonomie et cadre structuré</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q5" value="1">
                        <label style="margin: 0;">Dans une équipe avec des règles et de la supervision</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="6">
                <div class="question-number">Question 6/12</div>
                <div class="question-text">À quel moment veux-tu réussir financièrement ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q6" value="3">
                        <label style="margin: 0;">Rapidement - je suis prêt à tout donner maintenant</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q6" value="2">
                        <label style="margin: 0;">Dans 2-3 ans avec du travail régulier</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q6" value="1">
                        <label style="margin: 0;">À long terme, je ne suis pas pressé</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="7">
                <div class="question-number">Question 7/12</div>
                <div class="question-text">Comment réagis-tu face à des critiques ou des "non" ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q7" value="3">
                        <label style="margin: 0;">Je les utilise comme du feedback pour m'améliorer</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q7" value="2">
                        <label style="margin: 0;">Ça m'affecte un peu mais je rebondis</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q7" value="1">
                        <label style="margin: 0;">C'est difficile pour moi, ça me décourage</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="8">
                <div class="question-number">Question 8/12</div>
                <div class="question-text">Qu'est-ce qui te bloque le plus pour lancer un projet entrepreneurial ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q8" value="3">
                        <label style="margin: 0;">Rien - j'ai juste besoin de la bonne structure</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q8" value="2">
                        <label style="margin: 0;">Quelques doutes, mais je peux les surmonter</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q8" value="1">
                        <label style="margin: 0;">La peur de l'échec, l'incertitude financière</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="9">
                <div class="question-number">Question 9/12</div>
                <div class="question-text">Comment définis-tu le succès pour toi ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q9" value="3">
                        <label style="margin: 0;">Bâtir quelque chose de durable et rentable</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q9" value="2">
                        <label style="margin: 0;">Gagner bien tout en gardant du temps libre</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q9" value="1">
                        <label style="margin: 0;">Avoir un travail sympa sans trop de stress</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="10">
                <div class="question-number">Question 10/12</div>
                <div class="question-text">Crois-tu vraiment que tu peux réussir dans cette aventure ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q10" value="3">
                        <label style="margin: 0;">Oui, j'ai une conviction profonde</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q10" value="2">
                        <label style="margin: 0;">Je l'espère, avec le bon support</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q10" value="1">
                        <label style="margin: 0;">Je ne suis pas sûr, j'ai beaucoup de doutes</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="11">
                <div class="question-number">Question 11/12</div>
                <div class="question-text">Quel est ton rapport à la vision long terme ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q11" value="3">
                        <label style="margin: 0;">J'ai une vision claire de mes 5 prochaines années</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q11" value="2">
                        <label style="margin: 0;">Je sais où je veux aller globalement</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q11" value="1">
                        <label style="margin: 0;">Je préfère vivre au jour le jour</label>
                    </label>
                </div>
            </div>

            <div class="question-section" data-question="12">
                <div class="question-number">Question 12/12</div>
                <div class="question-text">Qu'est-ce qui te ferait vraiment regretté de ne pas essayer ?</div>
                <div class="options">
                    <label class="option">
                        <input type="radio" name="q12" value="3">
                        <label style="margin: 0;">Ne pas avoir vraiment tenté d'être indépendant</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q12" value="2">
                        <label style="margin: 0;">Rester en retrait sans prendre de risques</label>
                    </label>
                    <label class="option">
                        <input type="radio" name="q12" value="1">
                        <label style="margin: 0;">Pas vraiment, ma vie actuelle me convient</label>
                    </label>
                </div>
            </div>

            <!-- CONTACT FORM -->
            <div class="form-section" data-question="contact">
                <h3>Vos informations de contact</h3>
                <div class="form-group">
                    <label for="name">Nom complet *</label>
                    <input type="text" id="name" name="name" placeholder="Jean Dupont" required>
                </div>
                <div class="form-group">
                    <label for="email">Email *</label>
                    <input type="email" id="email" name="email" placeholder="jean@example.com" required>
                </div>
                <div class="form-group">
                    <label for="phone">Téléphone WhatsApp *</label>
                    <input type="tel" id="phone" name="phone" placeholder="+33 6 12 34 56 78" required>
                </div>
            </div>

            <!-- RESULTS -->
            <div class="results-section" id="results">
                <h2 id="resultTitle">🎯 Parfait candidat !</h2>
                <div class="score-badge" id="scoreBadge">Score: 30/30</div>
                <p id="resultText">Vous avez tous les profils pour réussir comme agent immobilier iad.</p>
                <div class="cta-box">
                    ✅ Résultats envoyés sur WhatsApp !<br>
                    Vérifiez votre téléphone
                </div>
            </div>

            <!-- BUTTONS -->
            <div class="button-group">
                <button type="button" class="btn-secondary" id="prevBtn" onclick="changeQuestion(-1)">← Précédent</button>
                <button type="button" class="btn-primary" id="nextBtn" onclick="changeQuestion(1)">Suivant →</button>
            </div>

            <div class="step-indicator" id="stepIndicator">Question 1 sur 11</div>
        </form>
    </div>

    <script>
        const GOOGLE_FORM_URL = 'https://docs.google.com/forms/d/e/1FAIpQLSd_EXAMPLE_FORM_ID/formResponse';
        const FORM_FIELD_MAPPINGS = {
            'entry.123456789': 'name',      // Remplacer par votre ID
            'entry.987654321': 'email',     // Remplacer par votre ID
            'entry.555555555': 'phone',     // Remplacer par votre ID
            'entry.666666666': 'score',     // Remplacer par votre ID
            'entry.777777777': 'level',     // Remplacer par votre ID
        };

        let currentQuestion = 1;
        const totalQuestions = 13; // 12 questions + formulaire de contact
        let answers = {};

        const SCORE_THRESHOLDS = {
            high: 28,
            medium: 18,
            low: 0
        };

        const RESULT_TEXTS = {
            high: {
                title: "🎯 Profil Excellent !",
                text: "Vous avez tous les atouts pour réussir en tant que mandataire immobilier ! Votre capital, votre mentalité entrepreneuriale et votre capacité à bâtir un réseau font de vous un candidat idéal pour développer votre propre fond de commerce. Cette opportunité d'indépendance avec un accompagnement structuré pourrait être parfaite pour vous."
            },
            medium: {
                title: "💪 Bon Potentiel",
                text: "Vous avez de bonnes bases pour devenir mandataire immobilier. Certains domaines comme la gestion du capital ou le recrutement de réseau pourraient être renforcés, mais ce sont des compétences qui s'apprennent avec la bonne structure. Notre formation complète vous permettra de combler les lacunes et de transformer votre fond de commerce en succès."
            },
            low: {
                title: "🤔 À Réfléchir",
                text: "Le métier de mandataire immobilier requiert des qualités et une préparation spécifiques. Ce diagnostic montre quelques points à travailler avant de vous lancer. Cela ne signifie pas que c'est impossible, mais qu'une discussion approfondie serait utile pour explorer vos véritables motivations et votre disponibilité."
            }
        };

        function updateProgress() {
            const progress = (currentQuestion / totalQuestions) * 100;
            document.getElementById('progressFill').style.width = progress + '%';
        }

        function showQuestion(num) {
            // Masquer toutes les questions
            document.querySelectorAll('.question-section').forEach(q => q.classList.remove('active'));
            document.querySelectorAll('.form-section').forEach(f => f.classList.remove('active'));
            document.getElementById('results').classList.remove('active');

            if (num <= 12) {
                const section = document.querySelector(`.question-section[data-question="${num}"]`);
                if (section) section.classList.add('active');
            } else if (num === 13) {
                document.querySelector('.form-section[data-question="contact"]').classList.add('active');
            }

            // Mettre à jour les boutons
            const prevBtn = document.getElementById('prevBtn');
            const nextBtn = document.getElementById('nextBtn');

            prevBtn.style.display = num > 1 ? 'block' : 'none';

            if (num < 13) {
                nextBtn.textContent = 'Suivant →';
                nextBtn.onclick = () => changeQuestion(1);
            } else {
                nextBtn.textContent = 'Voir mon résultat';
                nextBtn.onclick = submitQuiz;
            }

            document.getElementById('stepIndicator').textContent = `Question ${num} sur ${totalQuestions}`;
            updateProgress();
        }

        function changeQuestion(direction) {
            const currentSection = document.querySelector(`.question-section[data-question="${currentQuestion}"]`);
            if (currentSection) {
                const answer = document.querySelector(`input[name="q${currentQuestion}"]:checked`);
                if (!answer) {
                    alert('Veuillez répondre à la question avant de continuer');
                    return;
                }
                answers[`q${currentQuestion}`] = parseInt(answer.value);
            }

            currentQuestion += direction;
            if (currentQuestion < 1) currentQuestion = 1;
            if (currentQuestion > totalQuestions) currentQuestion = totalQuestions;

            showQuestion(currentQuestion);
            window.scrollTo(0, 0);
        }

        async function submitQuiz() {
            // Valider le formulaire de contact
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const phone = document.getElementById('phone').value;

            if (!name || !email || !phone) {
                alert('Veuillez remplir tous les champs');
                return;
            }

            // Calculer le score
            let score = 0;
            for (let i = 1; i <= 12; i++) {
                score += answers[`q${i}`] || 0;
            }

            // Déterminer le niveau
            let level = 'low';
            if (score >= SCORE_THRESHOLDS.high) level = 'high';
            else if (score >= SCORE_THRESHOLDS.medium) level = 'medium';

            const resultData = RESULT_TEXTS[level];

            // Afficher les résultats
            document.querySelector('.form-section[data-question="contact"]').classList.remove('active');
            document.getElementById('resultTitle').textContent = resultData.title;
            document.getElementById('resultText').textContent = resultData.text;
            document.getElementById('scoreBadge').textContent = `Score: ${score}/36`;
            document.getElementById('results').classList.add('active');

            // Boutons finaux
            const buttonGroup = document.querySelector('.button-group');
            buttonGroup.innerHTML = `
                <button type="button" class="btn-secondary" onclick="location.reload()">← Recommencer</button>
                <button type="button" class="btn-primary" onclick="shareOnWhatsApp('${name}', '${score}')">Partager sur WhatsApp</button>
            `;

            // Envoyer les données à Google Forms
            try {
                await sendToGoogleForms(name, email, phone, score, level);
            } catch (error) {
                console.log('Erreur envoi Google Forms:', error);
            }

            updateProgress();
            window.scrollTo(0, 0);
        }

        async function sendToGoogleForms(name, email, phone, score, level) {
            // Formation des données pour Google Forms
            const formData = new FormData();
            formData.append('entry.123456789', name);        // À personnaliser
            formData.append('entry.987654321', email);       // À personnaliser
            formData.append('entry.555555555', phone);       // À personnaliser
            formData.append('entry.666666666', score);       // À personnaliser
            formData.append('entry.777777777', level);       // À personnaliser

            try {
                await fetch(GOOGLE_FORM_URL, {
                    method: 'POST',
                    body: formData,
                    mode: 'no-cors'
                });
            } catch (error) {
                console.error('Erreur:', error);
            }
        }

        function shareOnWhatsApp(name, score) {
            const text = `Bonjour Maxime ! Je viens de compléter ton quiz agent immobilier iad. Mon score : ${score}/30. Mon nom : ${name}. Peux-tu me contacter pour la suite ? 🤔📱`;
            const whatsappUrl = `https://wa.me/33612345678?text=${encodeURIComponent(text)}`;
            window.open(whatsappUrl, '_blank');
        }

        // Initialiser
        showQuestion(1);
    </script>
</body>
</html>
