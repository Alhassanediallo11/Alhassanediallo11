<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulateur de Prêt</title>

    <!--Alhassane Diallo  Traité du projet 
            projet 3 : 

            1-Ecrire un simulateur de calcul des taux ! 
            2-Ecrire un simulateur pour afficher les échéances

             https://docs.google.com/document/d/1PUUE7HBeqJ2hDh6c5MxJsBS3N0W2xl83l0kaVofl4V4/edit#heading=h.cp5lm84zoza0
    -->
  
    <style>
       body {
            font-family: Arial, sans-serif;
            margin: 100px;
        }

        h1 {
            text-align: center;
        }

        form {
            max-width: 300px;
            margin: 0 auto;
            display: flex;
            flex-direction: column;
        }

        label {
            margin-bottom: 10px;
            display: flex;
            justify-content: space-between;
        }

        input {
            margin-bottom: 10px;
        }

        #calendrier {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th,
        td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }

        th {
            background-color: hsl(0, 0%, 95%);
        }

        #compteur {
            font-size: 18px;
            position: fixed;
            bottom: 20px;
            right: 20px;
            background-color: black;
            color: white;
            padding: 5px;
        }

        .gen {
            background: hwb(0 95% 5%);
        }
    </style>
</head>
<body>
    
    <div class="gen">
    
    <!-- Formulaire -->
    <form id="formulairePret">
        <label for="montant">Montant</label>
        <input type="number" id="montant" placeholder="Montant du prêt" required>

        <label for="duree">années</label>
        <input type="number" id="duree" placeholder="Durée du prêt en années" required>

        <label for="taux">Taux</label>
        <input type="number" id="taux" placeholder="Taux d'intérêt annuel" required>
    </form>

    <!-- Tableau des Échéances -->
   
    <table id="calendrier">
        <thead>
            <tr>
                <th>Période</th>
                <th>Capital Amorti</th>
                <th>Intérêt</th>
                <th>Capital Restant du</th>
                <th>Mensualité</th>
            </tr>
        </thead>
        <tbody id="corpsCalendrier"></tbody>
    </table>

    <!-- Compteur de temps  -->
    <div id="compteur"><span id="tempsEcoule">0:00</span></div>

    <!--  JavaScript -->
    <script>
        // Variables pour le compteur
        let tempsDepart, tempsEcoule;
        let intervalleCompteur;

        // Fonction pour démarrer compteur
        function demarrerCompteur() {
            tempsDepart = Date.now();
            mettreAJourTempsEcoule();

            // Écouteurs d'événements sur les champs du formulaire
            const champsFormulaire = document.querySelectorAll('#formulairePret input');
            champsFormulaire.forEach(champ => {
                champ.addEventListener('input', function () {
                    mettreAJourTempsEcoule();
                    mettreAJourCalendrier();
                });
            });

            calculerPret(); 
            intervalleCompteur = setInterval(mettreAJourTempsEcoule, 1000);
        }

        // Fonction pour mettre à jour le temps écoulé
        function mettreAJourTempsEcoule() {
            tempsEcoule = Math.floor((Date.now() - tempsDepart) / 1000);
            const minutes = Math.floor(tempsEcoule / 60);
            const secondes = tempsEcoule % 60;
            document.getElementById('tempsEcoule').textContent = `${minutes}:${secondes < 10 ? '0' : ''}${secondes}`;
        }

        // Fonction pour arrêter le compteur
        function arreterCompteur() {
            clearInterval(intervalleCompteur);
        }

        // Fonction pour le calcul du prêt
        function calculerPret() {
           
        }

        // Fonction pour mettre à jour le tableau des échéances
        function mettreAJourCalendrier() {
            const montant = parseFloat(document.getElementById('montant').value);
            const duree = parseInt(document.getElementById('duree').value);
            const taux = parseFloat(document.getElementById('taux').value);

            const corpsCalendrier = document.getElementById('corpsCalendrier');
            corpsCalendrier.innerHTML = '';

            let montantRestant = montant;
            const tauxMensuel = taux / 100 / 12;
            const nombreDePaiements = duree * 12;

            const mensualite = montant * (tauxMensuel / (1 - Math.pow(1 + tauxMensuel, -nombreDePaiements)));

            for (let periode = 1; periode <= nombreDePaiements; periode++) {
                const interet = montantRestant * tauxMensuel;
                const principal = mensualite - interet;
                montantRestant -= principal;

                const ligne = corpsCalendrier.insertRow();
                ligne.insertCell(0).textContent = periode;
                ligne.insertCell(1).textContent = principal.toFixed(2);
                ligne.insertCell(2).textContent = interet.toFixed(2);
                ligne.insertCell(3).textContent = montantRestant.toFixed(2);
                ligne.insertCell(4).textContent = mensualite.toFixed(2);
            }
        }

        // Déclencher le calcul initial et la mise à jour du calendrier au chargement de la page
        demarrerCompteur();
        //Alhassane Diallo L2 info
    </script>
    </div>
</body>
</html>
