<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>فاتورة - Fashion World</title>
    <style>
        body {
            font-family: 'Tahoma', Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 10px;
            color: #333;
        }
        .container {
            max-width: 100%;
            margin: 0 auto;
            background: #fff;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            border-bottom: 2px solid #eee;
            padding-bottom: 15px;
            margin-bottom: 15px;
        }
        .company-info h2 {
            margin: 0;
            color: #111;
            font-size: 20px;
        }
        .company-info p {
            margin: 3px 0 0;
            color: #666;
            font-size: 12px;
        }
        .invoice-title {
            font-size: 24px;
            font-weight: bold;
            color: #222;
        }
        .meta-section {
            margin-bottom: 20px;
            font-size: 13px;
        }
        .form-group {
            margin-bottom: 12px;
        }
        .form-group label {
            display: block;
            margin-bottom: 4px;
            font-weight: bold;
            font-size: 12px;
        }
        .form-group input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 14px;
            box-sizing: border-box;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 15px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
            font-size: 13px;
        }
        th {
            background-color: #f1f1f1;
            color: #333;
        }
        td input {
            width: 100%;
            padding: 6px;
            border: 1px solid #ccc;
            border-radius: 4px;
            text-align: center;
            box-sizing: border-box;
        }
        .totals-section {
            width: 100%;
            margin-top: 15px;
            font-size: 14px;
        }
        .total-row {
            display: flex;
            justify-content: space-between;
            padding: 6px 0;
            border-bottom: 1px solid #eee;
        }
        .total-row.final {
            font-weight: bold;
            font-size: 16px;
            border-top: 2px solid #333;
            border-bottom: 2px solid #333;
            margin-top: 5px;
            padding-top: 8px;
        }
        .btn {
            background-color: #2c3e50;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
            width: 100%;
            margin-top: 5px;
        }
        .btn-danger {
            background-color: #c0392b;
            padding: 4px 8px;
            font-size: 11px;
            width: auto;
        }
        .actions {
            margin-top: 20px;
            text-align: center;
        }
        .footer-note {
            text-align: center;
            margin-top: 30px;
            font-size: 11px;
            color: #777;
            border-top: 1px solid #eee;
            padding-top: 10px;
        }

        @media print {
            body { background: #fff; padding: 0; }
            .container { box-shadow: none; padding: 0; max-width: 100%; }
            .no-print { display: none !important; }
            input { border: none !important; background: transparent !important; }
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <div class="invoice-title">فاتورة</div>
        <div class="company-info" style="text-align: left;">
            <h2>Fashion World</h2>
            <p>سوريا</p>
            <p>دمشق، الحريقة</p>
        </div>
    </div>

    <div class="meta-section">
        <div class="form-group">
            <label>فاتورة لـ (اسم الزبون والعنوان):</label>
            <input type="text" id="clientName" placeholder="أدخل اسم الزبون وعنوانه...">
        </div>
        <div class="form-group">
            <label>رقم الهاتف:</label>
            <input type="text" id="clientPhone" placeholder="أدخل رقم الهاتف...">
        </div>
        
        <div style="display: flex; justify-content: space-between; margin-top: 10px; font-size: 12px;">
            <div><strong>رقم الفاتورة:</strong> <span id="invNumber">256</span></div>
            <div><strong>التاريخ:</strong> <span id="invDate"></span></div>
        </div>

        <div class="form-group no-print" style="margin-top: 12px;">
            <label style="font-size: 12px;">سعر صرف الليرة السورية (مقابل $):</label>
            <input type="number" id="exchangeRate" value="15000" style="text-align: center;" oninput="calculateTotals()">
        </div>
    </div>

    <table id="itemsTable">
        <thead>
            <tr>
                <th style="width: 40%;">الوصف (الموديل)</th>
                <th style="width: 18%;">السعر ($)</th>
                <th style="width: 15%;">الكمية</th>
                <th style="width: 18%;">المجموع</th>
                <th class="no-print" style="width: 9%;">حذف</th>
            </tr>
        </thead>
        <tbody id="tableBody">
            <tr>
                <td><input type="text" placeholder="اسم الموديل..."></td>
                <td><input type="number" step="0.01" value="0" oninput="calculateTotals()"></td>
                <td><input type="number" value="1" oninput="calculateTotals()"></td>
                <td class="row-total">$0.00</td>
                <td class="no-print"><button class="btn btn-danger" onclick="deleteRow(this)">×</button></td>
            </tr>
        </tbody>
    </table>

    <div class="no-print" style="margin-bottom: 15px;">
        <button class="btn" onclick="addRow()" style="background-color: #27ae60;">+ إضافة موديل جديد</button>
    </div>

    <div class="totals-section">
        <div class="total-row">
            <span>الإجمالي بالدولار:</span>
            <span id="totalUSD" style="font-weight: bold;">$0.00</span>
        </div>
        <div class="total-row final">
            <span>المبلغ المستحق ($):</span>
            <span id="finalUSD">$0.00</span>
        </div>
        <div class="total-row final" style="color: #27ae60; background: #f9fdf9; margin-top: 5px; padding: 8px;">
            <span>المبلغ بالليرة السورية:</span>
            <span id="finalSYP">0 ل.س</span>
        </div>
    </div>

    <div class="actions no-print">
        <button class="btn" onclick="window.print()" style="background-color: #2980b9; padding: 12px; font-size: 15px;">طباعة أو حفظ كـ PDF 🖨️</button>
    </div>

    <div class="footer-note">
        أنشئ بواسطة نظام Fashion World المخصص
    </div>
</div>

<script>
    const today = new Date();
    const dateString = String(today.getDate()).padStart(2, '0') + '/' + String(today.getMonth() + 1).padStart(2, '0') + '/' + today.getFullYear();
    document.getElementById('invDate').innerText = dateString;

    function addRow() {
        const tbody = document.getElementById('tableBody');
        const newRow = document.createElement('tr');
        newRow.innerHTML = `
            <td><input type="text" placeholder="اسم الموديل..."></td>
            <td><input type="number" step="0.01" value="0" oninput="calculateTotals()"></td>
            <td><input type="number" value="1" oninput="calculateTotals()"></td>
            <td class="row-total">$0.00</td>
            <td class="no-print"><button class="btn btn-danger" onclick="deleteRow(this)">×</button></td>
        `;
        tbody.appendChild(newRow);
    }

    function deleteRow(button) {
        const row = button.closest('tr');
        if(document.querySelectorAll('#tableBody tr').length > 1) {
            row.remove();
            calculateTotals();
        } else {
            alert('يجب أن تحتوي الفاتورة على بند واحد على الأقل.');
        }
    }

    function calculateTotals() {
        const rows = document.querySelectorAll('#tableBody tr');
        let grandTotalUSD = 0;
        const exchangeRate = parseFloat(document.getElementById('exchangeRate').value) || 0;

        rows.forEach(row => {
            const price = parseFloat(row.querySelectorAll('input')[1].value) || 0;
            const qty = parseFloat(row.querySelectorAll('input')[2].value) || 0;
            const rowTotal = price * qty;
            
            row.querySelector('.row-total').innerText = '$' + rowTotal.toFixed(2);
            grandTotalUSD += rowTotal;
        });

        document.getElementById('totalUSD').innerText = '$' + grandTotalUSD.toFixed(2);
        document.getElementById('finalUSD').innerText = '$' + grandTotalUSD.toFixed(2);

        const totalSYP = grandTotalUSD * exchangeRate;
        document.getElementById('finalSYP').innerText = totalSYP.toLocaleString('en-US', {maximumFractionDigits: 0}) + ' ل.س';
    }
</script>

</body>
</html>
