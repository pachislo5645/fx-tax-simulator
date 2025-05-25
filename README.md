<h3>海外FX税金計算シミュレーター</h3>
<p>海外FXの年間利益を入力すると、税金の概算を計算します（※住民税含む、概算です）。</p>

<label for="profit">年間利益（円）:</label>
<input type="number" id="profit" placeholder="例: 1500000">
<button onclick="calcTax()">税金を計算する</button>

<h4>結果:</h4>
<div id="result"></div>

<script>
function calcTax() {
  const profit = parseFloat(document.getElementById("profit").value);
  const resultDiv = document.getElementById("result");

  if (isNaN(profit) || profit <= 0) {
    resultDiv.innerHTML = "有効な利益金額を入力してください。";
    return;
  }

  // 税率テーブル（所得税）: 概算（2024年度版・超過累進課税）
  const brackets = [
    { limit: 1950000, rate: 0.05, deduction: 0 },
    { limit: 3300000, rate: 0.10, deduction: 97500 },
    { limit: 6950000, rate: 0.20, deduction: 427500 },
    { limit: 9000000, rate: 0.23, deduction: 636000 },
    { limit: 18000000, rate: 0.33, deduction: 1536000 },
    { limit: 40000000, rate: 0.40, deduction: 2796000 },
    { limit: Infinity, rate: 0.45, deduction: 4796000 }
  ];

  // 基礎控除（簡略化）
  const basicDeduction = 480000;

  // 課税所得
  const taxable = Math.max(0, profit - basicDeduction);

  // 所得税率の決定
  let incomeTax = 0;
  for (let b of brackets) {
    if (taxable <= b.limit) {
      incomeTax = taxable * b.rate - b.deduction;
      break;
    }
  }

  incomeTax = Math.floor(incomeTax);

  // 住民税（10%一律想定）
  const residentTax = Math.floor(taxable * 0.10);

  // 合計
  const totalTax = incomeTax + residentTax;

  resultDiv.innerHTML = `
    <p>課税所得: <strong>${taxable.toLocaleString()}円</strong></p>
    <p>所得税（概算）: <strong>${incomeTax.toLocaleString()}円</strong></p>
    <p>住民税（概算）: <strong>${residentTax.toLocaleString()}円</strong></p>
    <p><strong>合計税額: ${totalTax.toLocaleString()}円</strong></p>
  `;
}
</script>
