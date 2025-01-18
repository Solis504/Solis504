<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inventario IT Infraestructura</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-image: url('https://seeklogo.com/images/A/acosa-logo-27A84B5465-seeklogo.com.png');
            background-repeat: no-repeat;
            background-position: center;
            background-size: 50%;
            background-color: #f9f9f9;
        }

        .hidden {
            display: none;
        }

        .login-container, .main-container {
            width: 90%;
            max-width: 600px;
            margin: auto;
            padding: 20px;
            border: 1px solid #ddd;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
        }

        input, button, select {
            width: calc(100% - 20px);
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 5px;
        }

        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }

        button:hover {
            background-color: #45a049;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }

        th {
            background-color: #f4f4f4;
        }

        #loginMessage {
            color: red;
            text-align: center;
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
</head>
<body>
    <div class="login-container" id="loginContainer">
        <h1>Inicio de Sesión</h1>
        <input type="text" id="username" placeholder="Nombre de usuario" required>
        <input type="password" id="password" placeholder="Contraseña" required>
        <button onclick="login()">Entrar</button>
        <p id="loginMessage"></p>
    </div>

    <div class="main-container hidden" id="mainContainer">
        <h1>Inventario IT Infraestructura</h1>
        <form id="inventoryForm">
            <label for="name">Nombre del equipo:</label>
            <input type="text" id="name" name="name" required>

            <label for="location">Ubicación:</label>
            <input type="text" id="location" name="location">

            <label for="reviewDate">Fecha de Revisión:</label>
            <input type="date" id="reviewDate" name="reviewDate">

            <label for="retirementPlace">Lugar de Retiro:</label>
            <input type="text" id="retirementPlace" name="retirementPlace">

            <label for="description">Descripción:</label>
            <input type="text" id="description" name="description" required>

            <label for="series">Serie:</label>
            <input type="text" id="series" name="series" required>

            <label for="quantity">Cantidad:</label>
            <input type="number" id="quantity" name="quantity" min="1" required>

            <label for="condition">Condición:</label>
            <select id="condition" name="condition" required>
                <option value="Nuevo">Nuevo</option>
                <option value="Usado">Usado</option>
            </select>

            <button type="button" onclick="addToTable()">Agregar al inventario</button>
        </form>

        <table id="inventoryTable">
            <thead>
                <tr>
                    <th>Nombre</th>
                    <th>Descripción</th>
                    <th>Serie</th>
                    <th>Cantidad</th>
                    <th>Condición</th>
                    <th>Ubicación</th>
                    <th>Fecha de Revisión</th>
                    <th>Lugar de Retiro</th>
                    <th>Acción</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
        <button onclick="exportToExcel()">Descargar en Excel</button>
    </div>

    <script>
        const validCredentials = [
            { username: 'Solis', password: 'Acosa1991' },
            { username: 'John', password: 'Acosa1991' },
            { username: 'Kevin', password: 'Acosa1991' }
        ];

        function login() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            const message = document.getElementById('loginMessage');

            const isValid = validCredentials.some(
                user => user.username === username && user.password === password
            );

            if (isValid) {
                document.getElementById('loginContainer').classList.add('hidden');
                document.getElementById('mainContainer').classList.remove('hidden');
            } else {
                message.textContent = 'Usuario o contraseña incorrectos.';
            }
        }

        function addToTable() {
            const name = document.getElementById('name').value;
            const description = document.getElementById('description').value;
            const series = document.getElementById('series').value;
            const quantity = parseInt(document.getElementById('quantity').value, 10);
            const condition = document.getElementById('condition').value;
            const location = document.getElementById('location').value;
            const reviewDate = document.getElementById('reviewDate').value;
            const retirementPlace = document.getElementById('retirementPlace').value;

            if (name && description && series && quantity && condition) {
                const table = document.getElementById('inventoryTable').getElementsByTagName('tbody')[0];
                const newRow = table.insertRow();
                newRow.innerHTML = `
                    <td>${name}</td>
                    <td>${description}</td>
                    <td>${series}</td>
                    <td>${quantity}</td>
                    <td>${condition}</td>
                    <td>${location}</td>
                    <td>${reviewDate}</td>
                    <td>${retirementPlace}</td>
                    <td><button onclick="deleteRow(this)">Eliminar</button></td>
                `;
                document.getElementById('inventoryForm').reset();
            } else {
                alert('Por favor, complete todos los campos antes de agregar.');
            }
        }

        function deleteRow(button) {
            const row = button.parentNode.parentNode;
            row.parentNode.removeChild(row);
        }

        function exportToExcel() {
            const table = document.getElementById('inventoryTable');
            const wb = XLSX.utils.book_new();
            const ws = XLSX.utils.table_to_sheet(table);
            XLSX.utils.book_append_sheet(wb, ws, "Inventario");
            XLSX.writeFile(wb, "Inventario.xlsx");
        }
    </script>
</body>
</html>

