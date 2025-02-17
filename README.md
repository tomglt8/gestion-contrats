<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gestion des Contrats et Litiges</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }
        header {
            background: #333;
            color: #fff;
            padding: 15px;
            text-align: center;
            font-size: 24px;
        }
        .container {
            width: 80%;
            margin: 20px auto;
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: left;
        }
        th {
            background-color: #333;
            color: white;
        }
        .btn {
            padding: 5px 10px;
            margin: 2px;
            background: #dc3545;
            color: #fff;
            border: none;
            cursor: pointer;
            border-radius: 5px;
        }
        .btn-edit {
            background: #ffc107;
        }
    </style>
    <script>
        function ajouterLigne(tableId, values) {
            let table = document.getElementById(tableId);
            let newRow = table.insertRow(-1);
            for (let value of values) {
                let cell = newRow.insertCell();
                cell.textContent = value;
            }
            let actionCell = newRow.insertCell();
            actionCell.innerHTML = `<button class='btn btn-edit' onclick='modifierLigne(this)'>Modifier</button> <button class='btn' onclick='supprimerLigne(this)'>Supprimer</button>`;
        }

        function ajouterContrat(event) {
            event.preventDefault();
            let nom = document.getElementById("nom").value;
            let date = document.getElementById("date").value;
            let statut = document.getElementById("statut").value;
            ajouterLigne("tableContrats", [nom, date, statut]);
            event.target.reset();
        }

        function ajouterLitige(event) {
            event.preventDefault();
            let nom = document.getElementById("nomLitige").value;
            let statut = document.getElementById("statutLitige").value;
            let date = document.getElementById("dateLitige").value;
            ajouterLigne("tableLitiges", [nom, statut, date]);
            event.target.reset();
        }

        function supprimerLigne(button) {
            let row = button.parentNode.parentNode;
            row.parentNode.removeChild(row);
        }

        function modifierLigne(button) {
            let row = button.parentNode.parentNode;
            let cells = row.getElementsByTagName("td");
            for (let i = 0; i < cells.length - 1; i++) {
                let input = document.createElement("input");
                input.type = "text";
                input.value = cells[i].textContent;
                cells[i].innerHTML = "";
                cells[i].appendChild(input);
            }
            button.textContent = "Sauvegarder";
            button.onclick = function() { sauvegarderLigne(button); };
        }

        function sauvegarderLigne(button) {
            let row = button.parentNode.parentNode;
            let cells = row.getElementsByTagName("td");
            for (let i = 0; i < cells.length - 1; i++) {
                cells[i].textContent = cells[i].firstChild.value;
            }
            button.textContent = "Modifier";
            button.onclick = function() { modifierLigne(button); };
        }
    </script>
</head>
<body>
    <header>Gestion des Contrats et des Litiges</header>
    <div class="container">
        <h2>Liste des Contrats</h2>
        <table id="tableContrats">
            <tr>
                <th>Nom du Contrat</th>
                <th>Date de Signature</th>
                <th>Statut</th>
                <th>Actions</th>
            </tr>
        </table>
        
        <h2>Ajouter un Contrat</h2>
        <form onsubmit="ajouterContrat(event)">
            <input type="text" id="nom" placeholder="Nom du Contrat" required>
            <input type="date" id="date" required>
            <input type="text" id="statut" placeholder="Statut" required>
            <input type="submit" value="Ajouter">
        </form>
        
        <h2>Suivi des Litiges</h2>
        <table id="tableLitiges">
            <tr>
                <th>Nom du Litige</th>
                <th>Statut</th>
                <th>Dernière Mise à Jour</th>
                <th>Actions</th>
            </tr>
        </table>
        
        <h2>Ajouter un Litige</h2>
        <form onsubmit="ajouterLitige(event)">
            <input type="text" id="nomLitige" placeholder="Nom du Litige" required>
            <input type="text" id="statutLitige" placeholder="Statut" required>
            <input type="date" id="dateLitige" required>
            <input type="submit" value="Ajouter">
        </form>
    </div>
</body>
</html>
