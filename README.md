<!DOCTYPE html>
<html lang="es">
<head>
    <title>paguina web minijuegos</title>
</head>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplicación de Juegos HTML</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            text-align: center;
            margin: 0;
            padding: 0;
        }

        h1 {
            margin: 20px;
        }

        .juego {
            border: 2px solid #4CAF50;
            padding: 20px;
            margin: 20px auto;
            width: 90%;
            max-width: 600px;
            background-color: #fff;
        }

        .btn {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            font-size: 16px;
            margin-top: 10px;
        }

        .btn:hover {
            background-color: #45a049;
        }

        .resultado {
            font-weight: bold;
            margin-top: 10px;
        }

        #snakeCanvas, #pacmanCanvas {
            background-color: #000;
            border: 2px solid #4CAF50;
            margin: 10px 0;
        }
    </style>
</head>
<body>

    <h1>Aplicación de Juegos HTML</h1>

    <button class="btn" onclick="detenerJuegos()">Detener todos los juegos</button>

    <!-- Juego Adivina el Número -->
    <div class="juego" id="adivina-numero">
        <h2>Adivina el Número</h2>
        <p>Adivina el número entre 1 y 100.</p>
        <input type="number" id="numeroAdivina" placeholder="Ingresa un número" min="1" max="100">
        <button class="btn" onclick="adivinarNumero()">Adivinar</button>
        <p id="resultadoAdivina" class="resultado"></p>
    </div>

    <!-- Juego Piedra, Papel o Tijeras -->
    <div class="juego" id="piedra-papel-tijeras">
        <h2>Piedra, Papel o Tijeras</h2>
        <p>Elige tu opción:</p>
        <button class="btn" onclick="jugar('piedra')">Piedra</button>
        <button class="btn" onclick="jugar('papel')">Papel</button>
        <button class="btn" onclick="jugar('tijeras')">Tijeras</button>
        <p id="resultadoPPT" class="resultado"></p>
    </div>

    <!-- Juego de Memoria -->
    <div class="juego" id="juego-memoria">
        <h2>Juego de Memoria</h2>
        <p>Memoriza esta secuencia:</p>
        <button class="btn" onclick="generarSecuencia()">Generar Secuencia</button>
        <div id="secuencia"></div>
        <p>Ingresa la secuencia:</p>
        <input type="text" id="entradaMemoria">
        <button class="btn" onclick="verificarSecuencia()">Verificar</button>
        <p id="resultadoMemoria" class="resultado"></p>
    </div>

    <!-- Contador de Clics -->
    <div class="juego" id="contador-clics">
        <h2>Contador de Clics</h2>
        <p>¡Cuántos clics puedes hacer en 10 segundos!</p>
        <button class="btn" onclick="iniciarContador()">Empezar</button>
        <p id="resultadoClics" class="resultado"></p>
    </div>

    <!-- Juego Snake -->
    <div class="juego" id="snake-game">
        <h2>Snake</h2>
        <p>Usa las teclas de flecha para mover la serpiente.</p>
        <canvas id="snakeCanvas" width="300" height="300"></canvas>
        <p id="snakeScore" class="resultado">Puntuación: 0</p>
    </div>

    <!-- Juego Pac-Man -->
    <div class="juego" id="pacman-game">
        <h2>Pac-Man</h2>
        <p>Usa las teclas de flecha para mover a Pac-Man.</p>
        <canvas id="pacmanCanvas" width="300" height="300"></canvas>
        <p id="pacmanScore" class="resultado">Puntuación: 0</p>
    </div>

    <script>
        // Adivina el Número
        let numeroAleatorio = Math.floor(Math.random() * 100) + 1;
        function adivinarNumero() {
            const numeroUsuario = parseInt(document.getElementById("numeroAdivina").value);
            const resultado = document.getElementById("resultadoAdivina");
            if (numeroUsuario === numeroAleatorio) {
                resultado.textContent = "¡Correcto!";
                numeroAleatorio = Math.floor(Math.random() * 100) + 1;
            } else {
                resultado.textContent = numeroUsuario < numeroAleatorio ? "Demasiado bajo." : "Demasiado alto.";
            }
        }

        // Piedra, Papel o Tijeras
        function jugar(eleccionUsuario) {
            const opciones = ["piedra", "papel", "tijeras"];
            const eleccionMaquina = opciones[Math.floor(Math.random() * opciones.length)];
            document.getElementById("resultadoPPT").textContent =
                eleccionUsuario === eleccionMaquina ? "Empate." :
                (eleccionUsuario === "piedra" && eleccionMaquina === "tijeras") ||
                (eleccionUsuario === "papel" && eleccionMaquina === "piedra") ||
                (eleccionUsuario === "tijeras" && eleccionMaquina === "papel") ?
                "¡Ganaste!" : "Perdiste.";
        }

        // Juego de Memoria
        let secuenciaGenerada = "";
        function generarSecuencia() {
            secuenciaGenerada = "";
            for (let i = 0; i < 5; i++) secuenciaGenerada += Math.floor(Math.random() * 10);
            document.getElementById("secuencia").textContent = secuenciaGenerada;
            setTimeout(() => document.getElementById("secuencia").textContent = "", 2000);
        }
        function verificarSecuencia() {
            const entradaUsuario = document.getElementById("entradaMemoria").value;
            document.getElementById("resultadoMemoria").textContent =
                entradaUsuario === secuenciaGenerada ? "¡Correcto!" : "Incorrecto.";
        }

        // Contador de Clics
        let contadorClics = 0;
        let intervalo;
        function iniciarContador() {
            contadorClics = 0;
            tiempoRestante = 10;
            document.getElementById("resultadoClics").textContent = "¡Empieza!";
            intervalo = setInterval(() => {
                if (--tiempoRestante <= 0) {
                    clearInterval(intervalo);
                    document.getElementById("resultadoClics").textContent = "Clics: " + contadorClics;
                }
            }, 1000);
            document.querySelector(".juego").addEventListener("click", () => contadorClics++);
        }

        // Juego Snake
        const snakeCanvas = document.getElementById("snakeCanvas");
        const snakeCtx = snakeCanvas.getContext("2d");
        const box = 20;
        let snakeGame, pacmanGame, snake = [{ x: 4 * box, y: 4 * box }];
        let direction = null;
        document.addEventListener("keydown", (e) => direction = e.key.replace("Arrow", "").toUpperCase());
        function drawSnake() { /* ... lógica del juego Snake ... */ }

        // Juego Pac-Man
        const pacmanCanvas = document.getElementById("pacmanCanvas");
        const pacmanCtx = pacmanCanvas.getContext("2d");
        let pacman = { x: 150, y: 150, dx: 0, dy: 0 };
        document.addEventListener("keydown", (e) => {
            if (["Left", "Up", "Right", "Down"].includes(e.key)) {
                pacman.dx = { Left: -2, Right: 2 }[e.key] || 0;
                pacman.dy = { Up: -2, Down: 2 }[e.key] || 0;
            }
        });
        function drawPacman() { /* ... lógica del juego Pac-Man ... */ }

        // Detener todos los juegos
        function detenerJuegos() {
            clearInterval(snakeGame);
            clearInterval(pacmanGame);
            clearInterval(intervalo);
            alert("¡Todos los juegos han sido detenidos!");
        }
    </script>
</body>
</html>
