<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nhân hai ma trận</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --primary: #007bff;
      --secondary: #6c757d;
      --background: #f8f9fa;
      --card-bg: #ffffff;
      --input-bg: #e9ecef;
      --border-color: #ced4da;
    }

    body {
      font-family: 'Roboto', sans-serif;
      background-color: var(--background);
      margin: 0;
      padding: 20px;
    }

    h1 {
      text-align: center;
      color: var(--primary);
    }

    .section {
      max-width: 1000px;
      margin: auto;
      background: var(--card-bg);
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      margin-bottom: 30px;
    }

    .section h3 {
      color: var(--secondary);
      text-align: center;
    }

    label, input[type="number"] {
      font-size: 16px;
      margin-right: 8px;
    }

    input[type="number"] {
      width: 60px;
      padding: 6px;
      background: var(--input-bg);
      border: 1px solid var(--border-color);
      border-radius: 6px;
      text-align: center;
    }

    .matrix-container {
      display: flex;
      justify-content: space-between;
      gap: 40px;
      margin: 30px 0;
      flex-wrap: wrap;
    }

    .matrix {
      display: grid;
      gap: 8px;
    }

    .matrix-wrapper {
      flex: 1 1 45%;
    }

    button {
      padding: 10px 20px;
      background-color: var(--primary);
      color: white;
      font-size: 16px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background-color: #0056b3;
    }

    table {
      margin: auto;
      border-collapse: collapse;
    }

    td {
      border: 1px solid #000;
      padding: 12px;
      min-width: 50px;
      background-color: #fff;
    }

    #matrixResult {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>Máy tính nhân ma trận</h1>

  <div class="section">
    <label>Kích thước ma trận A (hàng x cột):
      <input type="number" id="rowsA" value="2" min="1"> x
      <input type="number" id="colsA" value="2" min="1">
    </label>
    <br><br>
    <label>Kích thước ma trận B (hàng x cột):
      <input type="number" id="rowsB" value="2" min="1"> x
      <input type="number" id="colsB" value="2" min="1">
    </label>
    <br><br>
    <div style="text-align: center;">
      <button onclick="createMatrices()">Tạo ma trận</button>
    </div>
  </div>

  <div class="section matrix-container">
    <div class="matrix-wrapper">
      <h3>Ma trận A</h3>
      <div id="matrixA" class="matrix"></div>
    </div>
    <div class="matrix-wrapper">
      <h3>Ma trận B</h3>
      <div id="matrixB" class="matrix"></div>
    </div>
  </div>

  <div class="section" style="text-align: center;">
    <button onclick="multiplyMatrices()">Nhân hai ma trận</button>
  </div>

  <div class="section" id="result">
    <h3>Kết quả</h3>
    <div id="matrixResult"></div>
  </div>

  <script>
    function createMatrixInputs(containerId, rows, cols) {
      const container = document.getElementById(containerId);
      container.innerHTML = '';
      container.style.gridTemplateColumns = `repeat(${cols}, auto)`;
      for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
          const input = document.createElement('input');
          input.type = 'number';
          input.value = '0';
          input.id = `${containerId}_${i}_${j}`;
          container.appendChild(input);
        }
      }
    }

    function createMatrices() {
      const rowsA = parseInt(document.getElementById('rowsA').value);
      const colsA = parseInt(document.getElementById('colsA').value);
      const rowsB = parseInt(document.getElementById('rowsB').value);
      const colsB = parseInt(document.getElementById('colsB').value);

      if (colsA !== rowsB) {
        alert('Số cột của ma trận A phải bằng số hàng của ma trận B để nhân được.');
        return;
      }

      createMatrixInputs('matrixA', rowsA, colsA);
      createMatrixInputs('matrixB', rowsB, colsB);
    }

    function multiplyMatrices() {
      const rowsA = parseInt(document.getElementById('rowsA').value);
      const colsA = parseInt(document.getElementById('colsA').value);
      const colsB = parseInt(document.getElementById('colsB').value);

      const A = [];
      const B = [];

      for (let i = 0; i < rowsA; i++) {
        A[i] = [];
        for (let j = 0; j < colsA; j++) {
          A[i][j] = parseFloat(document.getElementById(`matrixA_${i}_${j}`).value);
        }
      }

      for (let i = 0; i < colsA; i++) {
        B[i] = [];
        for (let j = 0; j < colsB; j++) {
          B[i][j] = parseFloat(document.getElementById(`matrixB_${i}_${j}`).value);
        }
      }

      const result = [];
      for (let i = 0; i < rowsA; i++) {
        result[i] = [];
        for (let j = 0; j < colsB; j++) {
          let sum = 0;
          for (let k = 0; k < colsA; k++) {
            sum += A[i][k] * B[k][j];
          }
          result[i][j] = sum;
        }
      }

      const resultContainer = document.getElementById('matrixResult');
      let html = '<table>';
      for (let i = 0; i < result.length; i++) {
        html += '<tr>';
        for (let j = 0; j < result[i].length; j++) {
          html += `<td>${result[i][j]}</td>`;
        }
        html += '</tr>';
      }
      html += '</table>';
      resultContainer.innerHTML = html;
    }
  </script>
</body>
</html>
