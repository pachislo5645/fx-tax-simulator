<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>海外FX税金シミュレーター</title>
  <style>
    body { font-family: sans-serif; padding: 1em; }
    input, button { margin: 0.5em 0; padding: 0.5em; width: 100%; max-width: 300px; }
    #result { margin-top: 1em; font-weight: bold; }
  </style>
</head>
<body>
  <h3>海外FX税金シミュレーター</h3>
  <p>年間の利益を入力してください（日本円）:</p>

  <input type="number" id="profit" placeholder="例: 1500000">
  <button onclick="calcTax()">税金を計算する</button>

  <div id="result"></div>

  <script>
    function calcTax() {
      const profit = parseFloat(document.getElementById("profit").value);
      const resultDiv = document.getElementById("result");

      if (isNaN(profit) || profit <= 0) {
        resultDiv.innerHTML = "有効な利益金額を入力してください。";
        return;
      }

      const brackets = [
        { limit: 1950000, rate: 0.05, deduction: 0 },
        { limit: 3300000, rate: 0.10, deduction: 97500 },
        { limit: 6950000, rate: 0.20, deduction: 427500 },
        { limit: 9000000, rate: 0.23, deduction: 636000 },
        { limit: 18000000, rate: 0.33, deduction: 1536000 },
        { limit: 40000000, rate: 0.40, deduction: 2796000 },
        { limit: Infinity, rate: 0.45, deduction: 4796000 }
      ];

      const basicDeduction = 480000;
      const taxable = Math.max(0, profit - basicDeduction);

      let incomeTax = 0;
      for (let b of brackets) {
        if (taxable <= b.limit) {
          incomeTax = taxable * b.rate - b.deduction;
          break;
        }
      }

      incomeTax = Math.floor(incomeTax);
      const residentTax = Math.floor(taxable * 0.10);
      const totalTax = incomeTax + residentTax;

      resultDiv.innerHTML = `
        <p>課税所得: <strong>${taxable.toLocaleString()}円</strong></p>
        <p>所得税（概算）: <strong>${incomeTax.toLocaleString()}円</strong></p>
        <p>住民税（概算）: <strong>${residentTax.toLocaleString()}円</strong></p>
        <p><strong>合計税額: ${totalTax.toLocaleString()}円</strong></p>
      `;
    }
  </script>
</body>
</html>
