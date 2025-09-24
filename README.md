
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Calculadora de Resistividad</title>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script id="MathJax-script" async
          src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 30px;
      background: #f9f9f9;
    }
    h1, h2 {
      text-align: center;
      color: #333;
    }
    .grupo {
      text-align: center;
      margin-bottom: 20px;
      font-weight: bold;
      color: #444;
      white-space: pre-line;
    }
    .container {
      max-width: 700px;
      margin: auto;
      background: #fff;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }
    label {
      display: block;
      margin-top: 10px;
      font-weight: bold;
    }
    input, select, button {
      width: 100%;
      padding: 8px;
      margin-top: 5px;
      border-radius: 8px;
      border: 1px solid #ccc;
      font-size: 1rem;
    }
    button {
      margin-top: 15px;
      background-color: #007bff;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
    .results {
      margin-top: 25px;
      background: #f4f4f4;
      padding: 15px;
      border-radius: 8px;
    }
    .resultado-final {
      background: #d4edda;
      border-left: 6px solid #28a745;
      padding: 10px;
      margin: 10px 0;
      font-weight: bold;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <h1>Calculadora de Resistividad</h1>
  <div class="grupo">
    Este código fue creado por el grupo:<br><br>
    Domínguez Medina Flor María<br>
    Moreno Caldas José Manuel<br>
    Jaimes Trujillo Jhon<br>
    Valdivia Rufino Merary Yamila<br>
    Vilchez Tapia Christian Piero
  </div>
  <div class="container">
    <label for="material">Selecciona el material:</label>
    <select id="material">
      <option value="cobre">Cobre</option>
      <option value="aluminio">Aluminio</option>
    </select>

    <label for="longitud">Longitud \(L\) (m):</label>
    <input type="number" id="longitud" step="0.01">

    <label for="diametro">Diámetro \(d\) (mm):</label>
    <input type="number" id="diametro" step="0.001">

    <label for="resistencia">Resistencia \(R\) (Ω):</label>
    <input type="number" id="resistencia" step="0.01">

    <button onclick="calcular()">Calcular</button>

    <div class="results" id="resultados"></div>
  </div>

  <script>
    // Función para convertir números a notación científica LaTeX
    function toScientificLatex(num, decimales = 2) {
      if (num === 0) return "0";
      const exp = Math.floor(Math.log10(Math.abs(num)));
      const mant = (num / Math.pow(10, exp)).toFixed(decimales);
      return `${mant} \\times 10^{${exp}}`;
    }

    function calcular() {
      const material = document.getElementById("material").value;
      const L = parseFloat(document.getElementById("longitud").value);
      const d_mm = parseFloat(document.getElementById("diametro").value);
      const R = parseFloat(document.getElementById("resistencia").value);

      if (isNaN(L) || isNaN(d_mm) || isNaN(R)) {
        alert("Por favor, completa todos los campos numéricos.");
        return;
      }

      const d = d_mm / 1000; // convertir a metros
      const A = Math.PI * Math.pow(d, 2) / 4;
      const rho_exp = (R * A) / L;

      let rho_teo = 0;
      if (material === "cobre") rho_teo = 1.68e-8;
      if (material === "aluminio") rho_teo = 2.65e-8;

      const E_abs = Math.abs(rho_exp - rho_teo);
      const E_pct = (E_abs / rho_teo) * 100;

      const resultados = `
      <h2>Resultados (${material.charAt(0).toUpperCase() + material.slice(1)})</h2>

      <p><b>1. Cálculo del área:</b></p>
      <p>\\[ A = \\frac{\\pi d^2}{4} \\]</p>
      <p>\\[ A = \\frac{\\pi (${d.toFixed(6)})^2}{4} = ${toScientificLatex(A,3)} \\, m^2 \\]</p>
      <div class="resultado-final">Área: \\(${toScientificLatex(A,3)} \\, m^2\\)</div>

      <p><b>2. Resistividad experimental:</b></p>
      <p>\\[ \\rho_{exp} = \\frac{R \\cdot A}{L} \\]</p>
      <p>\\[ \\rho_{exp} = \\frac{${R} \\times ${toScientificLatex(A,3)}}{${L}} 
      = ${toScientificLatex(rho_exp,3)} \\, \\Omega \\cdot m \\]</p>
      <div class="resultado-final">Resistividad experimental: \\(${toScientificLatex(rho_exp,3)} \\, \\Omega \\cdot m\\)</div>

      <p><b>3. Resistividad teórica:</b></p>
      <p>\\[ \\rho_{teo} = ${toScientificLatex(rho_teo,2)} \\, \\Omega \\cdot m \\]</p>
      <div class="resultado-final">Resistividad teórica: \\(${toScientificLatex(rho_teo,2)} \\, \\Omega \\cdot m\\)</div>

      <p><b>4. Error absoluto:</b></p>
      <p>\\[ E_{abs} = |\\rho_{exp} - \\rho_{teo}| \\]</p>
      <p>\\[ E_{abs} = |${toScientificLatex(rho_exp,3)} - ${toScientificLatex(rho_teo,2)}| 
      = ${toScientificLatex(E_abs,3)} \\, \\Omega \\cdot m \\]</p>
      <div class="resultado-final">Error absoluto: \\(${toScientificLatex(E_abs,3)} \\, \\Omega \\cdot m\\)</div>

      <p><b>5. Error porcentual:</b></p>
      <p>\\[ E_{\\%} = \\frac{E_{abs}}{\\rho_{teo}} \\times 100 \\]</p>
      <p>\\[ E_{\\%} = \\frac{${toScientificLatex(E_abs,3)}}{${toScientificLatex(rho_teo,2)}} \\times 100 
      = ${E_pct.toFixed(1)}\\% \\]</p>
      <div class="resultado-final">Error porcentual: ${E_pct.toFixed(1)}%</div>
      `;

      document.getElementById("resultados").innerHTML = resultados;
      MathJax.typesetPromise();
    }
  </script>
</body>
</html>
