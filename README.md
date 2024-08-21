# triplan-investment-calculator
<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>מחשבון השקעה TRIPLAN</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://unpkg.com/@popperjs/core@2"></script>
    <script src="https://unpkg.com/tippy.js@6"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.17.0/xlsx.full.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css">
    <link rel="stylesheet" href="https://unpkg.com/tippy.js@6/themes/light.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f0f0f0;
        }
        .container {
            max-width: 1035px;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: #333;
            font-size: 32px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        h2 {
            text-align: center;
            color: #333;
            font-size: 24px;
            margin-top: 30px;
            margin-bottom: 15px;
            cursor: pointer;
        }
        .input-container {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 15px;
            margin-bottom: 20px;
            max-width: 920px;
            margin-left: auto;
            margin-right: auto;
        }
        .input-group {
            border: 1px solid #ddd;
            padding: 10px;
            border-radius: 5px;
            display: flex;
            flex-direction: column;
            background-color: #f9f9f9;
        }
        label {
            display: block;
            margin-bottom: 5px;
            color: #666;
            font-weight: bold;
        }
        input, select {
            width: 65%;
            padding: 5px;
            border: 1px solid #ddd;
            border-radius: 3px;
            margin-bottom: 6px;
        }
        .real-data-container input, .real-data-container select {
            width: 33.8%;
            font-size: 0.9em;
        }
        #loanPercentage {
            width: calc(100% - 12px);
        }
        button {
            display: block;
            width: 80%;
            padding: 12px;
            background: linear-gradient(45deg, #FFD700, #FFA500);
            color: black;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 24px;
            font-weight: bold;
            margin: 20px auto;
            text-shadow: 1px 1px 2px rgba(255,255,255,0.5);
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            transition: all 0.3s ease;
        }
        button:hover {
            background: linear-gradient(45deg, #FFA500, #FFD700);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.3);
        }
        .real-data-container {
            margin-top: 20px;
            width: 100%;
            max-width: 920px;
            margin-left: auto;
            margin-right: auto;
        }
        .data-section {
            margin-bottom: 15px;
            border: 1px solid #ddd;
            padding: 15px;
            border-radius: 5px;
            background-color: #f9f9f9;
        }
        .data-section h3 {
            margin-top: 0;
            color: #333;
            font-size: 18px;
            margin-bottom: 10px;
        }
        .product-row, .deposit-row {
            display: flex;
            align-items: center;
            margin-bottom: 5px;
            justify-content: space-between;
        }
        .product-row > *, .deposit-row > * {
            margin-right: 5px;
        }
        .add-product-btn, .remove-product-btn, .add-deposit-btn, .remove-deposit-btn {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 5px 10px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 14px;
            margin: 2px 2px;
            cursor: pointer;
            border-radius: 4px;
            width: 24px;
            height: 24px;
            padding: 0;
            font-size: 16px;
        }
        .remove-product-btn, .remove-deposit-btn {
            background-color: #f44336;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            font-size: 14px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 1px;
            text-align: center;
            height: 20px;
            line-height: 20px;
        }
        th {
            background-color: #f2f2f2;
            font-weight: bold;
        }
        tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        tr:nth-child(odd) {
            background-color: #ffffff;
        }
        .summary {
            font-weight: bold;
            background-color: #e6e6e6;
        }
        #chart-container {
            margin-top: 20px;
        }
        .bold-cell {
            font-weight: bold;
        }
        .group-header {
            background-color: #f2f2f2;
            font-weight: bold;
            text-align: center;
        }
        .table-section {
            border-right: 1px solid #ccc;
        }
        .table-section:first-child {
            border-right: none;
        }
        .results-summary {
            background-color: #f9f9f9;
            border: 1px solid #ddd;
            padding: 10px;
            border-radius: 5px;
            margin-top: 20px;
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 10px;
        }
        .results-summary h4 {
            margin-top: 0;
            margin-bottom: 10px;
            font-size: 18px;
            color: #333;
        }
        .results-summary p {
            margin: 5px 0;
        }
        .results-summary strong {
            font-weight: bold;
            color: #444;
        }
        #loanPercentageContainer {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        #loanPercentageContainer label {
            flex: 1;
            text-align: right;
        }
        #loanPercentage {
            flex: 1;
            width: auto;
        }
        #loanAmount {
            flex: 1;
            text-align: left;
            font-size: 1em;
            color: #333;
            font-weight: bold;
        }
        .export-buttons {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
        }
        .export-buttons button {
            width: 45%;
        }
        .year-cell {
            position: relative;
            cursor: pointer;
        }
        .details-button {
            position: absolute;
            top: 2px;
            right: 2px;
            font-size: 10px;
            background: none;
            border: none;
            padding: 0;
            margin: 0;
            width: auto;
            height: auto;
        }
        .monthly-details {
            display: none;
        }
        .monthly-details th, .monthly-details td {
            font-size: 12px;
            height: 18px;
            line-height: 18px;
            padding: 1px;
        }
        .copyright {
            text-align: center;
            color: #999;
            font-size: 12px;
            margin-top: 20px;
        }
        .comparison-container {
            margin-top: 20px;
            border: 1px solid #ddd;
            border-radius: 5px;
            overflow: hidden;
            max-width: 920px;
            margin-left: auto;
            margin-right: auto;
        }
        .comparison-row {
            display: flex;
            justify-content: space-between;
            padding: 5px 10px;
        }
        .comparison-group {
            flex: 1;
            margin: 0 5px;
            display: flex;
            align-items: center;
        }
        .redemption-row {
            display: flex;
            align-items: center;
            margin-bottom: 5px;
            justify-content: center;
        }
        .redemption-fields {
            display: flex;
            width: 90%;
        }
        .redemption-fields .comparison-group {
            flex: 1;
            margin: 0 2px;
        }
        .redemption-fields .comparison-group:nth-child(2) {
            flex: 1;
        }
        .add-redemption-btn, .remove-redemption-btn {
            width: 24px;
            height: 24px;
            padding: 0;
            background-color: #f0f0f0;
            color: #333;
            border: 1px solid #ddd;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0 2px;
        }
        .add-redemption-btn:hover, .remove-redemption-btn:hover {
            background-color: #e0e0e0;
        }
        #displayTable {
            width: 50%;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>מחשבון השקעה TRIPLAN</h1>
        
        <h2 id="realDataToggle" onclick="toggleRealData()">נתוני אמת ►</h2>
        
        <div id="realDataContainer" class="real-data-container" style="display: none;">
            <div class="data-section">
                <h3>מוצרים פיננסיים ופנסיוניים</h3>
                <div id="productsContainer">
                    <div class="product-row">
                        <select class="product-type">
                            <option value="">בחר מוצר</option>
                            <option value="keren_hishtalmut_sakhir">קרן השתלמות שכיר</option>
                            <option value="keren_hishtalmut_atzmai">קרן השתלמות עצמאי</option>
                            <option value="kupat_gemel">קופת גמל</option>
                            <option value="kupat_gemel_lehashkaa">קופת גמל להשקעה</option>
                            <option value="keren_pensia">קרן פנסיה</option>
                            <option value="bituach_menahalim">ביטוח מנהלים</option>
                            <option value="ira">IRA</option>
                            <option value="polisa_hisachon">פוליסת חיסכון</option>
                            <option value="current_account">עו"ש</option>
                            <option value="fixed_deposit">פק"מ</option>
                            <option value="money_market_fund">קרן כספית</option>
                            <option value="advised_portfolio">תיק מיועץ</option>
                            <option value="managed_portfolio">תיק מנוהל</option>
                            <option value="other">אחר</option>
                        </select>
                        <input type="number" class="savings-amount" placeholder="גובה החיסכון">
                        <select class="liquidity">
                            <option value="yes">נזיל</option>
                            <option value="no">לא נזיל</option>
                        </select>
                        <input type="number" class="liquidity-year" placeholder="שנים לנזילות" style="display:none;">
                        <button class="add-product-btn" onclick="addProduct()">+</button>
                        <button class="remove-product-btn" onclick="removeProduct(this)" style="display:none;">-</button>
                    </div>
                </div>
            </div>
            
            <div class="data-section">
                <h3>נתוני תזרים</h3>
                <div class="product-row">
                    <input type="text" id="totalIncome" placeholder="הכנסה כוללת">
                    <input type="text" id="totalExpense" placeholder="הוצאה כוללת">
                    <input type="text" id="availableCashFlow" placeholder="תזרים פנוי" readonly>
                    <input type="text" id="investmentAllocation" placeholder="אחוז הקצאה להשקעה">
                    <input type="text" id="investmentAmount" placeholder="סכום להשקעה" readonly>
                </div>
            </div>
            
            <div class="data-section">
                <h3>הפקדות לחיסכון חודשי</h3>
                <div id="monthlyDepositsContainer">
                    <div class="deposit-row">
                        <select class="deposit-product">
                            <option value="">בחר מכשיר</option>
                        </select>
                        <input type="number" class="deposit-amount" placeholder="סכום">
                        <input type="text" class="deposit-purpose" placeholder="מטרה">
                        <button class="add-deposit-btn" onclick="addDeposit()">+</button>
                        <button class="remove-deposit-btn" onclick="removeDeposit(this)" style="display:none;">-</button>
                    </div>
                </div>
            </div>
            
            <div class="data-section">
                <h3>נתוני משפחה</h3>
                <div class="product-row">
                    <input type="number" id="familyMembers" placeholder="בני משפחה">
                    <input type="number" id="childrenOver25" placeholder="ילדים מעל 25">
                    <input type="number" id="childrenUnder25" placeholder="ילדים מתחת ל-25">
                    <input type="text" id="address" placeholder="כתובת מגורים">
                </div>
            </div>
        </div>

        <div class="input-container">
            <div class="input-group">
                <label for="initialInvestment">סכום השקעה ראשוני:</label>
                <input type="text" id="initialInvestment" value="100,000 ₪" onchange="formatNumber(this)">
                
                <label for="monthlyInvestment">סכום השקעה חודשי:</label>
                <input type="text" id="monthlyInvestment" value="500 ₪" onchange="formatNumber(this)">
                
                <label for="investmentYears">שנות השקעה:</label>
                <input type="number" id="investmentYears" value="15">
                
                <label for="returnRate">תשואה שנתית (%):</label>
                <input type="text" id="returnRate" value="8%" onchange="formatPercentage(this)">
            </div>
            
            <div class="input-group">
                <div id="loanPercentageContainer">
                    <label for="loanPercentage">אחוז הלוואה:</label>
                    <select id="loanPercentage" onchange="updateLoanAmount()">
                        <option value="0">0%</option>
                        <option value="10">10%</option>
                        <option value="20">20%</option>
                        <option value="30">30%</option>
                        <option value="40">40%</option>
                        <option value="50">50%</option>
                        <option value="60">60%</option>
                        <option value="70" selected>70%</option>
                        <option value="80">80%</option>
                    </select>
                    <span id="loanAmount"></span>
                </div>
                
                <label for="loanYears">שנות הלוואה:</label>
                <input type="number" id="loanYears" value="7">
                
                <label for="loanType">סוג ההלוואה:</label>
                <select id="loanType">
                    <option value="balloon" selected>בלון 🎈</option>
                    <option value="spitzer">שפיצר</option>
                </select>
                
                <label for="loanInterest">ריבית על ההלוואה (%):</label>
                <input type="text" id="loanInterest" value="2%" onchange="formatPercentage(this)">
            </div>
            
            <div class="input-group">
                <label for="investmentType">סוג ההשקעה:</label>
                <select id="investmentType">
                    <option value="real_estate" selected>נדל"ן</option>
                    <option value="stock_market">שוק ההון</option>
                    <option value="alternative">אלטרנטיבי</option>
                </select>
                
                <label for="loanReturnRate">תשואה שנתית ממינוף:</label>
                <input type="text" id="loanReturnRate" value="5%" onchange="formatPercentage(this)">
                
                <label for="compoundLoanReturn">חישוב ריבית דריבית:</label>
                <select id="compoundLoanReturn">
                    <option value="true" selected>כן</option>
                    <option value="false">לא</option>
                </select>

                <label for="inflationRate">שיעור אינפלציה שנתי (%):</label>
                <input type="text" id="inflationRate" value="0%" onchange="formatPercentage(this)">
            </div>
        </div>
        
        <div class="comparison-container">
            <div class="comparison-row">
                <div class="comparison-group">
                    <label>
                        <input type="checkbox" id="showComparison" onchange="toggleComparisonVisibility()">
                        תמהיל להשוואה:
                    </label>
                </div>
                <div class="comparison-group">
                    <label for="displayTable">סוג טבלה:</label>
                    <select id="displayTable" onchange="updateComparisonVisibility()">
                        <option value="leveraged" selected>עם מינוף</option>
                        <option value="unleveraged">ללא מינוף</option>
                    </select>
                </div>
            </div>
            <div id="redemptionsContainer">
                <!-- Redemption rows will be added here -->
            </div>
        </div>
        
        <button onclick="calculateInvestment()">חשב</button>
        
        <div id="results"></div>
        <div id="chart-container">
            <canvas id="myChart"></canvas>
        </div>
        <div id="summary"></div>
        
        <div class="export-buttons">
            <button id="exportPDF">
                <i class="fas fa-file-pdf"></i> ייצא ל-PDF
            </button>
            <button id="exportExcel">
                <i class="fas fa-file-excel"></i> ייצא לאקסל
            </button>
        </div>
        
        <p class="copyright">כל הזכויות שמורות לTriplan</p>
    </div>

    <script>
    let myChart;
    let monthlyData = [];
    let redemptionCount = 0;
    let isRealDataMode = false;
    let selectedProducts = new Set();

    // Variables for tazrim fields
    let totalIncomeValue = 0;
    let totalExpenseValue = 0;
    let availableCashFlowValue = 0;
    let investmentAllocationValue = 0;
    let investmentAmountValue = 0;

    function toggleRealData() {
        const realDataContainer = document.getElementById('realDataContainer');
        const realDataToggle = document.getElementById('realDataToggle');
        isRealDataMode = !isRealDataMode;
        
        if (isRealDataMode) {
            realDataContainer.style.display = 'block';
            realDataToggle.textContent = 'נתוני אמת ▼';
            document.getElementById('initialInvestment').disabled = true;
            document.getElementById('monthlyInvestment').disabled = true;
        } else {
            realDataContainer.style.display = 'none';
            realDataToggle.textContent = 'נתוני אמת ►';
            document.getElementById('initialInvestment').disabled = false;
            document.getElementById('monthlyInvestment').disabled = false;
        }
    }

    function addProduct() {
        const container = document.getElementById('productsContainer');
        const newRow = container.firstElementChild.cloneNode(true);
        newRow.querySelectorAll('input').forEach(input => input.value = '');
        newRow.querySelector('select').selectedIndex = 0;
        newRow.querySelector('.add-product-btn').style.display = 'none';
        newRow.querySelector('.remove-product-btn').style.display = 'inline-block';
        container.appendChild(newRow);
        updateSelectedProducts();
    }

    function removeProduct(button) {
        const row = button.closest('.product-row');
        const container = row.parentElement;
        if (container.children.length > 1) {
            row.remove();
        }
        updateSelectedProducts();
    }

    function updateSelectedProducts() {
        selectedProducts.clear();
        document.querySelectorAll('#productsContainer .product-type').forEach(select => {
            if (select.value) {
                selectedProducts.add(select.value);
            }
        });
        updateDepositProductOptions();
    }

    function updateDepositProductOptions() {
        const depositSelects = document.querySelectorAll('#monthlyDepositsContainer .deposit-product');
        depositSelects.forEach(select => {
            const currentValue = select.value;
            select.innerHTML = '<option value="">בחר מכשיר</option>';
            selectedProducts.forEach(product => {
                const option = document.createElement('option');
                option.value = product;
                option.textContent = getProductName(product);
                select.appendChild(option);
            });
            select.value = currentValue;
        });
    }

    function getProductName(value) {
        const productMap = {
            "keren_hishtalmut_sakhir": "קרן השתלמות שכיר",
            "keren_hishtalmut_atzmai": "קרן השתלמות עצמאי",
            "kupat_gemel": "קופת גמל",
            "kupat_gemel_lehashkaa": "קופת גמל להשקעה",
            "keren_pensia": "קרן פנסיה",
            "bituach_menahalim": "ביטוח מנהלים",
            "ira": "IRA",
            "polisa_hisachon": "פוליסת חיסכון",
            "current_account": "עו\"ש",
            "fixed_deposit": "פק\"מ",
            "money_market_fund": "קרן כספית",
            "advised_portfolio": "תיק מיועץ",
            "managed_portfolio": "תיק מנוהל",
            "other": "אחר"
        };
        return productMap[value] || value;
    }

    function addDeposit() {
        const container = document.getElementById('monthlyDepositsContainer');
        const newRow = container.firstElementChild.cloneNode(true);
        newRow.querySelectorAll('input, select').forEach(el => el.value = '');
        newRow.querySelector('.add-deposit-btn').style.display = 'none';
        newRow.querySelector('.remove-deposit-btn').style.display = 'inline-block';
        container.appendChild(newRow);
        updateDepositProductOptions();
    }

    function removeDeposit(button) {
        const row = button.closest('.deposit-row');
        const container = row.parentElement;
        if (container.children.length > 1) {
            row.remove();
        }
    }

    function calculateTotalInvestment() {
        let totalInvestment = 0;
        document.querySelectorAll('#productsContainer .savings-amount').forEach(input => {
            const product = input.closest('.product-row').querySelector('.product-type').value;
            if (product !== 'keren_pensia' && product !== 'kupat_gemel' && product !== 'bituach_menahalim') {
                totalInvestment += parseFloat(input.value) || 0;
            }
        });
        document.getElementById('initialInvestment').value = totalInvestment.toLocaleString('he-IL') + ' ₪';
    }

    function updateTazrimFields() {
        document.getElementById('totalIncome').value = totalIncomeValue.toLocaleString('he-IL') + ' ₪ הכנסה';
        document.getElementById('totalExpense').value = totalExpenseValue.toLocaleString('he-IL') + ' ₪ הוצאה';
        document.getElementById('availableCashFlow').value = availableCashFlowValue.toLocaleString('he-IL') + ' ₪ תזרים';
        document.getElementById('investmentAllocation').value = investmentAllocationValue + '%';
        document.getElementById('investmentAmount').value = investmentAmountValue.toLocaleString('he-IL') + ' ₪';
    }

    function updateAvailableCashFlow() {
        availableCashFlowValue = totalIncomeValue - totalExpenseValue;
        updateInvestmentAmount();
        updateTazrimFields();
    }

    function updateInvestmentAmount() {
        investmentAmountValue = Math.round(availableCashFlowValue * (investmentAllocationValue / 100));
        document.getElementById('monthlyInvestment').value = investmentAmountValue.toLocaleString('he-IL') + ' ₪';
    }

    function formatNumber(input) {
        let value = input.value.replace(/[^\d.-]/g, '');
        value = parseFloat(value) || 0;
        input.value = value.toLocaleString('he-IL') + ' ₪';
    }

    function formatPercentage(input) {
        let value = input.value.replace(/[^\d.-]/g, '');
        value = parseFloat(value) || 0;
        input.value = value + '%';
    }

    function updateLoanAmount() {
        const initialInvestment = parseFloat(document.getElementById('initialInvestment').value.replace(/[^\d.-]/g, '')) || 0;
        const loanPercentage = parseFloat(document.getElementById('loanPercentage').value) || 0;
        const loanAmount = initialInvestment * (loanPercentage / 100);
        document.getElementById('loanAmount').textContent = `${Math.round(loanAmount).toLocaleString('he-IL')} ₪`;
    }

    function toggleComparisonVisibility() {
        const showComparison = document.getElementById('showComparison').checked;
        updateComparisonVisibility();
        if (showComparison && document.querySelectorAll('.redemption-row').length === 0) {
            addRedemption();
        }
    }

    function updateComparisonVisibility() {
        const showComparison = document.getElementById('showComparison').checked;
        const redemptionContainer = document.getElementById('redemptionsContainer');
        redemptionContainer.style.display = showComparison ? 'block' : 'none';
    }

    function addRedemption() {
        redemptionCount++;
        const newRedemptionRow = document.createElement('div');
        newRedemptionRow.className = 'redemption-row';
        newRedemptionRow.innerHTML = `
            <div class ="redemption-fields">
                <div class="comparison-group">
                    <label for="redemptionYear-${redemptionCount}">שנת פדיון:</label>
                    <input type="number" id="redemptionYear-${redemptionCount}" min="1" value="10">
                </div>
                <div class="comparison-group">
                    <label for="redemptionAmount-${redemptionCount}">סכום פדיון:</label>
                    <input type="text" id="redemptionAmount-${redemptionCount}" value="70,000 ₪" onchange="formatNumber(this)">
                </div>
                <div class="comparison-group">
                    <label for="redemptionMethod-${redemptionCount}">שיטת פדיון:</label>
                    <select id="redemptionMethod-${redemptionCount}">
                        <option value="FIFO">FIFO</option>
                        <option value="LIFO" selected>LIFO</option>
                        <option value="balanced">מאוזן</option>
                    </select>
                </div>
            </div>
            <button class="add-redemption-btn" onclick="addRedemption()">+</button>
            <button class="remove-redemption-btn" onclick="removeRedemption(this)">-</button>
        `;
        document.getElementById('redemptionsContainer').appendChild(newRedemptionRow);
    }

    function removeRedemption(button) {
        const redemptionRow = button.closest('.redemption-row');
        redemptionRow.remove();
        redemptionCount--;
        if (redemptionCount === 0) {
            document.getElementById('showComparison').checked = false;
            updateComparisonVisibility();
        }
    }

    function calculateInvestment() {
        if (isRealDataMode) {
            calculateTotalInvestment();
            updateAvailableCashFlow();
        }

        const initialInvestment = parseFloat(document.getElementById('initialInvestment').value.replace(/[^\d.-]/g, '')) || 0;
        const monthlyInvestment = parseFloat(document.getElementById('monthlyInvestment').value.replace(/[^\d.-]/g, '')) || 0;
        const investmentYears = parseInt(document.getElementById('investmentYears').value) || 1;
        const annualReturnRate = parseFloat(document.getElementById('returnRate').value.replace(/[^\d.-]/g, '')) / 100 || 0;
        const loanPercentage = parseFloat(document.getElementById('loanPercentage').value) || 0;
        const loanYears = parseInt(document.getElementById('loanYears').value) || 1;
        const loanType = document.getElementById('loanType').value;
        const annualLoanInterest = parseFloat(document.getElementById('loanInterest').value.replace(/[^\d.-]/g, '')) / 100 || 0;
        const annualLoanReturnRate = parseFloat(document.getElementById('loanReturnRate').value.replace(/[^\d.-]/g, '')) / 100 || 0;
        const compoundLoanReturn = document.getElementById('compoundLoanReturn').value === 'true';
        const inflationRate = parseFloat(document.getElementById('inflationRate').value.replace(/[^\d.-]/g, '')) / 100 || 0;

        const showComparison = document.getElementById('showComparison').checked;
        const displayTable = document.getElementById('displayTable').value;
        const isLeveraged = displayTable === 'leveraged';

        const redemptions = [];
        if (showComparison) {
            const redemptionRows = document.querySelectorAll('.redemption-row');
            redemptionRows.forEach((row, index) => {
                redemptions.push({
                    year: parseInt(row.querySelector(`#redemptionYear-${index + 1}`).value) || 0,
                    amount: parseFloat(row.querySelector(`#redemptionAmount-${index + 1}`).value.replace(/[^\d.-]/g, '')) || 0,
                    method: row.querySelector(`#redemptionMethod-${index + 1}`).value
                });
            });
        }

        // Sort redemptions by year
        redemptions.sort((a, b) => a.year - b.year);

        const loanAmount = isLeveraged ? initialInvestment * (loanPercentage / 100) : 0;

        let yearlyData = [];
        let originalInvestment = initialInvestment;
        let originalInvestmentValue = initialInvestment;
        let leveragedInvestment = loanAmount;
        let leveragedInvestmentValue = loanAmount;
        let remainingLoan = loanAmount;
        let totalLoanCost = 0;
        let isLoanPaid = false;
        let wasLoanPaidBefore = false;
        let cumulativeOriginalReturn = 0;
        let cumulativeLeveragedReturn = 0;

        monthlyData = [];

        const monthlyReturnRate = Math.pow(1 + annualReturnRate, 1/12) - 1;
        const monthlyLoanReturnRate = Math.pow(1 + annualLoanReturnRate, 1/12) - 1;

        for (let year = 1; year <= investmentYears; year++) {
            let yearlyOriginalReturn = 0;
            let yearlyLeveragedReturn = 0;
            let yearlyPrincipalPayment = 0;
            let yearlyInterestPayment = 0;
            let yearlyRedemptionAmount = 0;
            let yearlyRedemptionTax = 0;
            let yearlyRedemptionFromOriginal = 0;
            let yearlyRedemptionFromReturn = 0;

            for (let month = 1; month <= 12; month++) {
                let monthlyLoanCost = 0;
                let monthlyPrincipalPayment = 0;
                let monthlyInterestPayment = 0;

                // Redemption calculation
                const currentRedemptions = redemptions.filter(r => r.year === year && !r.performed);
                if (showComparison && currentRedemptions.length > 0 && month === 1) {
                    for (const redemption of currentRedemptions) {
                        let remainingRedemption = redemption.amount;

                        if (redemption.method === 'LIFO') {
                            // LIFO logic
                            if (remainingRedemption > 0 && cumulativeOriginalReturn > 0) {
                                const redemptionFromOriginalReturn = Math.min(remainingRedemption, cumulativeOriginalReturn);
                                remainingRedemption -= redemptionFromOriginalReturn;
                                cumulativeOriginalReturn -= redemptionFromOriginalReturn;
                                yearlyRedemptionFromReturn += redemptionFromOriginalReturn;
                            }
                            if (remainingRedemption > 0 && cumulativeLeveragedReturn > 0) {
                                const redemptionFromLeveragedReturn = Math.min(remainingRedemption, cumulativeLeveragedReturn);
                                remainingRedemption -= redemptionFromLeveragedReturn;
                                cumulativeLeveragedReturn -= redemptionFromLeveragedReturn;
                                yearlyRedemptionFromReturn += redemptionFromLeveragedReturn;
                            }
                            if (remainingRedemption > 0) {
                                const redemptionFromOriginal = Math.min(remainingRedemption, originalInvestment);
                                originalInvestment -= redemptionFromOriginal;
                                yearlyRedemptionFromOriginal += redemptionFromOriginal;
                                remainingRedemption -= redemptionFromOriginal;
                            }
                        } else if (redemption.method === 'FIFO') {
                            // FIFO logic
                            if (remainingRedemption > 0) {
                                const redemptionFromOriginal = Math.min(remainingRedemption, originalInvestment);
                                originalInvestment -= redemptionFromOriginal;
                                yearlyRedemptionFromOriginal += redemptionFromOriginal;
                                remainingRedemption -= redemptionFromOriginal;
                            }
                            if (remainingRedemption > 0 && cumulativeOriginalReturn > 0) {
                                const redemptionFromOriginalReturn = Math.min(remainingRedemption, cumulativeOriginalReturn);
                                remainingRedemption -= redemptionFromOriginalReturn;
                                cumulativeOriginalReturn -= redemptionFromOriginalReturn;
                                yearlyRedemptionFromReturn += redemptionFromOriginalReturn;
                            }
                            if (remainingRedemption > 0 && cumulativeLeveragedReturn > 0) {
                                const redemptionFromLeveragedReturn = Math.min(remainingRedemption, cumulativeLeveragedReturn);
                                remainingRedemption -= redemptionFromLeveragedReturn;
                                cumulativeLeveragedReturn -= redemptionFromLeveragedReturn;
                                yearlyRedemptionFromReturn += redemptionFromLeveragedReturn;
                            }
                        } else {
                            // Balanced logic
                            const totalInvestedCapital = originalInvestment + cumulativeOriginalReturn + cumulativeLeveragedReturn;
                            const redemptionRatio = redemption.amount / totalInvestedCapital;

                            const redemptionFromOriginal = Math.min(redemptionRatio * originalInvestment, originalInvestment);
                            originalInvestment -= redemptionFromOriginal;
                            yearlyRedemptionFromOriginal += redemptionFromOriginal;
                            remainingRedemption -= redemptionFromOriginal;

                            const redemptionFromOriginalReturn = Math.min(redemptionRatio * cumulativeOriginalReturn, cumulativeOriginalReturn);
                            cumulativeOriginalReturn -= redemptionFromOriginalReturn;
                            yearlyRedemptionFromReturn += redemptionFromOriginalReturn;
                            remainingRedemption -= redemptionFromOriginalReturn;

                            const redemptionFromLeveragedReturn = Math.min(redemptionRatio * cumulativeLeveragedReturn, cumulativeLeveragedReturn);
                            cumulativeLeveragedReturn -= redemptionFromLeveragedReturn;
                            yearlyRedemptionFromReturn += redemptionFromLeveragedReturn;
                            remainingRedemption -= redemptionFromLeveragedReturn;
                        }

                        const totalRedemption = redemption.amount - remainingRedemption;
                        const redemptionTax = yearlyRedemptionFromReturn * 0.25;

                        yearlyRedemptionTax += redemptionTax;
                        yearlyRedemptionAmount += totalRedemption;

                        originalInvestmentValue = Math.max(0, originalInvestmentValue - totalRedemption);
                        
                        redemption.performed = true;
                    }
                }

                // Loan calculations (only if leveraged)
                if (isLeveraged && year <= loanYears && !isLoanPaid && loanAmount > 0) {
                    if (loanType === 'balloon') {
                        monthlyInterestPayment = remainingLoan * (annualLoanInterest / 12);
                        monthlyLoanCost = monthlyInterestPayment;
                        if (year === loanYears && month === 12) {
                            monthlyPrincipalPayment = remainingLoan;
                            monthlyLoanCost += monthlyPrincipalPayment;
                            remainingLoan = 0;
                            isLoanPaid = true;
                        }
                    } else if (loanType === 'spitzer') {
                        const monthlyRate = annualLoanInterest / 12;
                        const totalPayments = loanYears * 12;
                        const monthlyPayment = (loanAmount * monthlyRate * Math.pow(1 + monthlyRate, totalPayments)) / (Math.pow(1 + monthlyRate, totalPayments) - 1);
                        monthlyInterestPayment = remainingLoan * monthlyRate;
                        monthlyPrincipalPayment = monthlyPayment - monthlyInterestPayment;
                        remainingLoan = Math.max(0, remainingLoan - monthlyPrincipalPayment);
                        monthlyLoanCost = monthlyPayment;
                        if (remainingLoan <= 0) {
                            isLoanPaid = true;
                            remainingLoan = 0;
                        }
                    }
                    yearlyPrincipalPayment += monthlyPrincipalPayment;
                    yearlyInterestPayment += monthlyInterestPayment;
                    totalLoanCost += monthlyInterestPayment;
                } else {
                    isLoanPaid = true;
                    remainingLoan = 0;
                }

                // Investment calculations
                const monthlyOriginalReturn = originalInvestmentValue * monthlyReturnRate;
                originalInvestmentValue += monthlyOriginalReturn + monthlyInvestment;
                yearlyOriginalReturn += monthlyOriginalReturn;
                cumulativeOriginalReturn += monthlyOriginalReturn;
                originalInvestment += monthlyInvestment;

                let monthlyLeveragedReturn = 0;
                if (isLeveraged && loanAmount > 0) {
                    if (compoundLoanReturn) {
                        monthlyLeveragedReturn = leveragedInvestmentValue * monthlyLoanReturnRate;
                        leveragedInvestmentValue += monthlyLeveragedReturn;
                    } else {
                        monthlyLeveragedReturn = leveragedInvestment * (annualLoanReturnRate / 12);
                        leveragedInvestmentValue = leveragedInvestment + monthlyLeveragedReturn;
                    }
                    yearlyLeveragedReturn += monthlyLeveragedReturn;
                    cumulativeLeveragedReturn += monthlyLeveragedReturn;
                } else {
                    leveragedInvestmentValue = 0;
                    leveragedInvestment = 0;
                }

                // Ensure no negative values
                originalInvestmentValue = Math.max(0, originalInvestmentValue);
                cumulativeOriginalReturn = Math.max(0, cumulativeOriginalReturn);
                leveragedInvestmentValue = Math.max(0, leveragedInvestmentValue);
                cumulativeLeveragedReturn = Math.max(0, cumulativeLeveragedReturn);

                // Inflation adjustment
                const monthlyInflationRate = inflationRate / 12;
                const inflationAdjustment = (originalInvestmentValue + leveragedInvestmentValue) * monthlyInflationRate;
                originalInvestmentValue -= inflationAdjustment / 2;
                if (isLeveraged && loanAmount > 0) {
                    leveragedInvestmentValue -= inflationAdjustment / 2;
                }

                const totalInvestedCapital = originalInvestmentValue + leveragedInvestmentValue;

                monthlyData.push({
                    year,
                    month,
                    originalInvestment,
                    originalInvestmentValue,
                    cumulativeOriginalReturn,
                    leveragedInvestment,
                    leveragedInvestmentValue,
                    cumulativeLeveragedReturn,
                    remainingLoan,
                    principalPayment: monthlyPrincipalPayment,
                    interestPayment: monthlyInterestPayment,
                    totalInvestedCapital,
                    redemptionAmount: month === 1 ? yearlyRedemptionAmount : 0,
                    redemptionTax: month === 1 ? yearlyRedemptionTax : 0,
                    redemptionFromOriginal: month === 1 ? yearlyRedemptionFromOriginal : 0,
                    redemptionFromReturn: month === 1 ? yearlyRedemptionFromReturn : 0
                });
            }

            yearlyData.push({
                year,
                originalInvestment,
                cumulativeOriginalReturn,
                leveragedInvestment,
                cumulativeLeveragedReturn,
                remainingLoan,
                principalPayment: yearlyPrincipalPayment,
                interestPayment: yearlyInterestPayment,
                isLoanPaid,
                wasLoanPaidBefore,
                totalInvestedCapital: originalInvestmentValue + leveragedInvestmentValue,
                redemptionAmount: yearlyRedemptionAmount,
                redemptionTax: yearlyRedemptionTax,
                redemptionFromOriginal: yearlyRedemptionFromOriginal,
                redemptionFromReturn: yearlyRedemptionFromReturn
            });

            wasLoanPaidBefore = isLoanPaid;

            if (originalInvestmentValue <= 0 && cumulativeOriginalReturn <= 0 && 
                leveragedInvestmentValue <= 0 && cumulativeLeveragedReturn <= 0 && monthlyInvestment <= 0) {
                break;
            }
        }

        displayResults(yearlyData, displayTable);
        updateChart(yearlyData, displayTable);
        displaySummary(yearlyData, displayTable);
    }

    function displayResults(data, displayTable) {
        let tableHTML = `<table>
            <tr>
                <th rowspan="2">שנה</th>
                <th colspan="2" class="group-header table-section">השקעה מקורית</th>`;

        if (displayTable === 'leveraged') {
            tableHTML += `
                <th colspan="2" class="group-header table-section">השקעה ממונפת</th>
                <th colspan="3" class="group-header table-section">התחייבויות</th>
                <th rowspan="2">מרווח</th>`;
        } else {
            tableHTML += `
                <th colspan="2" class="group-header table-section">פדיון</th>`;
        }

        tableHTML += `
                <th rowspan="2">סך ההון המושקע</th>
            </tr>
            <tr>
                <th>השקעה מקורית</th>
                <th>ריבית מצטברת</th>`;

        if (displayTable === 'leveraged') {
            tableHTML += `
                <th>השקעה ממונפת</th>
                <th>ריבית מצטברת</th>
                <th>יתרת הלוואה</th>
                <th>החזר קרן</th>
                <th>החזר ריבית</th>`;
        } else {
            tableHTML += `
                <th>פדיון</th>
                <th>תשלום המס</th>`;
        }

        tableHTML += `</tr>`;

        let totals = {
            originalInvestment: 0,
            cumulativeOriginalReturn: 0,
            leveragedInvestment: 0,
            cumulativeLeveragedReturn: 0,
            remainingLoan: 0,
            principalPayment: 0,
            interestPayment: 0,
            totalInvestedCapital: 0,
            redemptionAmount: 0,
            redemptionTax: 0
        };

        data.forEach((year, index) => {
            const margin = year.cumulativeLeveragedReturn - year.interestPayment;
            const leveragedInvestmentClass = year.isLoanPaid && !year.wasLoanPaidBefore ? 'bold-cell' : '';
            tableHTML += `<tr>
                <td class="year-cell" onclick="toggleMonthlyDetails(${year.year})">
                    ${year.year}
                    <span class="details-button">+</span>
                </td>
                <td class="table-section">${Math.round(year.originalInvestment).toLocaleString('he-IL')} ₪</td>
                <td class="table-section">${Math.round(year.cumulativeOriginalReturn).toLocaleString('he-IL')} ₪</td>`;

            if (displayTable === 'leveraged') {
                tableHTML += `
                    <td class="${leveragedInvestmentClass} table-section">${Math.round(year.leveragedInvestment).toLocaleString('he-IL')} ₪</td>
                    <td class="table-section">${Math.round(year.cumulativeLeveragedReturn).toLocaleString('he-IL')} ₪</td>
                    <td class="table-section">${Math.round(year.remainingLoan).toLocaleString('he-IL')} ₪</td>
                    <td class="table-section">${Math.round(year.principalPayment).toLocaleString('he-IL')} ₪</td>
                    <td class="table-section">${Math.round(year.interestPayment).toLocaleString('he-IL')} ₪</td>
                    <td>${Math.round(margin).toLocaleString('he-IL')} ₪</td>`;
            } else {
                tableHTML += `
                    <td class="table-section">${Math.round(year.redemptionAmount).toLocaleString('he-IL')} ₪</td>
                    <td class="table-section">${Math.round(year.redemptionTax).toLocaleString('he-IL')} ₪</td>`;
            }

            tableHTML += `
                <td>${Math.round(year.totalInvestedCapital).toLocaleString('he-IL')} ₪</td>
            </tr>
            <tr id="monthlyDetails${year.year}" class="monthly-details">
                <td colspan="${displayTable === 'leveraged' ? '10' : '6'}">
                    <table>
                        <tr>
                            <th>חודש</th>
                            <th>השקעה מקורית</th>
                            <th>ריבית מצטברת</th>
                            ${displayTable === 'leveraged' ? `
                                <th>השקעה ממונפת</th>
                                <th>ריבית מצטברת</th>
                                <th>יתרת הלוואה</th>
                                <th>החזר קרן</th>
                                <th>החזר ריבית</th>
                            ` : `
                                <th>פדיון מהשקעה מקורית</th>
                                <th>פדיון מתשואה</th>
                                <th>תשלום המס</th>
                            `}
                            <th>סה"כ הון מושקע</th>
                        </tr>
                        ${generateMonthlyDetailsHTML(year.year, displayTable)}
                    </table>
                </td>
            </tr>`;

            totals.originalInvestment = year.originalInvestment;
            totals.cumulativeOriginalReturn = year.cumulativeOriginalReturn;
            totals.leveragedInvestment = year.leveragedInvestment;
            totals.cumulativeLeveragedReturn = year.cumulativeLeveragedReturn;
            totals.remainingLoan = year.remainingLoan;
            totals.principalPayment += year.principalPayment;
            totals.interestPayment += year.interestPayment;
            totals.totalInvestedCapital = year.totalInvestedCapital;
            totals.redemptionAmount += year.redemptionAmount;
            totals.redemptionTax += year.redemptionTax;
        });

        const finalMargin = totals.cumulativeLeveragedReturn - totals.interestPayment;

        tableHTML += `<tr class="summary">
            <td>סה"כ</td>
            <td class="table-section">${Math.round(totals.originalInvestment).toLocaleString('he-IL')} ₪</td>
            <td class="table-section">${Math.round(totals.cumulativeOriginalReturn).toLocaleString('he-IL')} ₪</td>`;

        if (displayTable === 'leveraged') {
            tableHTML += `
                <td class="table-section">${Math.round(totals.leveragedInvestment).toLocaleString('he-IL')} ₪</td>
                <td class="table-section">${Math.round(totals.cumulativeLeveragedReturn).toLocaleString('he-IL')} ₪</td>
                <td class="table-section">${Math.round(totals.remainingLoan).toLocaleString('he-IL')} ₪</td>
                <td class="table-section">${Math.round(totals.principalPayment).toLocaleString('he-IL')} ₪</td>
                <td class="table-section">${Math.round(totals.interestPayment).toLocaleString('he-IL')} ₪</td>
                <td>${Math.round(finalMargin).toLocaleString('he-IL')} ₪</td>`;
        } else {
            tableHTML += `
                <td class="table-section">${Math.round(totals.redemptionAmount).toLocaleString('he-IL')} ₪</td>
                <td class="table-section">${Math.round(totals.redemptionTax).toLocaleString('he-IL')} ₪</td>`;
        }

        tableHTML += `
            <td>${Math.round(totals.totalInvestedCapital).toLocaleString('he-IL')} ₪</td>
        </tr>`;

        tableHTML += '</table>';
        document.getElementById('results').innerHTML = tableHTML;
    }

    function generateMonthlyDetailsHTML(year, displayTable) {
        const yearData = monthlyData.filter(data => data.year === year);
        let html = '';
        yearData.forEach(month => {
            html += `<tr>
                <td>${month.month}</td>
                <td>${Math.round(month.originalInvestment).toLocaleString('he-IL')} ₪</td>
                <td>${Math.round(month.cumulativeOriginalReturn).toLocaleString('he-IL')} ₪</td>`;
            
            if (displayTable === 'leveraged') {
                html += `
                    <td>${Math.round(month.leveragedInvestmentValue).toLocaleString('he-IL')} ₪</td>
                    <td>${Math.round(month.cumulativeLeveragedReturn).toLocaleString('he-IL')} ₪</td>
                    <td>${Math.round(month.remainingLoan).toLocaleString('he-IL')} ₪</td>
                    <td>${Math.round(month.principalPayment).toLocaleString('he-IL')} ₪</td>
                    <td>${Math.round(month.interestPayment).toLocaleString('he-IL')} ₪</td>`;
            } else {
                html += `
                    <td>${Math.round(month.redemptionFromOriginal).toLocaleString('he-IL')} ₪</td>
                    <td>${Math.round(month.redemptionFromReturn).toLocaleString('he-IL')} ₪</td>
                    <td>${Math.round(month.redemptionTax).toLocaleString('he-IL')} ₪</td>`;
            }

            html += `
                <td>${Math.round(month.totalInvestedCapital).toLocaleString('he-IL')} ₪</td>
            </tr>`;
        });
        return html;
    }

    function toggleMonthlyDetails(year) {
        const detailsRow = document.getElementById(`monthlyDetails${year}`);
        const button = document.querySelector(`.year-cell:nth-child(${year}) .details-button`);
        if (detailsRow.style.display === 'none' || detailsRow.style.display === '') {
            detailsRow.style.display = 'table-row';
            button.textContent = '-';
        } else {
            detailsRow.style.display = 'none';
            button.textContent = '+';
        }
    }

    function updateChart(data, displayTable) {
        const ctx = document.getElementById('myChart').getContext('2d');
        
        if (myChart) {
            myChart.destroy();
        }

        // Determine the last year with non-zero total invested capital
        const lastActiveYear = data.reduce((last, d, index) => d.totalInvestedCapital > 0 ? index + 1 : last, 0);

        const datasets = [
            {
                label: 'השקעה מקורית',
                data: data.slice(0, lastActiveYear).map(d => d.originalInvestment),
                backgroundColor: 'rgba(75, 192, 192, 0.8)',
            },
            {
                label: 'ריבית מצטברת (מקורי)',
                data: data.slice(0, lastActiveYear).map(d => d.cumulativeOriginalReturn),
                backgroundColor: 'rgba(153, 102, 255, 0.8)',
            }
        ];

        if (displayTable === 'leveraged') {
            datasets.push(
                {
                    label: 'השקעה ממונפת',
                    data: data.slice(0, lastActiveYear).map(d => d.leveragedInvestment),
                    backgroundColor: 'rgba(255, 159, 64, 0.8)',
                },
                {
                    label: 'ריבית מצטברת (ממונף)',
                    data: data.slice(0, lastActiveYear).map(d => d.cumulativeLeveragedReturn),
                    backgroundColor: 'rgba(255, 205, 86, 0.8)',
                },
                {
                    label: 'החזר הלוואה',
                    data: data.slice(0, lastActiveYear).map(d => -(d.principalPayment + d.interestPayment)),
                    backgroundColor: 'rgba(255, 99, 132, 0.8)',
                }
            );
        } else {
            datasets.push(
                {
                    label: 'פדיון מהשקעה מקורית',
                    data: data.slice(0, lastActiveYear).map(d => -d.redemptionFromOriginal),
                    backgroundColor: 'rgba(54, 162, 235, 0.8)',
                },
                {
                    label: 'פדיון מתשואה',
                    data: data.slice(0, lastActiveYear).map(d => -(d.redemptionFromReturn - d.redemptionTax)),
                    backgroundColor: 'rgba(255, 99, 132, 0.8)',
                },
                {
                    label: 'מס על פדיון',
                    data: data.slice(0, lastActiveYear).map(d => -d.redemptionTax),
                    backgroundColor: 'rgba(255, 206, 86, 0.8)',
                }
            );
        }

        myChart = new Chart(ctx, {
            type: 'bar',
            data: {
                labels: data.slice(0, lastActiveYear).map(d => `שנה ${d.year}`),
                datasets: datasets
            },
            options: {
                responsive: true,
                scales: {
                    x: { stacked: true },
                    y: {
                        stacked: true,
                        ticks: {
                            callback: value => new Intl.NumberFormat('he-IL', { style: 'currency', currency: 'ILS' }).format(value)
                        }
                    }
                },
                plugins: {
                    legend: {
                        position: 'top',
                        labels: {
                            font: {
                                size: 14
                            }
                        }
                    },
                    title: { 
                        display: true, 
                        text: 'גרף התפתחות השקעות',
                        font: {
                            size: 18,
                            weight: 'bold'
                        }
                    },
                    tooltip: {
                        rtl: true,
                        titleAlign: 'right',
                        bodyAlign: 'right',
                        callbacks: {
                            label: context => {
                                let label = context.dataset.label || '';
                                if (label) {
                                    label += ': ';
                                }
                                if (context.parsed.y !== null) {
                                    label += new Intl.NumberFormat('he-IL', { style: 'currency', currency: 'ILS' }).format(Math.abs(context.parsed.y));
                                }
                                return label;
                            }
                        }
                    }
                }
            }
        });
    }

    function displaySummary(data, displayTable) {
        const lastYear = data[data.length - 1];
        const totalInvestment = lastYear.originalInvestment;
        const totalOriginalReturn = lastYear.cumulativeOriginalReturn;
        const totalLeveragedReturn = lastYear.cumulativeLeveragedReturn;
        const totalReturn = totalOriginalReturn + totalLeveragedReturn;
        const roi = (totalReturn / totalInvestment) * 100;
        const averageAnnualReturn = (Math.pow((lastYear.totalInvestedCapital) / totalInvestment, 1 / data.length) - 1) * 100;

        // Calculate total interest payments on the loan
        const totalInterestPayments = data.reduce((sum, year) => sum + year.interestPayment, 0);

        // Calculate leverage profit
        const leverageProfit = totalLeveragedReturn - totalInterestPayments;

        const summaryHTML = `
        <div class="results-summary">
            <div>
                <h4>סיכום השקעה</h4>
                <p><strong data-tippy-content="סך כל הכסף שהושקע, כולל ההשקעה הראשונית וההפקדות החודשיות">סך הכל השקעה:</strong> ${Math.round(totalInvestment).toLocaleString('he-IL')} ₪</p>
                <p><strong data-tippy-content="יחס החזר ההשקעה, מחושב על ידי חלוקת הרווח הכולל בסכום ההשקעה הכוללת">ROI:</strong> ${Math.round(roi)}%</p>
            </div>
            <div>
                <h4>${displayTable === 'leveraged' ? 'פרטי הלוואה' : 'פרטי פדיון'}</h4>
                ${displayTable === 'leveraged' ? `
                    <p><strong data-tippy-content="הסכום הכולל ששולם עבור ריבית ההלוואה לאורך כל תקופת ההלוואה">סך הכל עלות ריבית:</strong> ${Math.round(totalInterestPayments).toLocaleString('he-IL')} ₪</p>
                    <p><strong data-tippy-content="היחס בין סכום ההלוואה לבין ההשקעה הראשונית">יחס מינוף:</strong> ${(lastYear.leveragedInvestment / totalInvestment).toFixed(2)}</p>
                    <p><strong data-tippy-content="הרווח מהמינוף, מחושב כריבית המצטברת מההשקעה הממונפת פחות עלויות הריבית על ההלוואה">רווח מינוף:</strong> ${Math.round(leverageProfit).toLocaleString('he-IL')} ₪</p>
                ` : `
                    <p><strong data-tippy-content="הסכום הכולל שנפדה">סך הכל פדיון:</strong> ${Math.round(data.reduce((sum, year) => sum + year.redemptionAmount, 0)).toLocaleString('he-IL')} ₪</p>
                    <p><strong data-tippy-content="סך המס ששולם על הפדיון">סך הכל מס על פדיון:</strong> ${Math.round(data.reduce((sum, year) => sum + year.redemptionTax, 0)).toLocaleString('he-IL')} ₪</p>
                    <p><strong data-tippy-content="הסכום הכולל שהתקבל בפועל לאחר ניכוי מס">סך הכל כסף ליד:</strong> ${Math.round(data.reduce((sum, year) => sum + year.redemptionAmount - year.redemptionTax, 0)).toLocaleString('he-IL')} ₪</p>
                `}
            </div>
            <div>
                <h4>ביצועי השקעה</h4>
                <p><strong data-tippy-content="התשואה השנתית הממוצעת על ההון העצמי, כולל רווחי ההשקעה השנייה">תשואה שנתית ממוצעת:</strong> ${averageAnnualReturn.toFixed(2)}%</p>
                <p><strong data-tippy-content="היחס בין הרווח הכולל לבין ההשקעה המקורית">יחס החזר להשקעה:</strong> ${(totalReturn / totalInvestment).toFixed(2)}</p>
                <p><strong data-tippy-content="הרווח הנקי הכולל, כולל כל ההון המושקע וכל התשואות">רווח נטו כולל:</strong> ${Math.round(totalReturn).toLocaleString('he-IL')} ₪</p>
                <p><strong data-tippy-content="התשואה על ההון העצמי, מחושבת כרווח כולל חלקי ההשקעה המקורית">תשואה על ההון העצמי:</strong> ${Math.round((totalReturn / totalInvestment) * 100)}%</p>
            </div>
        </div>
        `;

        document.getElementById('summary').innerHTML = summaryHTML;
        
        tippy('[data-tippy-content]', {
            theme: 'light',
            placement: 'top',
        });
    }

    // Initialize on page load
    window.onload = function() {
        updateLoanAmount();
        toggleComparisonVisibility();
        
        // Populate product options for monthly deposits
        updateSelectedProducts();

        document.querySelector('#productsContainer').addEventListener('change', updateSelectedProducts);
        
        document.getElementById('totalIncome').addEventListener('input', function(e) {
            totalIncomeValue = parseFloat(e.target.value.replace(/[^\d.-]/g, '')) || 0;
        });
        document.getElementById('totalIncome').addEventListener('blur', function() {
            updateAvailableCashFlow();
        });

        document.getElementById('totalExpense').addEventListener('input', function(e) {
            totalExpenseValue = parseFloat(e.target.value.replace(/[^\d.-]/g, '')) || 0;
        });
        document.getElementById('totalExpense').addEventListener('blur', function() {
            updateAvailableCashFlow();
        });

        document.getElementById('investmentAllocation').addEventListener('input', function(e) {
            investmentAllocationValue = parseFloat(e.target.value.replace(/[^\d.-]/g, '')) || 0;
            if (investmentAllocationValue > 100) investmentAllocationValue = 100;
        });
        document.getElementById('investmentAllocation').addEventListener('blur', function() {
            updateInvestmentAmount();
            updateTazrimFields();
        });

        // Add event listeners for formatting
        document.querySelectorAll('input[type="text"]').forEach(input => {
            input.addEventListener('blur', function() {
                if (this.id === 'investmentAllocation') {
                    formatPercentage(this);
                } else {
                    formatNumber(this);
                }
            });
        });
    };

    // Export to PDF function
    document.getElementById('exportPDF').addEventListener('click', function() {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF('p', 'mm', 'a4');
        
        const content = document.querySelector('.container');
        html2canvas(content, {
            scale: 2,
            useCORS: true,
            logging: true,
            letterRendering: 1,
            allowTaint: false,
        }).then(canvas => {
            const imgData = canvas.toDataURL('image/jpeg', 1.0);
            const imgProps = doc.getImageProperties(imgData);
            const pdfWidth = doc.internal.pageSize.getWidth();
            const pdfHeight = (imgProps.height * pdfWidth) / imgProps.width;
            
            const pageHeight = doc.internal.pageSize.getHeight();
            const pageCount = Math.ceil(pdfHeight / pageHeight);
            
            for (let i = 0; i < pageCount; i++) {
                if (i > 0) {
                    doc.addPage();
                }
                
                const srcY = i * pageHeight * (imgProps.width / pdfWidth);
                const srcHeight = Math.min(pageHeight * (imgProps.width / pdfWidth), imgProps.height - srcY);
                
                doc.addImage(imgData, 'JPEG', 0, 0, pdfWidth, pdfHeight, null, 'FAST', 0, srcY);
                
                doc.setFontSize(10);
                doc.text(`עמוד ${i + 1} מתוך ${pageCount}`, pdfWidth / 2, pageHeight - 10, { align: 'center' });
            }
            
            doc.setFontSize(8);
            doc.setTextColor(150);
            doc.text('כל הזכויות שמורות לTriplan', pdfWidth / 2, pageHeight - 5, { align: 'center' });
            
            doc.save('TRIPLAN_investment_report.pdf');
        });
    });

    // Export to Excel function
    document.getElementById('exportExcel').addEventListener('click', function() {
        const wb = XLSX.utils.book_new();
        wb.Props = {
            Title: "TRIPLAN Investment Report",
            Subject: "Investment Calculation",
            Author: "TRIPLAN",
            CreatedDate: new Date()
        };
        
        // Monthly data
        const excelData = monthlyData.map(d => ({
            'חודש': `${d.year}-${d.month.toString().padStart(2, '0')}`,
            'השקעה מקורית': d.originalInvestmentValue,
            'ריבית מצטברת (מקורי)': d.cumulativeOriginalReturn,
            'השקעה ממונפת': d.leveragedInvestmentValue,
            'ריבית מצטברת (ממונף)': d.cumulativeLeveragedReturn,
            'יתרת הלוואה': d.remainingLoan,
            'החזר קרן': d.principalPayment,
            'החזר ריבית': d.interestPayment,
            'סה"כ הון מושקע': d.totalInvestedCapital,
            'פדיון מהשקעה מקורית': d.redemptionFromOriginal,
            'פדיון מתשואה': d.redemptionFromReturn,
            'תשלום המס': d.redemptionTax,
            'כסף ליד': d.redemptionAmount - d.redemptionTax
        }));

        const ws = XLSX.utils.json_to_sheet(excelData);

        // Style the header
        const range = XLSX.utils.decode_range(ws['!ref']);
        for (let C = range.s.c; C <= range.e.c; ++C) {
            const address = XLSX.utils.encode_col(C) + "1";
            if (!ws[address]) continue;
            ws[address].s = { font: { bold: true }, fill: { fgColor: { rgb: "EFEFEF" } } };
        }

        XLSX.utils.book_append_sheet(wb, ws, "נתונים חודשיים");

        // Generate Excel file
        XLSX.writeFile(wb, 'TRIPLAN_investment_report.xlsx');
    });

    </script>
</body>
</html>
