<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EL KIRED MATÉRIAUX - Assistant</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
            background: #fef3c7;
            height: 100vh;
            display: flex;
            flex-direction: column;
        }

        .header {
            background: #1f2937;
            border-bottom: 1px solid #374151;
            padding: 16px 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .company-name {
            font-size: 18px;
            font-weight: 700;
            color: #fbbf24;
            text-align: center;
        }

        .tagline {
            font-size: 14px;
            color: #fcd34d;
            margin-top: 4px;
        }

        .chat-container {
            flex: 1;
            max-width: 768px;
            margin: 0 auto;
            width: 100%;
            display: flex;
            flex-direction: column;
            background: white;
            box-shadow: 0 0 0 1px #fbbf24;
        }

        .messages {
            flex: 1;
            padding: 24px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .message {
            max-width: 85%;
            padding: 12px 16px;
            border-radius: 16px;
            line-height: 1.5;
            font-size: 15px;
        }

        .bot-message {
            background: #fef3c7;
            color: #1f2937;
            align-self: flex-start;
            border: 1px solid #fbbf24;
        }

        .user-message {
            background: #1f2937;
            color: #fbbf24;
            align-self: flex-end;
        }

        .input-container {
            padding: 16px 24px 24px;
            border-top: 1px solid #fbbf24;
            background: white;
        }

        .input-area {
            display: flex;
            gap: 12px;
            align-items: flex-end;
            max-width: 100%;
        }

        #userInput {
            flex: 1;
            min-height: 44px;
            padding: 12px 16px;
            border: 1px solid #fbbf24;
            border-radius: 22px;
            outline: none;
            font-size: 15px;
            font-family: inherit;
            background: white;
            resize: none;
        }

        #userInput:focus {
            border-color: #1f2937;
            box-shadow: 0 0 0 2px rgba(251, 191, 36, 0.2);
        }

        .send-button {
            width: 44px;
            height: 44px;
            border: none;
            border-radius: 50%;
            background: #1f2937;
            color: #fbbf24;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            transition: background-color 0.2s;
        }

        .send-button:hover:not(:disabled) {
            background: #374151;
        }

        .send-button:disabled {
            background: #9ca3af;
            cursor: not-allowed;
        }

        .typing-indicator {
            display: none;
            align-self: flex-start;
            background: #fef3c7;
            border: 1px solid #fbbf24;
            border-radius: 16px;
            padding: 12px 16px;
            max-width: 85%;
        }

        .typing-dots {
            display: flex;
            gap: 4px;
            align-items: center;
        }

        .typing-dot {
            width: 6px;
            height: 6px;
            border-radius: 50%;
            background: #1f2937;
            animation: typing 1.4s infinite;
        }

        .typing-dot:nth-child(2) { animation-delay: 0.2s; }
        .typing-dot:nth-child(3) { animation-delay: 0.4s; }

        @keyframes typing {
            0%, 60%, 100% { opacity: 0.3; }
            30% { opacity: 1; }
        }

        .welcome-card {
            background: #fef3c7;
            border: 1px solid #fbbf24;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 16px;
        }

        .welcome-title {
            font-weight: 600;
            color: #1f2937;
            margin-bottom: 8px;
            font-size: 16px;
        }

        .welcome-list {
            list-style: none;
            color: #1f2937;
            font-size: 14px;
            line-height: 1.6;
        }

        .welcome-list li {
            margin: 4px 0;
            padding-left: 20px;
            position: relative;
        }

        .welcome-list li:before {
            content: "•";
            position: absolute;
            left: 0;
            color: #1f2937;
            font-weight: bold;
        }

        @media (max-width: 768px) {
            .messages {
                padding: 16px;
            }
            
            .input-container {
                padding: 12px 16px 16px;
            }
            
            .message {
                max-width: 90%;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <div>
            <div class="company-name">EL KIRED MATÉRIAUX</div>
            <div class="tagline">Assistant spécialisé autobloquants</div>
        </div>
    </div>

    <div class="chat-container">
        <div class="messages" id="messages">
            <div class="welcome-card">
                <div class="welcome-title">👋 Bienvenue chez EL KIRED MATÉRIAUX</div>
                <div style="color: #1f2937; font-size: 14px; margin-bottom: 12px;">
                    Je peux vous aider avec toutes vos questions concernant nos autobloquants :
                </div>
                <ul class="welcome-list">
                    <li>Types et prix des pavés (4cm, 6cm, 8cm)</li>
                    <li>Couleurs disponibles (gris, rouge, jaune, beige)</li>
                    <li>Livraison dans tout le Maroc</li>
                    <li>Service de pose professionnel</li>
                    <li>Devis gratuit sous 24h</li>
                    <li>Conseils techniques</li>
                </ul>
            </div>
        </div>

        <div class="typing-indicator" id="typingIndicator">
            <div class="typing-dots">
                <div class="typing-dot"></div>
                <div class="typing-dot"></div>
                <div class="typing-dot"></div>
            </div>
        </div>

        <div class="input-container">
            <div class="input-area">
                <input 
                    type="text" 
                    id="userInput" 
                    placeholder="Posez votre question..." 
                    onkeypress="handleKeyPress(event)"
                >
                <button class="send-button" onclick="sendMessage()" id="sendButton">
                    ↗
                </button>
            </div>
        </div>
    </div>

    <script>
        // Base de connaissances EL KIRED MATÉRIAUX
        const autobloquantKnowledge = {
            // Produits
            'pavé 8': 'Pavé autobloquant 8cm - Parfait pour parkings et voiries à fort trafic.\n\n💰 Prix: 85-95 DH/m²\n🎨 Couleurs: Gris, Rouge, Jaune, Beige\n💪 Résistance: Trafic lourd (camions, voitures)\n📐 Format standard: 20x10cm',
            
            'pavé 6': 'Pavé autobloquant 6cm - Idéal pour cours et passages piétons.\n\n💰 Prix: 75-85 DH/m²\n🎨 Couleurs: Gris, Rouge, Beige\n💪 Résistance: Trafic moyen\n📐 Format standard: 20x10cm',
            
            'pavé 4': 'Pavé autobloquant 4cm - Parfait pour terrasses et jardins.\n\n💰 Prix: 65-75 DH/m²\n🎨 Couleurs: Gris, Rouge, Jaune, Beige, Noir\n💪 Résistance: Passage piéton uniquement\n📐 Format standard: 20x10cm',
            
            // Couleurs
            'gris': 'Couleur gris disponible pour tous nos pavés. C\'est notre couleur de base, la plus économique et polyvalente.',
            
            'rouge': 'Couleur rouge disponible pour tous types de pavés. Très demandé pour les entrées et décorations. Supplément: +5 DH/m²',
            
            'jaune': 'Couleur jaune disponible pour pavés 4cm et 8cm. Parfait pour délimiter les zones. Supplément: +8 DH/m²',
            
            'beige': 'Couleur beige disponible pour tous pavés. Couleur moderne et élégante. Supplément: +7 DH/m²',
            
            // Livraison
            'livraison casa': 'Livraison Casablanca et région:\n🚚 Tarif: 200-300 DH selon quantité\n⏱ Délai: 2-3 jours ouvrables\n📦 Livraison gratuite à partir de 200m²',
            
            'livraison rabat': 'Livraison Rabat et région:\n🚚 Tarif: 250-350 DH selon quantité\n⏱ Délai: 2-4 jours ouvrables\n📦 Livraison gratuite à partir de 200m²',
            
            'livraison marrakech': 'Livraison Marrakech:\n🚚 Tarif: 400-500 DH selon quantité\n⏱ Délai: 3-5 jours ouvrables',
            
            'livraison fès': 'Livraison Fès:\n🚚 Tarif: 450-550 DH selon quantité\n⏱ Délai: 4-6 jours ouvrables',
            
            'livraison agadir': 'Livraison Agadir:\n🚚 Tarif: 500-600 DH selon quantité\n⏱ Délai: 4-6 jours ouvrables',
            
            // Services
            'pose': 'Service de pose professionnel:\n\n💰 Prix: 35-45 DH/m² selon complexité\n✅ Inclut: préparation terrain, lit de sable, pose, joints\n🛡 Garantie: 2 ans sur la pose\n👷 Équipe certifiée',
            
            'devis': 'Devis gratuit sous 24h !\n\nEnvoyez-nous:\n📏 Surface en m²\n🧱 Type de pavé souhaité\n🏠 Adresse de livraison\n📱 WhatsApp: 06 XX XX XX XX',
            
            // Horaires et contact
            'horaire': 'Nos horaires:\n📅 Lun-Ven: 8h-18h\n📅 Samedi: 8h-16h\n🚫 Fermé dimanche\n🏢 Showroom: Zone Industrielle, Casablanca',
            
            'contact': 'Contactez-nous:\n📞 Fixe: 05 22 XX XX XX\n📱 WhatsApp: 06 XX XX XX XX\n📧 Email: contact@elkired.ma\n🏢 Adresse: Zone Industrielle Ain Sebaa, Casablanca'
        };

        const cities = ['casa', 'casablanca', 'rabat', 'marrakech', 'fès', 'fes', 'agadir', 'tanger', 'oujda'];

        function findBestMatch(question) {
            question = question.toLowerCase();
            
            // Recherche exacte
            for (let keyword in autobloquantKnowledge) {
                if (question.includes(keyword)) {
                    return autobloquantKnowledge[keyword];
                }
            }
            
            // Recherches spécifiques
            if (question.includes('prix') || question.includes('coût') || question.includes('tarif')) {
                if (question.includes('8')) return autobloquantKnowledge['pavé 8'];
                if (question.includes('6')) return autobloquantKnowledge['pavé 6'];
                if (question.includes('4')) return autobloquantKnowledge['pavé 4'];
                return '💰 Nos tarifs autobloquants:\n\n• 4cm: 65-75 DH/m² (terrasses)\n• 6cm: 75-85 DH/m² (passages)\n• 8cm: 85-95 DH/m² (parkings)\n\n🔧 Pose: 35-45 DH/m²\n🎨 Couleurs spéciales: +5 à +8 DH/m²';
            }
            
            // Livraison par ville
            for (let city of cities) {
                if (question.includes(city)) {
                    return autobloquantKnowledge['livraison ' + city] || 
                           'Nous livrons dans cette ville. Frais: 250-600 DH selon distance. Contactez-nous pour un devis précis.';
                }
            }
            
            if (question.includes('épaisseur') || question.includes('taille') || question.includes('dimension')) {
                return '📐 Nos épaisseurs disponibles:\n\n• 4cm: terrasses, jardins (65-75 DH/m²)\n• 6cm: cours, passages (75-85 DH/m²)\n• 8cm: parkings, voiries (85-95 DH/m²)\n\n📏 Dimensions standard: 20x10cm selon modèle';
            }
            
            if (question.includes('minimum') || question.includes('commande')) {
                return '📦 Conditions de commande:\n\n• Commande minimum: 50m²\n• Livraison gratuite à partir de 200m² (zones Casa-Rabat)\n• Paiement: espèces, chèque, virement\n• Acompte: 30% à la commande';
            }
            
            return null;
        }

        function sendMessage() {
            const input = document.getElementById('userInput');
            const message = input.value.trim();
            
            if (message === '') return;
            
            addMessage(message, 'user');
            input.value = '';
            
            showTyping();
            
            setTimeout(() => {
                hideTyping();
                const response = findBestMatch(message);
                
                if (response) {
                    addMessage(response, 'bot');
                } else {
                    addMessage('Je n\'ai pas trouvé d\'information sur ce point précis. \n\nPour une réponse personnalisée, contactez-nous:\n📞 05 22 XX XX XX\n📱 WhatsApp: 06 XX XX XX XX\n\nOu demandez-moi des infos sur: prix, livraison, couleurs, pose, devis.', 'bot');
                }
            }, 800);
        }

        function addMessage(text, sender) {
            const messagesDiv = document.getElementById('messages');
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${sender}-message`;
            messageDiv.innerHTML = text.replace(/\n/g, '<br>');
            messagesDiv.appendChild(messageDiv);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }

        function showTyping() {
            document.getElementById('typingIndicator').style.display = 'block';
            document.getElementById('messages').scrollTop = document.getElementById('messages').scrollHeight;
        }

        function hideTyping() {
            document.getElementById('typingIndicator').style.display = 'none';
        }

        function handleKeyPress(event) {
            if (event.key === 'Enter' && !event.shiftKey) {
                event.preventDefault();
                sendMessage();
            }
        }

        // Message d'aide après quelques secondes
        setTimeout(() => {
            addMessage('💡 Exemples de questions:\n\n• "Prix pavé 8cm rouge"\n• "Livraison Rabat"\n• "Service de pose"\n• "Devis gratuit"\n• "Couleurs disponibles"', 'bot');
        }, 3000);
    </script>
</body>
</html>                    
            
        
