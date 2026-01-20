<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Prefeito da Cidade</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #121212;
    color: white;
}

.painel {
    padding: 10px;
    background: #1e1e1e;
    position: fixed;
    width: 100%;
    z-index: 10;
    font-size: 14px;
}

button {
    margin: 3px;
    padding: 7px;
    border: none;
    border-radius: 6px;
}

.casa { background: #4CAF50; }
.hospital { background: #2196F3; }
.delegacia { background: #f44336; }
.veiculo { background: #FF9800; }

.mapa-container {
    position: absolute;
    top: 110px;
    bottom: 0;
    left: 0;
    right: 0;
    overflow: hidden;
    touch-action: none;
}

.mapa {
    display: grid;
    grid-template-columns: repeat(20, 60px);
    grid-auto-rows: 60px;
    gap: 2px;
    position: absolute;
    transform-origin: 0 0;
}

.bloco {
    background: #2e2e2e;
    border-radius: 4px;
    font-size: 26px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.estrada {
    background: #555;
    font-size: 18px;
}
</style>
</head>

<body>

<div class="painel">
💰 <span id="dinheiro">1000</span> |
👨‍👩‍👧‍👦 <span id="moradores">20</span> |
😊 <span id="felicidade">50</span>%<br>

<button class="casa" onclick="selecionar('🏠',200)">Casa</button>
<button class="hospital" onclick="selecionar('🏥',800)">Hospital</button>
<button class="delegacia" onclick="selecionar('🚓',600)">Delegacia</button>
<button class="veiculo" onclick="selecionar('🚗',300)">Transporte</button>
<button onclick="zoomIn()">＋</button>
<button onclick="zoomOut()">－</button>
</div>

<div class="mapa-container" id="container">
  <div class="mapa" id="mapa"></div>
</div>

<script>
const linhas = 20, colunas = 20;
let dinheiro = 1000, moradores = 20, felicidade = 50;
let selecionado = null, custoSelecionado = 0;
let cidade = [];

const mapa = document.getElementById("mapa");

/* ---------- MAPA ---------- */
function criarMapa() {
    mapa.innerHTML = "";
    cidade.forEach((item, i) => {
        const bloco = document.createElement("div");
        bloco.className = "bloco" + (item === "🛣️" ? " estrada" : "");
        bloco.textContent = item;
        bloco.onclick = () => construir(i);
        mapa.appendChild(bloco);
    });
}

/* ---------- CONSTRUÇÃO ---------- */
function selecionar(icone, custo) {
    selecionado = icone;
    custoSelecionado = custo;
}

function construir(i) {
    if (!selecionado || cidade[i] !== "" || dinheiro < custoSelecionado) return;

    cidade[i] = selecionado;
    dinheiro -= custoSelecionado;

    if (selecionado === "🏠") moradores += 5;
    if (selecionado === "🏥") felicidade += 5;
    if (selecionado === "🚓") felicidade += 3;
    if (selecionado === "🚗") felicidade += 2;

    criarEstradas(i);
    salvarJogo();
    atualizarTela();
    criarMapa();
}

/* ---------- ESTRADAS ---------- */
function criarEstradas(i) {
    const vizinhos = [
        i - colunas, // cima
        i + colunas, // baixo
        i - 1,       // esquerda
        i + 1        // direita
    ];

    vizinhos.forEach(v => {
        if (cidade[v] && cidade[v] !== "" && cidade[v] !== "🛣️") {
            const meio = Math.floor((i + v) / 2);
            if (cidade[meio] === "") cidade[meio] = "🛣️";
        }
    });
}

/* ---------- SAVE AUTOMÁTICO ---------- */
function salvarJogo() {
    localStorage.setItem("cidadeSave", JSON.stringify({
        dinheiro, moradores, felicidade, cidade
    }));
}

function carregarJogo() {
    const save = JSON.parse(localStorage.getItem("cidadeSave"));
    if (save) {
        dinheiro = save.dinheiro;
        moradores = save.moradores;
        felicidade = save.felicidade;
        cidade = save.cidade;
