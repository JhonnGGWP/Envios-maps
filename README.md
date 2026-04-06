<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Rutas de Entrega</title>
<style>
body { font-family: Arial; background:#111; color:#fff; text-align:center; }
input, button { padding:10px; margin:5px; width:80%; }
.lista { margin-top:10px; }
.item { background:#222; padding:8px; margin:5px; }
</style>
</head>
<body>

<h2>📦 Entregas (máx 10)</h2>

<input id="direccion" placeholder="Ingresar dirección">
<button onclick="agregar()">Agregar</button>

<div class="lista" id="lista"></div>

<button onclick="invertir()">Invertir orden</button>
<button onclick="iniciar()">Iniciar ruta</button>
<button onclick="siguiente()">Siguiente destino</button>

<p id="estado"></p>

<script>
let destinos = [];
let actual = 0;

function agregar() {
    let dir = document.getElementById("direccion").value;
    if (dir && destinos.length < 10) {
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
    document.getElementById("estado").innerText = "Destino: " + destino;

    navigator.geolocation.getCurrentPosition(pos => {
        let lat = pos.coords.latitude;
        let lon = pos.coords.longitude;

        let url = `https://www.google.com/maps/dir/${lat},${lon}/${encodeURIComponent(destino)}`;
        window.open(url, "_blank");
    });
}

// Ubicación en tiempo real (solo muestra en consola)
navigator.geolocation.watchPosition(pos => {
    console.log("Ubicación:", pos.coords.latitude, pos.coords.longitude);
});
</script>

</body>
</html>
