<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PoneXiom - RabbitHole 🐰</title>
    <style>
        body { font-family: sans-serif; max-width: 500px; margin: 20px auto; padding: 10px; background: #f4f4f9; text-align: center; }
        select, button, textarea { width: 100%; padding: 12px; margin: 8px 0; box-sizing: border-box; font-size: 16px; border-radius: 6px; border: 1px solid #ccc; }
        button { background: #28a745; color: white; font-weight: bold; border: none; cursor: pointer; }
        .danger { background: #dc3545; }
        #logs { text-align: left; background: white; padding: 10px; border-radius: 6px; max-height: 200px; overflow-y: auto; font-size: 14px; }
    </style>
</head>
<body>
    <h2>🐰 Proyecto RabbitHole</h2>
    <p>Sistema de Contingencia Ponejos</p>

    <form id="emergencyForm">
        <select id="remitente" required>
            <option value="">¿Quién reporta?</option>
            <option value="Pone">Pone ♀️</option>
            <option value="Poshe">Poshe ♂️</option>
            <option value="Ponejitas">Ponejitas 👧</option>
        </select>

        <select id="estado" required>
            <option value="BIEN">🟢 ESTOY BIEN</option>
            <option value="RESGUARDADA">🔵 RESGUARDADO / SEGURO</option>
            <option value="HERIDA">⚠️ HERIDO (Monitoreable)</option>
            <option value="AUXILIO">🚨 INMEDIATO - NECESITO AUXILIO</option>
        </select>

        <button type="button" onclick="obtenerCoordenadas()">📍 Adjuntar Coordenadas Actuales</button>
        <input type="text" id="coordenadas" placeholder="Coordenadas (Esperando GPS...)" readonly>

        <textarea id="mensaje" placeholder="Mensaje corto (Ej: En casa de la abuela, sin luz)"></textarea>
        <button type="submit" class="danger">ENVIAR REPORTE A LA MADRIGUERA</button>
    </form>

    <h3>📋 Últimos Reportes de la Familia</h3>
    <div id="logs">Cargando bitácora...</div>

    <script>
        // Reemplaza esto con la URL que te dará Alibaba en el Paso 3
        const ALIBABA_FC_URL = "TU_URL_DE_ALIBABA_FUNCTION_COMPUTE";

        function obtenerCoordenadas() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(pos => {
                    document.getElementById('coordenadas').value = `${pos.coords.latitude}, ${pos.coords.longitude}`;
                }, () => alert("Error al obtener GPS. Envía sin coordenadas."));
            }
        }

        document.getElementById('emergencyForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const datos = {
                remitente: document.getElementById('remitente').value,
                estado: document.getElementById('estado').value,
                coordenadas: document.getElementById('coordenadas').value,
                mensaje: document.getElementById('mensaje').value
            };

            const res = await fetch(ALIBABA_FC_URL, { method: 'POST', body: JSON.stringify(datos) });
            if (res.ok) { alert("¡Reporte enviado exitosamente!"); cargarReportes(); }
        });

        async function cargarReportes() {
            const res = await fetch(ALIBABA_FC_URL);
            if (res.ok) {
                const datos = await res.json();
                document.getElementById('logs').innerHTML = datos.map(r => 
                    `<b>[${r.remitente}]</b> Estado: ${r.estado} <br> 📍 ${r.coordenadas || 'Sin GPS'} <br> 📝 ${r.mensaje} <hr>`
                ).join('');
            }
        }
        window.onload = cargarReportes;
    </script>
</body>
</html>
