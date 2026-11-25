<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Zero Music</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',system-ui,sans-serif}
:root{--c-primario:#00d4ff;--c-secundario:#00ffb8;--c-pausa:#00d4ff;--c-reproduciendo:#00ff88}

html,body{width:100%;height:100%;overflow:hidden;position:relative}

/* 🖼️ Fondo */
.fondo-imagen{position:absolute;inset:0;background-color:#0a0e27;background-image:url(https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?w=1920);background-size:cover;background-position:center;filter:blur(35px) brightness(0.15);transform:scale(1.1);z-index:1}
.fondo-sombra{position:absolute;inset:0;background:linear-gradient(180deg,rgba(10,14,39,.88),rgba(15,23,55,.92));z-index:2}

/* 🎵 Nombre App */
.app-nombre{position:absolute;top:16px;left:50%;transform:translateX(-50%);font-size:22px;font-weight:700;color:#fff;letter-spacing:2px;text-shadow:0 0 12px rgba(0,212,255,.35);z-index:10}
.app-nombre span{color:var(--c-primario)}

/* 📁 Botón Agregar */
.boton-agregar{position:absolute;top:14px;right:16px;width:48px;height:48px;border-radius:10px;background:rgba(0,212,255,.15);border:2px solid rgba(0,212,255,.5);color:#00d4ff;font-size:24px;cursor:pointer;display:flex;align-items:center;justify-content:center;z-index:10;transition:.2s}
.boton-agregar:active{background:rgba(0,212,255,.25);transform:scale(0.95)}
#entrada-audio{display:none}

/* 📦 Contenedor Principal — Ajustado a celular */
.contenedor-principal{position:relative;z-index:5;width:100%;height:100%;display:flex;align-items:center;justify-content:center;padding:70px 16px 30px}

.reproductor{width:100%;max-width:380px;background:rgba(15,18,30,.85);border-radius:24px;padding:22px;border:3px solid var(--c-pausa);transition:border-color .4s ease;box-shadow:0 10px 40px rgba(0,0,0,.45);backdrop-filter:blur(12px)}
.reproductor.reproduciendo{border-color:var(--c-reproduciendo);box-shadow:0 0 20px rgba(0,255,136,.18)}

/* 🖼️ Portada */
.portada{width:160px;height:160px;margin:0 auto 16px;border-radius:14px;overflow:hidden;box-shadow:0 5px 20px rgba(0,0,0,.35)}
.portada img{width:100%;height:100%;object-fit:cover;background:linear-gradient(135deg,#1e1e3a,#2a2a50)}

/* 📝 Info Canción */
.info-cancion{text-align:center;margin-bottom:16px;padding:0 8px}
.nombre-cancion{font-size:16px;font-weight:600;color:#fff;margin-bottom:3px;word-break:break-all;line-height:1.3}
.artista{color:#8899bb;font-size:13px}

/* ⏱️ Barra de Tiempo */
.barra-tiempo{width:100%;height:5px;background:rgba(80,120,200,.12);border-radius:3px;cursor:pointer;margin:8px 0}
.progreso{height:100%;width:0%;background:linear-gradient(90deg,#00d4ff,#0099cc);border-radius:3px;transition:width .1s linear}
.tiempos{display:flex;justify-content:space-between;font-size:11px;color:#7788aa;margin:4px 0 12px;font-family:monospace}

/* 🎚️ ECUALIZADOR — TODO DENTRO DEL BORDE */
.ecualizador{background:rgba(0,0,35,.35);border:1px solid rgba(80,140,220,.12);border-radius:12px;padding:12px;margin-bottom:16px;width:100%}
.eq-titulo{font-size:13px;color:#00ffb8;font-weight:500;margin-bottom:10px;text-align:center}
.eq-preset-botones{display:flex;justify-content:center;gap:6px;margin-bottom:12px;flex-wrap:wrap}
.eq-preset{padding:3px 8px;background:transparent;border:1px solid rgba(0,255,184,.2);color:#88ddbb;border-radius:5px;font-size:11px;cursor:pointer;transition:.2s}
.eq-preset.activo,.eq-preset:hover{background:rgba(0,255,184,.1);border-color:rgba(0,255,184,.4);color:#00ffb8}

.eq-sliders{display:flex;justify-content:space-between;align-items:center;height:80px;padding:0 4px}
.eq-columna{display:flex;flex-direction:column;align-items:center;gap:3px;flex:1}
.eq-valor{font-size:9px;color:#aabbcc;min-width:20px;text-align:center}
.eq-slider{width:60px;height:4px;-webkit-appearance:none;background:rgba(80,140,220,.15);border-radius:2px;transform:rotate(-90deg);transform-origin:center;cursor:pointer;margin:0 -28px}
.eq-slider::-webkit-slider-thumb{-webkit-appearance:none;width:13px;height:13px;background:#fff;border-radius:50%;border:2px solid #00ffb8;cursor:pointer}
.eq-etiqueta{font-size:8px;color:#8899aa;text-transform:uppercase;margin-top:4px}

/* 🔊 Volumen */
.volumen-fila{display:flex;align-items:center;gap:8px;margin-bottom:18px;width:100%}
.volumen-icon{font-size:15px}
.volumen-barra{flex:1;height:5px;-webkit-appearance:none;background:rgba(80,140,220,.15);border-radius:3px}
.volumen-barra::-webkit-slider-thumb{-webkit-appearance:none;width:13px;height:13px;background:#00d4ff;border-radius:50%;cursor:pointer}
.volumen-texto{font-size:11px;color:#8899aa;width:38px;text-align:right}

/* ▶️ Controles */
.controles{display:flex;align-items:center;justify-content:center;gap:14px}
.btn-control{border:none;background:transparent;color:#aabbdd;font-size:19px;cursor:pointer;transition:.2s;display:flex;align-items:center;justify-content:center}
.btn-prev,.btn-next{width:42px;height:42px;border-radius:50%;border:1px solid rgba(80,140,220,.2)}
.btn-prev:active,.btn-next:active{color:var(--c-primario)}
.btn-play{width:58px;height:58px;background:linear-gradient(135deg,#00d4ff,#0088aa);border-radius:50%;font-size:20px;color:#000;display:flex;align-items:center;justify-content:center;box-shadow:0 0 15px rgba(0,212,255,.22)}
.btn-play:active{transform:scale(0.97)}
</style>
</head>
<body>

<div class="fondo-imagen" id="fondoImagen"></div>
<div class="fondo-sombra"></div>

<div class="app-nombre">ZERO <span>MUSIC</span></div>

<label for="entrada-audio" class="boton-agregar" title="Seleccionar música">📁</label>
<input type="file" id="entrada-audio" accept="audio/*,.mp3,.wav,.flac,.ogg,.m4a" />

<div class="contenedor-principal">
  <div class="reproductor" id="cajaReproductor">
    <div class="portada">
      <img id="imagenPortada" src="https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?w=500" alt="Portada">
    </div>

    <div class="info-cancion">
      <div class="nombre-cancion" id="nombreCancion">Sin canción</div>
      <div class="artista" id="artistaInfo">Toca la carpeta arriba 📁</div>
    </div>

    <div class="barra-tiempo" id="barraTiempo">
      <div class="progreso" id="barraProgreso"></div>
    </div>
    <div class="tiempos">
      <span id="tiempoActual">00:00</span>
      <span id="tiempoTotal">00:00</span>
    </div>

    <!-- 🎚️ ECUALIZADOR — DENTRO DEL BORDE -->
    <div class="ecualizador">
      <div class="eq-titulo">🎚️ Ecualizador</div>
      <div class="eq-preset-botones">
        <button class="eq-preset activo" data-valor="plano">Plano</button>
        <button class="eq-preset" data-valor="bajos">Bajos</button>
        <button class="eq-preset" data-valor="voces">Voces</button>
        <button class="eq-preset" data-valor="agudos">Agudos</button>
      </div>
      <div class="eq-sliders">
        <div class="eq-columna"><div class="eq-valor" id="val60">0</div><input type="range" class="eq-slider" id="eq60" min="-12" max="12" value="0"><div class="eq-etiqueta">60Hz</div></div>
        <div class="eq-columna"><div class="eq-valor" id="val250">0</div><input type="range" class="eq-slider" id="eq250" min="-12" max="12" value="0"><div class="eq-etiqueta">250Hz</div></div>
        <div class="eq-columna"><div class="eq-valor" id="val1k">0</div><input type="range" class="eq-slider" id="eq1k" min="-12" max="12" value="0"><div class="eq-etiqueta">1KHz</div></div>
        <div class="eq-columna"><div class="eq-valor" id="val4k">0</div><input type="range" class="eq-slider" id="eq4k" min="-12" max="12" value="0"><div class="eq-etiqueta">4KHz</div></div>
        <div class="eq-columna"><div class="eq-valor" id="val16k">0</div><input type="range" class="eq-slider" id="eq16k" min="-12" max="12" value="0"><div class="eq-etiqueta">16KHz</div></div>
      </div>
    </div>

    <div class="volumen-fila">
      <span class="volumen-icon">🔊</span>
      <input type="range" class="volumen-barra" id="volumen" min="0" max="1" step="0.01" value="0.8">
      <span class="volumen-texto" id="textoVol">80%</span>
    </div>

    <div class="controles">
      <button class="btn-control btn-prev" id="btnPrevio">⏮</button>
      <button class="btn-control btn-play" id="btnPlay">▶</button>
      <button class="btn-control btn-next" id="btnSiguiente">⏭</button>
    </div>
  </div>
</div>

<audio id="audioElement" crossorigin="anonymous" preload="metadata">

<script>
const entradaAudio = document.getElementById('entrada-audio');
const fondoImagen = document.getElementById('fondoImagen');
const cajaReproductor = document.getElementById('cajaReproductor');
const audioElement = document.getElementById('audioElement');
const nombreCancion = document.getElementById('nombreCancion');
const artistaInfo = document.getElementById('artistaInfo');
const barraProgreso = document.getElementById('barraProgreso');
const barraTiempo = document.getElementById('barraTiempo');
const tiempoActual = document.getElementById('tiempoActual');
const tiempoTotal = document.getElementById('tiempoTotal');
const btnPlay = document.getElementById('btnPlay');
const volumen = document.getElementById('volumen');
const textoVol = document.getElementById('textoVol');

const filtros = {};
let ctxAudio = null;

function formatearTiempo(segundos) {
  if (!segundos || isNaN(segundos)) return '00:00';
  const m = Math.floor(segundos / 60);
  const s = Math.floor(segundos % 60);
  return `${m < 10 ? '0' : ''}${m}:${s < 10 ? '0' : ''}${s}`;
}

// 📁 Agregar canción
entradaAudio.addEventListener('change', function(e) {
  const archivos = e.target.files;
  if (!archivos || archivos.length === 0) return;

  const archivo = archivos[0];
  const esAudio = archivo.type.startsWith('audio/') || /\.(mp3|wav|flac|ogg|m4a)$/i.test(archivo.name);

  if (!esAudio) {
    alert('❌ Archivo no válido\nUsa: .mp3, .wav, .flac, .ogg o .m4a');
    return;
  }

  nombreCancion.textContent = archivo.name.replace(/\.[^/.]+$/, '');
  artistaInfo.textContent = '✅ ¡Listo! Toca ▶ para reproducir';

  const url = URL.createObjectURL(archivo);
  audioElement.src = url;

  if (!ctxAudio) iniciarEcualizador();
});

// 🎚️ Inicializar ecualizador
function iniciarEcualizador() {
  try {
    ctxAudio = new (window.AudioContext || window.webkitAudioContext)();
    const fuente = ctxAudio.createMediaElementSource(audioElement);

    const config = {
      eq60: { freq: 80, type: 'lowshelf' },
      eq250: { freq: 250, type: 'peaking' },
      eq1k: { freq: 1000, type: 'peaking' },
      eq4k: { freq: 4000, type: 'peaking' },
      eq16k: { freq: 16000, type: 'highshelf' }
    };

    let nodo = fuente;
    Object.keys(config).forEach(id => {
      filtros[id] = ctxAudio.createBiquadFilter();
      filtros[id].type = config[id].type;
      filtros[id].frequency.value = config[id].freq;
      filtros[id].gain.value = 0;
      nodo.connect(filtros[id]);
      nodo = filtros[id];

      const slider = document.getElementById(id);
      const valorDisplay = document.getElementById('val' + id.slice(2));
      slider.addEventListener('input', function() {
        const val = parseFloat(this.value);
        valorDisplay.textContent = val;
        if (filtros[id]) filtros[id].gain.value = val;
      });
    });
    nodo.connect(ctxAudio.destination);
  } catch (err) {
    console.log('Ecualizador:', err.message);
  }
}

// Preajustes del ecualizador
document.querySelectorAll('.eq-preset').forEach(boton => {
  boton.addEventListener('click', function() {
    document.querySelectorAll('.eq-preset').forEach(b => b.classList.remove('activo'));
    this.classList.add('activo');

    const tipo = this.dataset.valor;
    const vals = {
      plano: { eq60:0, eq250:0, eq1k:0, eq4k:0, eq16k:0 },
      bajos: { eq60:10, eq250:6, eq1k:0, eq4k:-4, eq16k:-6 },
      voces: { eq60:-3, eq250:3, eq1k:7, eq4k:4, eq16k:0 },
      agudos: { eq60:-5, eq250:-3, eq1k:0, eq4k:6, eq16k:10 }
    }[tipo];

    Object.keys(vals).forEach(id => {
      const slider = document.getElementById(id);
      const valorDisplay = document.getElementById('val' + id.slice(2));
      slider.value = vals[id];
      valorDisplay.textContent = vals[id];
      if (filtros[id]) filtros[id].gain.value = vals[id];
    });
  });
});

// ▶️ Reproducir/Pausar
btnPlay.addEventListener('click', function() {
  if (!audioElement.src) {
    alert('📁 Primero elige una canción con la carpeta');
    return;
  }
  if (ctxAudio && ctxAudio.state === 'suspended') ctxAudio.resume();
  audioElement.paused ? audioElement.play() : audioElement.pause();
});

// Cambiar borde al reproducir
audioElement.addEventListener('play', () => {
  cajaReproductor.classList.add('reproduciendo');
  btnPlay.textContent = '⏸';
});
audioElement.addEventListener('pause', () => {
  cajaReproductor.classList.remove('reproduciendo');
  btnPlay.textContent = '▶';
});
audioElement.addEventListener('ended', () => {
  cajaReproductor.classList.remove('reproduciendo');
  btnPlay.textContent = '▶';
});

// ⏱️ Tiempo
audioElement.addEventListener('timeupdate', () => {
  if (audioElement.duration) {
    barraProgreso.style.width = (audioElement.currentTime / audioElement.duration * 100) + '%';
  }
  tiempoActual.textContent = formatearTiempo(audioElement.currentTime);
});
audioElement.addEventListener('loadedmetadata', () => {
  tiempoTotal.textContent = formatearTiempo(audioElement.duration);
});

barraTiempo.addEventListener('click', e => {
  if (!audioElement.duration) return;
  const rect = barraTiempo.getBoundingClientRect();
  audioElement.currentTime = ((e.clientX - rect.left) / rect.width) * audioElement.duration;
});

// 🔊 Volumen
volumen.addEventListener('input', function() {
  const val = Math.round(parseFloat(this.value) * 100);
  textoVol.textContent = val + '%';
  audioElement.volume = parseFloat(this.value);
});
</script>
</body>
</html>
