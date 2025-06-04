<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>เพ่ยเงินชิล - บันทึกรายรับรายจ่าย</title>
  <style>
    body {
      font-family: 'Sarabun', sans-serif;
      max-width: 600px;
      margin: auto;
      padding: 20px;
      background: #fefefe;
      color: #333;
    }

    h1 {
      text-align: center;
      color: #00796B;
    }

    form input, form select, form button {
      padding: 10px;
      margin: 5px 0;
      width: 100%;
      box-sizing: border-box;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
    }

    th, td {
      border: 1px solid #ccc;
      padding: 8px;
      text-align: center;
    }

    .income { color: green; }
    .expense { color: red; }

    button {
      background-color: #00796B;
      color: white;
      border: none;
      cursor: pointer;
    }

    button:hover {
      background-color: #005B4F;
    }
  </style>
</head>
<body>

  <h1>เพ่ยเงินชิล 💸</h1>
  <p style="text-align: center;">ระบบบันทึกรายรับรายจ่ายอย่างชิล ๆ</p>

  <form id="form">
    <input type="text" id="desc" placeholder="รายการ (เช่น ค่าอาหาร)">
    <input type="number" id="amount" placeholder="จำนวนเงิน">
    <select id="type">
      <option value="income">รายรับ</option>
      <option value="expense">รายจ่าย</option>
    </select>
    <button type="submit">เพิ่มรายการ</button>
  </form>

  <h3>สรุป</h3>
  <p>รายรับ: <span id="totalIncome">0</span> บาท</p>
  <p>รายจ่าย: <span id="totalExpense">0</span> บาท</p>
  <p>คงเหลือ: <span id="balance">0</span> บาท</p>

  <table>
    <thead>
      <tr>
        <th>รายการ</th>
        <th>จำนวนเงิน</th>
        <th>ประเภท</th>
      </tr>
    </thead>
    <tbody id="list"></tbody>
  </table>

  <script>
    const form = document.getElementById('form');
    const list = document.getElementById('list');
    const totalIncome = document.getElementById('totalIncome');
    const totalExpense = document.getElementById('totalExpense');
    const balance = document.getElementById('balance');

    let items = JSON.parse(localStorage.getItem('peiyenchill')) || [];

    function render() {
      list.innerHTML = '';
      let income = 0, expense = 0;

      items.forEach(item => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${item.desc}</td>
          <td class="${item.type}">${item.amount}</td>
          <td>${item.type === 'income' ? 'รายรับ' : 'รายจ่าย'}</td>
        `;
        list.appendChild(row);

        if (item.type === 'income') income += item.amount;
        else expense += item.amount;
      });

      totalIncome.textContent = income;
      totalExpense.textContent = expense;
      balance.textContent = income - expense;
      localStorage.setItem('peiyenchill', JSON.stringify(items));
    }

    form.onsubmit = function(e) {
      e.preventDefault();
      const desc = document.getElementById('desc').value.trim();
      const amount = parseFloat(document.getElementById('amount').value);
      const type = document.getElementById('type').value;

      if (desc && amount) {
        items.push({ desc, amount, type });
        render();
        form.reset();
      }
    };

    render();
  </script>

</body>
</html>
