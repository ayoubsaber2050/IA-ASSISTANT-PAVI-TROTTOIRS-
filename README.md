# IA-ASSISTANT-PAVI-TROTTOIRS-
IA Assistant Virtuel 
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Assistant Autobloquants Maroc</title>
    <style>
        body {
            margin: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #e74c3c 0%, #c0392b 100%);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .chat-container {
            width: 95%;
            max-width: 600px;
            height: 85vh;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.2);
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .header {
            background: linear-gradient(45deg, #2c3e50, #34495e);
            color: white;
            padding: 20px;
            text-align: center;
            font-weight: bold;
            font-size: 18px;
        }

        .header .company-name {
            font-size: 22px;
            margin-bottom: 5px;
        }

        .header .tagline {
            font-size: 14px;
            opacity: 0.9;
        }

        .messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            background: #f8f9fa;
        }

        .message {
            margin: 15px 0;
            padding: 12px 16px;
            border-radius: 18px;
            max-width: 85%;
            word-wrap: break-word;
            line-height: 1.4;
        }

        .bot-message {
            background: #e8f5e8;
            color: #2d5a2d;
            margin-right: auto;
            border-bottom-left-radius: 4px;
            border-left: 4px solid #27ae60;
        }

        .user-message {
            background: #3498db;
            color: white;
            margin-left: auto;
            border-bottom-right-radius: 4px;
        }

        .input-area {
            padding: 20px;
            background: white;
            display: flex;
            gap: 10px;
            border-top: 1px solid #eee;
        }

        #userInput {
            flex: 1;
            padding: 14px 18px;
            border: 2px solid #ddd;
            border-radius: 25px;
            outline: none;
            font-size: 16px;
        }

        #userInput:focus {
            border-color: #3498db;
        }

        button {
            padding: 14px 22px;
            background: #3498db;
            color: white;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.3s;
        }

        button:hover {
            background: #2980b9;
        }

        .typing {
            display: none;
            color: #666;
            font-style: italic;
            padding: 10px 16px;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { opacity: 0.6; }
            50% { opacity: 1; }
            100% { opacity: 0.6; }
        }

        .price-card {
            background: #fff3cd;
            border: 1px solid #ffeaa7;
            border-radius: 8px;
            padding: 12px;
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div class="header">
            <div class="company-name">🏗️ PAVÉ MAROC</div>
            <div class="tagline">Spécialiste en autobloquants et pavage</div>
        </div>
        <div class="messages" id="messages">
            <div class="message bot-message">
                🏠 Bienvenue chez <strong>Pavé Maroc</strong> !<br><br>
                Je peux vous renseigner sur :
                <br>• 📦 Nos autobloquants (types, prix, couleurs)
                <br>• 🚚 Livraison dans tout le Maroc
                <br>• ⏰ Horaires et délais
                <br>• 💰 Devis gratuit
                <br>• 🔧 Service de pose
                <br><br>Que puis-je faire pour vous ?
            </div>
        </div>
        <div class="typing" id="typing">⏳ Je prépare votre réponse...</div>
        <div class="input-area">
            <input type="text" id="userInput" placeholder="Posez votre question (ex: prix pavé 8cm gris)..." onkeypress="handleKeyPress(event)">
            <button onclick="sendMessage()">📤</button>
        </div>
    </div>

    <script>
        // Base de connaissances autobloquants Maroc
        const autobloquantKnowledge = {
            // Produits
            'pavé 8': 'Pavé autobloquant 8cm - Idéal pour parkings et voiries. Prix: 85-95 DH/m². Disponible en: Gris, Rouge, Jaune, Beige. Résistance: Trafic lourd.',
            'pavé 6': 'Pavé autobloquant 6cm - Parfait pour passages piétons et cours. Prix: 75-85 DH/m². Couleurs: Gris, Rouge, Beige. Résistance: Trafic moyen.',
            'pavé 4': 'Pavé autobloquant 4cm - Idéal pour terrasses et jardins. Prix: 65-75 DH/m². Couleurs: Gris, Rouge, Jaune, Beige, Noir. Résistance: Piéton uniquement.',
            
            // Couleurs
            'gris': 'Couleur gris disponible pour tous nos pavés (4cm, 6cm, 8cm). La couleur la plus demandée et économique.',
            'rouge': 'Couleur rouge disponible pour pavés 4cm, 6cm et 8cm. Très apprécié pour les entrées et décorations. +5 DH/m² par rapport au gris.',
            'jaune': 'Couleur jaune disponible pour pavés 4cm et 8cm. Idéal pour délimiter zones de stationnement. +8 DH/m² par rapport au gris.',
            'beige': 'Couleur beige disponible pour tous pavés. Couleur élégante et moderne. +7 DH/m² par rapport au gris.',
            
            // Livraison
            'livraison casa': 'Livraison Casablanca et région: 200-300 DH selon quantité. Délai: 2-3 jours ouvrables.',
            'livraison rabat': 'Livraison Rabat et région: 250-350 DH selon quantité. Délai: 2-4 jours ouvrables.',
            'livraison marrakech': 'Livraison Marrakech: 400-500 DH selon quantité. Délai: 3-5 jours ouvrables.',
            'livraison fès': 'Livraison Fès: 450-550 DH selon quantité. Délai: 4-6 jours ouvrables.',
            'livraison agadir': 'Livraison Agadir: 500-600 DH selon quantité. Délai: 4-6 jours ouvrables.',
            
            // Services
            'pose': 'Service de pose disponible. Prix: 35-45 DH/m² selon complexité. Inclut: préparation terrain, sable, pose, joints. Garantie 2 ans.',
            'devis': 'Devis gratuit sous 24h! Envoyez-nous: surface en m², type de pavé souhaité, adresse de livraison. WhatsApp: 06 XX XX XX XX',
            
            // Horaires
            'horaire': 'Horaires: Lun-Ven 8h-18h, Sam 8h-16h. Fermé dimanche. Showroom: Zone Industrielle, Casablanca.',
            
            // Contact
            'contact': 'Contact: 📞 05 22 XX XX XX | 📱 WhatsApp: 06 XX XX XX XX | 📧 contact@pavemaroc.ma | Showroom: Zone Industrielle Ain Sebaa, Casablanca'
        };

        const cities = ['casa', 'casablanca', 'rabat', 'marrakech', 'fès', 'fes', 'agadir', 'tanger', 'oujda'];
        const colors = ['gris', 'rouge', 'jaune', 'beige', 'noir'];
        const thicknesses = ['4', '6', '8', '4cm', '6cm', '8cm'];

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
                return 'Prix autobloquants:\n• 4cm: 65-75 DH/m²\n• 6cm: 75-85 DH/m²\n• 8cm: 85-95 DH/m²\n\nPose: 35-45 DH/m². Couleurs spéciales: +5 à +8 DH/m²';
            }
            
            // Recherche livraison par ville
            for (let city of cities) {
                if (question.includes(city)) {
                    return autobloquantKnowledge['livraison ' + city] || 
                           'Nous livrons dans cette ville. Frais: 250-600 DH selon distance. Délai: 2-6 jours. Contactez-nous pour devis précis.';
                }
            }
            
            // Recherche couleurs
            for (let color of colors) {
                if (question.includes(color)) {
                    return autobloquantKnowledge[color] || 
                           `Couleur ${color} disponible. Consultez notre catalogue complet ou contactez-nous pour plus d'infos.`;
                }
            }
            
            if (question.includes('épaisseur') || question.includes('taille') || question.includes('dimension')) {
                return 'Épaisseurs disponibles:\n• 4cm: terrasses, jardins (65-75 DH/m²)\n• 6cm: cours, passages (75-85 DH/m²)\n• 8cm: parkings, voiries (85-95 DH/m²)\n\nDimensions standard: 20x10cm, 20x20cm selon le modèle.';
            }
            
            if (question.includes('minimum') || question.includes('commande')) {
                return 'Commande minimum: 50m². Livraison gratuite à partir de 200m² (zones Casablanca-Rabat). Paiement: espèces, chèque, ou virement.';
            }
            
            if (question.includes('qualité') || question.includes('garantie')) {
                return 'Nos pavés sont conformes aux normes marocaines. Résistance testée. Garantie 2 ans sur la pose. Certification qualité ISO. Échantillons disponibles.';
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
                    addMessage('Je n\'ai pas trouvé d\'information sur ce point. Contactez-nous directement:\n📞 05 22 XX XX XX\n📱 WhatsApp: 06 XX XX XX XX\n\nOu demandez-moi des infos sur: prix, livraison, couleurs, épaisseurs, pose, devis.', 'bot');
                }
            }, 1500);
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
            document.getElementById('typing').style.display = 'block';
        }

        function hideTyping() {
            document.getElementById('typing').style.display = 'none';
        }

        function handleKeyPress(event) {
            if (event.key === 'Enter') {
                sendMessage();
            }
        }

        // Message d'aide
        setTimeout(() => {
            addMessage('💡 <strong>Exemples de questions :</strong><br>• "Prix pavé 8cm rouge"<br>• "Livraison Rabat"<br>• "Service de pose"<br>• "Devis gratuit"', 'bot');
        }, 3000);
    </script>
</body>
</html>
