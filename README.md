<html>
<!DOCTYPE html>   
<head>
<meta charset="utf-8">
<title>Rutas de Entrega</title>
<style>
body { font-family: Arial; background:#111; color:#fff; text-align:center; }
input, button { padding:10px; margin:5px; width:85%; }
.item { background:#222; padding:8px; margin:5px; }
</style>
</head>
<body>

<h2>📦 Entregas</h2>

<input id="direccion" placeholder="Ej: San Benito 681, Merlo Buenos Aires">
<button onclick="agregar()">Agregar</button>

<div id="lista"></div>

<button onclick="invertir()">Invertir orden</button>
<button onclick="iniciar()">Iniciar</button>
<button onclick="siguiente()">Siguiente</button>

<p id="estado"></p>

<script>
let destinos = [];
let actual = 0;

function agregar() {
    let dir = document.getElementById("direccion").value.trim();

    if (!dir) return;

    // 🔥 Forzar contexto Argentina (mejora precisión)
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
        document.getElementById("estado").innerText = "✔️ Terminaste todas las entregas";
    }
}

function irADestino() {
    let destino = destinos[actual];
    document.getElementById("estado").innerText = "🚗 Navegando a: " + destino;

    // 🔥 URL navegación directa (IMPORTANTE)
    let url = `https://www.google.com/maps/dir/?api=1&destination=${encodeURIComponent(destino)}&travelmode=driving`;

    window.location.href = url; // abre directo en Maps
}

// ubicación en tiempo real (opcional)
navigator.geolocation.watchPosition(pos => {
    console.log("Ubicación:", pos.coords.latitude, pos.coords.longitude);
});
</script>

</body>
</html>
