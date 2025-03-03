<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prise de Rendez-vous</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 20px;
        }
        form {
            max-width: 400px;
            margin: auto;
            background: #f8f8f8;
            padding: 20px;
            border-radius: 10px;
        }
        input, select, button {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        button {
            background-color: #25D366;
            color: white;
            font-size: 16px;
            cursor: pointer;
        }
        .horaires {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
        }
        .horaire {
            background: #007bff;
            color: white;
            padding: 10px;
            margin: 5px;
            border-radius: 5px;
            cursor: pointer;
        }
        .horaire.selected {
            background: #0047ab;
        }
    </style>
</head>
<body>
    <h2>Réservez un Rendez-vous</h2>
    <form id="rdvForm">
        <label for="name">Nom :</label>
        <input type="text" id="name" required>

        <label for="phoneCode">Indicatif :</label>
        <select id="phoneCode" required></select>

        <label for="phone">Numéro WhatsApp :</label>
        <input type="tel" id="phone" required>

        <label for="service">Service :</label>
        <select id="service" required onchange="updateServiceList()">
            <option value="">Sélectionnez un service</option>
            <option value="Opérateurs GMS">Opérateurs GMS</option>
            <option value="Services publics">Services publics</option>
            <option value="Banques">Banques</option>
        </select>

        <label for="subService">Sélection :</label>
        <select id="subService" required></select>

        <label for="date">Date :</label>
        <input type="date" id="date" required>

        <div class="horaires" id="horaires"></div>

        <button type="button" onclick="envoyerWhatsApp()">Confirmer et Envoyer sur WhatsApp</button>
    </form>

    <script>
        document.addEventListener("DOMContentLoaded", function() {
            let phoneSelect = document.getElementById("phoneCode");
            let countryCodes = {
                "+33": "🇫🇷 France (+33)", "+1": "🇺🇸 USA (+1)", "+32": "🇧🇪 Belgique (+32)",
                "+237": "🇨🇲 Cameroun (+237)", "+229": "🇧🇯 Bénin (+229)", "+225": "🇨🇮 Côte d'Ivoire (+225)", "+221": "🇸🇳 Sénégal (+221)",
                "+216": "🇹🇳 Tunisie (+216)", "+44": "🇬🇧 Royaume-Uni (+44)", "+49": "🇩🇪 Allemagne (+49)",
                "+86": "🇨🇳 Chine (+86)", "+91": "🇮🇳 Inde (+91)", "+81": "🇯🇵 Japon (+81)"
            };
            for (let code in countryCodes) {
                let option = document.createElement("option");
                option.value = code;
                option.textContent = countryCodes[code];
                phoneSelect.appendChild(option);
            }
        });

        function updateServiceList() {
            let subServiceSelect = document.getElementById("subService");
            subServiceSelect.innerHTML = "";
            let selectedService = document.getElementById("service").value;
            let options = [];

            if (selectedService === "Banques") {
                options = ["BANK OF AFRICA - BENIN (BOA – BENIN)", "BANQUE ATLANTIQUE BENIN", "BANQUE INTERNATIONALE POUR L'INDUSTRIE ET LE COMMERCE (B.I.I.C)", "BANQUE SAHELO-SAHARIENNE POUR L'INVESTISSEMENT ET LE COMMERCE – BENIN (BSIC - BENIN)", "BGFIBANK BENIN", "CCEI BANK BENIN", "CORIS BANK INTERNATIONAL – BENIN", "ECOBANK – BENIN", "NSIA BANQUE BENIN", "ORABANK BENIN"];
            } else if (selectedService === "Services publics") {
                options = ["ANIP", "IMPÔTS", "PRÉFECTURE"];
            } else if (selectedService === "Opérateurs GMS") {
                options = ["MTN", "MOOV", "CELTIIS"];
            }
            
            options.forEach(opt => {
                let option = document.createElement("option");
                option.value = opt;
                option.textContent = opt;
                subServiceSelect.appendChild(option);
            });
        }

        document.getElementById("date").addEventListener("change", function() {
            let horairesContainer = document.getElementById("horaires");
            horairesContainer.innerHTML = "";
            let heuresDisponibles = ["08:00", "09:00", "10:00", "11:00", "14:00", "15:00", "16:00", "17:00"];
            
            heuresDisponibles.forEach(heure => {
                let div = document.createElement("div");
                div.classList.add("horaire");
                div.textContent = heure;
                div.onclick = function() {
                    document.querySelectorAll(".horaire").forEach(h => h.classList.remove("selected"));
                    div.classList.add("selected");
                    div.setAttribute("data-selected", heure);
                };
                horairesContainer.appendChild(div);
            });
        });
    </script>
</body>
</html>
