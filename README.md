<html>
<head>
<meta charset="utf-8">
<title>ENVÍOS MAPA</title>

<style>
body {
    margin:0;
    font-family: -apple-system, BlinkMacSystemFont, sans-serif;
    background:#0c0c0e;
    color:white;
    text-align:center;
}

h1 {
    margin-top:20px;
    font-weight:600;
    font-size:28px;
}

.container {
    padding:15px;
}

input {
    width:90%;
    padding:12px;
    margin:10px 0;
    border-radius:12px;
    border:none;
    background:#1c1c1e;
    color:white;
    font-size:16px;
}

button {
    width:90%;
    padding:12px;
    margin:6px 0;
    border:none;
    border-radius:14px;
    font-size:16px;
    font-weight:500;
    background:#2c2c2e;
    color:white;
}

button:active {
    background:#3a3a3c;
}

.lista {
    margin-top:10px;
}

.item {
    background:#1c1c1e;
    margin:6px auto;
    padding:10px;
    border-radius:12px;
    width:90%;
    text-align:left;
    font-size:14px;
}

.estado {
    margin-top:15px;
    font-size:14px;
    opacity:0.8;
}
</style>
</head>

<body>

<h1>ENVÍOS MAPA</h1>

<div class="container">

<input id="direccion" placeholder="Ej: San Benito 681, Merlo">

<button onclick="agregar()">Agregar destino</button>

<div id="lista" class="lista"></div>

<button onclick="invertir()">Invertir orden</button>
<button onclick="iniciar()">Iniciar ruta</button>
<button onclick="siguiente()">Siguiente destino</button>

<p id="estado" class="estado"></p>

</div>

<script>
let destinos = [];
let actual = 0;

function agregar() {
    let dir = document.getElementById("direccion").value.trim();
    if (!dir) return;

    dir += ", Buenos Aires, Argentina";

    if (destinos.length < 10) {
        destinos.push(dir);
        document.getElementById("direccion").value = "";
        mostrar();
    }
}

function mostrar() {
    let lista = document.getElementById("lista");
    lista.innerHTML = "";
    destinos.forEach((d, i) => {
        lista.innerHTML += `<div class="item">${i+1}. ${d}</div>`;
    });
}

function invertir() {
    destinos.reverse();
    mostrar();
}

function iniciar() {
    if (destinos.length === 0) return;
    actual = 0;
    irADestino();
}

function siguiente() {
    actual++;
    if (actual < destinos.length) {
        irADestino();
    } else {
        document.getElementById("estado").innerText = "✔️ Entregas finalizadas";
    }
}

function irADestino() {
    let destino = destinos[actual];
    document.getElementById("estado").innerText = "🚗 Navegando a: " + destino;

    let url = `https://www.google.com/maps/dir/?api=1&destination=${encodeURIComponent(destino)}&travelmode=driving`;

    window.location.href = url;
}

navigator.geolocation.watchPosition(pos => {
    console.log("Ubicación:", pos.coords.latitude, pos.coords.longitude);
});
</script>

</body>
</html>
