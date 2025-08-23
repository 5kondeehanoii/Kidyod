ระบบคิดยอดพี่กวาง 30%
<html lang="th">
<head>
    <meta charset="UTF-8">
    <title>ระบบคีย์เลขหวย</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
    <style>
        body { padding: 20px; }
        .btn-yellow { background-color: #ffc107; color: #000; font-weight: bold; }
        .btn-teal { background-color: #20c997; color: #fff; font-weight: bold; }
        .btn-red { background-color: #dc3545; color: white; font-weight: bold; }
        .btn-blue { background-color: #0d6efd; color: white; font-weight: bold; }
        .btn-purple { background-color: #6f42c1; color: white; font-weight: bold; }
        .btn-dark-green { background-color: #198754; color: white; font-weight: bold; }
        .winning-info-text { color: green; font-weight: bold; margin-left: 10px; }
        .winning-amount-display { color: darkgreen; font-weight: bold; font-size: 1.2em; }
        .correct-numbers-row .col-md-3 { flex: 0 0 auto; width: 25%; }
        @media (max-width: 767.98px) { .correct-numbers-row .col-md-3 { width: 50%; } }
        .input-amounts-group .col-auto { padding-right: 5px; }
        .input-amounts-group .col-auto:last-child { padding-right: 0; }
        .summary-layout { display: flex; justify-content: space-between; align-items: flex-start; margin-top: 20px; }
        .main-bill-summary { flex: 1; margin-right: 20px; }
        .bill-entry-old { margin-bottom: 5px; border-bottom: 1px dashed #ccc; padding-bottom: 5px; }
        .total-time-display { font-size: 0.9em; color: #555; display: block; margin-bottom: 5px; font-weight: normal; }
        .winning-number-detail { font-size: 0.9em; color: green; margin-left: 0; margin-top: 3px; }
        #inputNumbersDisplay { margin-top: 10px; margin-bottom: 10px; padding: 5px; border: 1px dashed #ddd; min-height: 30px; background-color: #f9f9f9; border-radius: 5px; }
        .displayed-number { display: inline-block; background-color: #e0e0e0; padding: 3px 8px; margin: 2px; border-radius: 3px; font-weight: bold; cursor: pointer; }
        .displayed-number:hover { background-color: #ccc; }
        #timeBasedSummaryTableContainer { margin-top: 20px; width: 100%; overflow-x: auto; }
        #timeBasedSummaryTable { width: 100%; border-collapse: collapse; margin-top: 10px; }
        #timeBasedSummaryTable th, #timeBasedSummaryTable td { border: 1px solid #dee2e6; padding: 8px; text-align: right; white-space: nowrap; }
        #timeBasedSummaryTable th { background-color: #f8f9fa; text-align: center; }
        #timeBasedSummaryTable td:first-child { text-align: center; font-weight: bold; }
        #timeBasedSummaryTable tfoot tr { font-weight: bold; background-color: #e9ecef; }
        #timeBasedSummaryTable td.winning-numbers-cell { text-align: left; white-space: normal; font-size: 0.85em; }
        .delete-summary-btn { background: none; border: none; color: #dc3545; cursor: pointer; font-size: 1.2em; padding: 0; margin-left: 5px; vertical-align: middle; }
        .delete-summary-btn:hover { color: #bd2130; }
        #currentBillDisplay { margin-top: 20px; border: 1px solid #ddd; padding: 15px; background-color: #f8f9fa; border-radius: 8px; box-shadow: 0 0 10px rgba(0,0,0,0.05); }
        .ticket-group { border: 1px solid #cce5ff; background-color: #e6f7ff; padding: 10px; margin-bottom: 15px; border-radius: 5px; }
        .ticket-header { font-weight: bold; font-size: 1.1em; margin-bottom: 5px; display: flex; justify-content: space-between; align-items: center; }
        .ticket-item { display: flex; justify-content: space-between; padding: 2px 0; border-bottom: 1px dotted #a8dafc; }
        .ticket-item:last-child { border-bottom: none; }
        .ticket-number-type { font-weight: bold; }
        .ticket-amount { color: #007bff; }
        .ticket-delete-btn { background: none; border: none; color: #dc3545; cursor: pointer; font-size: 1em; margin-left: 10px; padding: 0; }
        .ticket-delete-btn:hover { color: #bd2130; }
        .hide-for-single-discount { display: none !important; }
        .th-33-percent, .td-33-percent { color: purple; font-weight: bold; }
        .overall-summary-table th, .overall-summary-table td { text-align: right; padding: 8px; border: 1px solid #dee2e6; }
        .overall-summary-table th:first-child, .overall-summary-table td:first-child { text-align: left; }
        .overall-summary-table tfoot th, .overall-summary-table tfoot td { font-weight: bold; background-color: #e9ecef; }
        .overall-summary-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 15px; border: 1px solid #ddd; padding: 10px; border-radius: 5px; }
        .overall-summary-grid strong { justify-self: start; }
        .overall-summary-grid .value { justify-self: end; }
        .overall-summary-footer { margin-top: 20px; padding-top: 10px; border-top: 1px solid #ccc; }
        .summary-total-footer { font-size: 1.2em; font-weight: bold; }
        .summary-total-footer.negative { color: red; }
        .summary-total-footer.positive { color: green; }
        .summary-total-status { font-size: 1.1em; font-weight: bold; margin-top: 5px; }
        .summary-total-status.negative { color: red; }
        .summary-total-status.positive { color: green; }
        .specific-summary-table { width: 100%; border-collapse: collapse; font-size: 1.2em; }
        .specific-summary-table td { padding: 8px 0; border-bottom: 1px solid #eee; }
        .specific-summary-table td:first-child { font-weight: bold; }
        .specific-summary-table td:last-child { text-align: right; font-weight: bold; }
        .specific-summary-table tr:last-child td { border-bottom: none; }
        .specific-summary-table .negative-value { color: #dc3545; }
        .specific-summary-table .total-row td { border-top: 2px solid #000; }
        .summary-status-row td { padding-top: 15px; }
        .summary-status-row .status-text { color: green; font-weight: bold; }
        .summary-status-row .status-text.negative { color: #dc3545; }
        #overallSummaryModal .modal-body { padding: 1.5rem; }
        #overallSummaryModal .modal-header { background-color: #f8f9fa; border-bottom: 1px solid #dee2e6; }
        #modalSummaryTable { width: 100%; border-collapse: collapse; margin-top: 10px; }
        #modalSummaryTable th, #modalSummaryTable td { border: 1px solid #dee2e6; padding: 8px; text-align: right; white-space: nowrap; }
        #modalSummaryTable th { background-color: #f8f9fa; text-align: center; }
        #modalSummaryTable td:first-child { text-align: center; font-weight: bold; }
        #modalSummaryTable tfoot tr { font-weight: bold; background-color: #e9ecef; }
        #modalSummaryTable td.winning-numbers-cell { text-align: left; white-space: normal; font-size: 0.85em; }
    </style>
</head>
<body>
    <h4 class="mb-3 p-4">ระบบคีย์เลขหวย</h4>
    <div class="container-fluid">
        <div class="row">
            <div class="col-12">
                <div class="row mb-3 gx-3 gy-2 correct-numbers-row">
                    <div class="col-md-3">
                        <label for="correctThreeDigitsDirectInput">✅ เลขถูก 3 ตัวตรง :</label>
                        <input type="text" id="correctThreeDigitsDirectInput" class="form-control" placeholder="เช่น 123" oninput="updateCorrectNumbers()">
                    </div>
                    <div class="col-md-3">
                        <label for="correctThreeDigitsTodsInput">✅ เลขถูก 3 ตัวโต๊ด :</label>
                        <input type="text" id="correctThreeDigitsTodsInput" class="form-control" placeholder="เช่น 123" oninput="updateCorrectNumbers()">
                    </div>
                    <div class="col-md-3">
                        <label for="correctTwoDigitsUpperInput">✅ เลขถูก 2 ตัวบน :</label>
                        <input type="text" id="correctTwoDigitsUpperInput" class="form-control" placeholder="เช่น 12" oninput="updateCorrectNumbers()">
                    </div>
                    <div class="col-md-3">
                        <label for="correctTwoDigitsLowerInput">✅ เลขถูก 2 ตัวล่าง :</label>
                        <input type="text" id="correctTwoDigitsLowerInput" class="form-control" placeholder="เช่น 34" oninput="updateCorrectNumbers()">
                    </div>
                </div>
                <div id="numberList" class="mb-3"></div>
                <div class="d-flex justify-content-start gap-2 mt-3">
                    <button class="btn btn-yellow" onclick="generate6Reverse()">6 กลับ</button>
                    <button class="btn btn-yellow" onclick="reverseInputNumbers()">กลับเลข</button>
                </div>
                <div class="row g-2 align-items-center mb-3 input-amounts-group">
                    <div class="col-auto">
                        <input type="text" id="billTime" class="form-control" placeholder="เช่น 15:30">
                    </div>
                    <div class="col-auto">
                        <input type="text" id="inputNumber" class="form-control" placeholder="ใส่เลข เช่น 13 31 23" oninput="displayInputNumbers()" onkeydown="handleInputNumberKeydown(event)" onblur="handleInputNumberBlur()" autocomplete="off">
                    </div>
                    <div class="col-auto">
                        <input type="number" id="upperAmount" class="form-control" placeholder="บน">
                    </div>
                    <div class="col-auto">
                        <input type="number" id="lowerAmount" class="form-control" placeholder="ล่าง">
                    </div>
                    <div class="col-auto">
                        <input type="number" id="todsAmount" class="form-control" placeholder="โต๊ด">
                    </div>
                    <div class="col-auto">
                        <button class="btn btn-teal" onclick="addNumber()">+ เพิ่มบิล</button>
                    </div>
                </div>
                <div id="inputNumbersDisplay" class="mb-3"></div>
                <div class="row">
                    <div class="col-md-6">
                        <div class="mt-4 mb-3">
                            <label for="phoyInput" class="form-label">เพิ่มช่อง ใส่โพย เพื่อแปลงเลข</label>
                            <textarea class="form-control" id="phoyInput" rows="5" placeholder="วางโพยที่นี่..."></textarea>
                        </div>
                        <div class="d-flex justify-content-start gap-2 mt-3">
                            <button class="btn btn-primary" onclick="convertPhoy()">แปลงตัวเลข</button>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="mt-4 mb-3">
                            <label for="textInput" class="form-label">วางข้อความสำหรับคำนวณ</label>
                            <textarea class="form-control" id="textInput" rows="5" placeholder="วางข้อความที่นี่..."></textarea>
                        </div>
                        <div class="d-flex justify-content-start gap-2 mt-3">
                            <button class="btn btn-primary" onclick="processCopiedText()">คำนวณจากข้อความ</button>
                        </div>
                    </div>
                </div>
                <div class="d-flex justify-content-start gap-2 mt-3">
                    <button class="btn btn-red" onclick="clearBill()">🗑 ล้างบิล</button>
                    <button class="btn btn-blue" onclick="removeDuplicates()">🚫 ลบเลขซ้ำ</button>
                    <button class="btn btn-purple" onclick="clearAllData()">🧹 ล้างเลขทั้งหมด</button>
                    <div class="ms-auto d-flex gap-2">
                        <button class="btn btn-dark-green" onclick="saveCurrentBillSummary()">💾 บันทึกยอดรวม</button>
                        <button class="btn btn-primary" onclick="showCombinedSummaryModal()">📊 สรุปยอดทั้งหมด</button>
                    </div>
                </div>
                <div id="currentBillDisplay" class="mt-4">
                    <h5>รายการบิล</h5>
                </div>
                <div class="summary-layout">
                    <div class="main-bill-summary">
                        <h5 class="mt-2 mb-2"><span id="totalTime" class="total-time-display"></span></h5>
                        <h5 class="mb-2">💲 รวมยอดซื้อทั้งหมด (ก่อนหัก %): <span id="grandTotalBeforeDiscount">0</span> บาท</h5>
                        <h5 class="mb-2">🎉 ยอดซื้อรวม (หลังหักส่วนลด 30%): <span id="singleDiscountTotalAmount">0</span> บาท</h5>
                        <h5 class="mt-2 mb-2">💰 รวมยอดทั้งหมด: <span id="totalDisplay">0</span> บาท</h5>
                        <h5 class="mt-2 mb-2">🏆 ยอดถูกรางวัลทั้งหมด: <span id="totalWinningAmount" class="winning-amount-display">0</span> บาท</h5>
                        <div id="winningNumbersDisplay"></div>
                    </div>
                </div>
                <div id="timeBasedSummaryTableContainer">
                    <h5 class="mt-4 mb-2">สรุปยอดรวมตามเวลา</h5>
                    <table id="timeBasedSummaryTable" class="table table-bordered table-striped">
                        <thead>
                            <tr>
                                <th>เวลา</th>
                                <th>ยอดรวมก่อนหัก %</th>
                                <th>ยอดซื้อส่วนลด 30%</th>
                                <th class="text-center">ตัวเลขที่ถูก (จำนวนซื้อ)</th>
                                <th>ยอดถูกรางวัล</th>
                                <th>ลบ</th>
                            </tr>
                        </thead>
                        <tbody id="timeBasedSummaryTableBody"></tbody>
                        <tfoot id="timeBasedSummaryTableFoot"></tfoot>
                    </table>
                </div>
            </div>
        </div>
    </div>
    <div class="modal fade" id="overallSummaryModal" tabindex="-1" aria-labelledby="overallSummaryModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-xl">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="overallSummaryModalLabel">📊 สรุปยอดทั้งหมด</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div id="overallSummaryContent">
                        <table class="specific-summary-table mb-4">
                            <tbody>
                                <tr>
                                    <td>ยอดซื้อทั้งหมด</td>
                                    <td id="overallTotalPurchase">0</td>
                                </tr>
                                <tr>
                                    <td>ยอดซื้อ 30%</td>
                                    <td id="overallDiscountedPurchase">0</td>
                                </tr>
                                <tr>
                                    <td>ยอดถูก</td>
                                    <td id="overallWinningAmount">0</td>
                                </tr>
                                <tr class="total-row">
                                    <td>สรุปยอดหักยอดถูก</td>
                                    <td id="overallNetTotal">0</td>
                                </tr>
                                <tr class="summary-status-row">
                                    <td>ส่งเจ้า / โอนลูกค้า</td>
                                    <td id="overallStatusText" class="status-text"></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div id="modalSummaryTableContainer">
                        <h5 class="mt-4 mb-2">สรุปยอดรวมตามเวลา</h5>
                        <table id="modalTimeBasedSummaryTable" class="table table-bordered table-striped">
                            <thead>
                                <tr>
                                    <th>เวลา</th>
                                    <th>ยอดรวมก่อนหัก %</th>
                                    <th>ยอดซื้อส่วนลด 30%</th>
                                    <th class="text-center">ตัวเลขที่ถูก (จำนวนซื้อ)</th>
                                    <th>ยอดถูกรางวัล</th>
                                </tr>
                            </thead>
                            <tbody id="modalTimeBasedSummaryTableBody"></tbody>
                            <tfoot id="modalTimeBasedSummaryTableFoot"></tfoot>
                        </table>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">ปิด</button>
                    <button type="button" class="btn btn-primary" onclick="saveOverallSummaryAsImage()">💾 บันทึกรูปภาพ</button>
                </div>
            </div>
        </div>
    </div>
    <script>
        const billTimeInput = document.getElementById('billTime');
        const inputNumberInput = document.getElementById('inputNumber');
        const upperAmountInput = document.getElementById('upperAmount');
        const lowerAmountInput = document.getElementById('lowerAmount');
        const todsAmountInput = document.getElementById('todsAmount');
        const textInput = document.getElementById('textInput');
        const phoyInput = document.getElementById('phoyInput');
        const inputNumbersDisplay = document.getElementById('inputNumbersDisplay');
        const currentBillDisplay = document.getElementById('currentBillDisplay');
        const timeBasedSummaryTableBody = document.getElementById('timeBasedSummaryTableBody');
        const timeBasedSummaryTableFoot = document.getElementById('timeBasedSummaryTableFoot');
        const totalDisplay = document.getElementById('totalDisplay');
        const grandTotalBeforeDiscountDisplay = document.getElementById('grandTotalBeforeDiscount');
        const singleDiscountTotalAmountDisplay = document.getElementById('singleDiscountTotalAmount');
        const totalWinningAmountDisplay = document.getElementById('totalWinningAmount');
        const winningNumbersDisplay = document.getElementById('winningNumbersDisplay');
        const correctThreeDigitsDirectInput = document.getElementById('correctThreeDigitsDirectInput');
        const correctThreeDigitsTodsInput = document.getElementById('correctThreeDigitsTodsInput');
        const correctTwoDigitsUpperInput = document.getElementById('correctTwoDigitsUpperInput');
        const correctTwoDigitsLowerInput = document.getElementById('correctTwoDigitsLowerInput');
        let currentBillEntries = [];
        let allBills = [];

        function saveData() {
            localStorage.setItem('lottoCurrentBill', JSON.stringify(currentBillEntries));
            localStorage.setItem('lottoAllBills', JSON.stringify(allBills));
        }

        function loadData() {
            const savedCurrentBill = localStorage.getItem('lottoCurrentBill');
            if (savedCurrentBill) {
                currentBillEntries = JSON.parse(savedCurrentBill);
            }
            const savedAllBills = localStorage.getItem('lottoAllBills');
            if (savedAllBills) {
                allBills = JSON.parse(savedAllBills);
            }
        }

        function updateDisplay() {
            const billContainer = document.getElementById('currentBillDisplay');
            billContainer.innerHTML = '<h5>รายการบิล</h5>';
            let htmlContent = '';
            currentBillEntries.forEach((entry, index) => {
                const totalForEntry = (entry.upper || 0) + (entry.lower || 0) + (entry.tods || 0);
                let details = '';
                if (entry.upper > 0) details += ` บน: ${entry.upper}`;
                if (entry.lower > 0) details += ` ล่าง: ${entry.lower}`;
                if (entry.tods > 0) details += ` โต๊ด: ${entry.tods}`;
                htmlContent += `<div class="ticket-group"><div class="ticket-header"><span>${entry.number}</span></div><div class="ticket-item"><span class="ticket-number-type">${details}</span><span class="ticket-amount">${totalForEntry}</span></div></div>`;
            });
            billContainer.innerHTML += htmlContent;
            
            const totals = calculateTotal(currentBillEntries);
            grandTotalBeforeDiscountDisplay.textContent = totals.grandTotalBeforeDiscount;
            singleDiscountTotalAmountDisplay.textContent = totals.singleDiscountTotalAmount;
            totalDisplay.textContent = totals.total;
            checkWinningNumbers();
            updateWinningNumbersDisplay();
            saveData();
        }

        function renderBillDetails(entry) {
            let details = '';
            if (entry.upper > 0) {
                if (entry.number.length === 3) details += `ตรง ${entry.upper} `;
                else details += `บน ${entry.upper} `;
            }
            if (entry.tods > 0) details += `โต๊ด ${entry.tods} `;
            if (entry.lower > 0) details += `ล่าง ${entry.lower} `;
            return details.trim();
        }

        function calculateTotal(entries) {
            let total = 0;
            let grandTotalBeforeDiscount = 0;
            let singleDiscountTotalAmount = 0;
            entries.forEach(entry => {
                const upperTotal = entry.upper;
                const lowerTotal = entry.lower;
                const todsTotal = entry.tods;
                const totalForEntry = upperTotal + lowerTotal + todsTotal;
                grandTotalBeforeDiscount += totalForEntry;
                if (totalForEntry > 50) {
                    singleDiscountTotalAmount += 50 + (totalForEntry - 50) * 0.7;
                } else {
                    singleDiscountTotalAmount += totalForEntry;
                }
                total += totalForEntry;
            });
            return {
                total: total.toFixed(2),
                grandTotalBeforeDiscount: grandTotalBeforeDiscount.toFixed(2),
                singleDiscountTotalAmount: singleDiscountTotalAmount.toFixed(2)
            };
        }

        function addNumber() {
            const numbers = inputNumberInput.value.trim().split(/\s+/).filter(n => n.length > 0);
            const upperAmount = parseFloat(upperAmountInput.value) || 0;
            const lowerAmount = parseFloat(lowerAmountInput.value) || 0;
            const todsAmount = parseFloat(todsAmountInput.value) || 0;
            if (numbers.length === 0 || (upperAmount === 0 && lowerAmount === 0 && todsAmount === 0)) {
                alert('กรุณาใส่เลขและยอดเงินอย่างน้อยหนึ่งช่อง');
                return;
            }
            const billTime = billTimeInput.value || new Date().toLocaleTimeString('th-TH', { hour: '2-digit', minute: '2-digit' });
            numbers.forEach(num => {
                const newEntry = { time: billTime, number: num, upper: upperAmount, lower: lowerAmount, tods: todsAmount };
                currentBillEntries.push(newEntry);
            });
            inputNumberInput.value = '';
            upperAmountInput.value = '';
            lowerAmountInput.value = '';
            todsAmountInput.value = '';
            inputNumbersDisplay.innerHTML = '';
            updateDisplay();
        }

        function deleteNumber(number) {
            currentBillEntries = currentBillEntries.filter(entry => entry.number !== number);
            saveData();
            updateDisplay();
        }

        function clearBill() {
            if (confirm('ยืนยันการล้างบิลทั้งหมดในรอบนี้?')) {
                currentBillEntries = [];
                saveData();
                updateDisplay();
            }
        }

        function removeDuplicates() {
            const uniqueEntries = [];
            const numbersSeen = new Set();
            for (const entry of currentBillEntries) {
                if (!numbersSeen.has(entry.number)) {
                    uniqueEntries.push(entry);
                    numbersSeen.add(entry.number);
                }
            }
            currentBillEntries = uniqueEntries;
            saveData();
            updateDisplay();
        }

        function clearAllData() {
            if (confirm('ยืนยันการล้างข้อมูลทั้งหมด (รวมถึงประวัติบิล)?')) {
                currentBillEntries = [];
                allBills = [];
                saveData();
                updateDisplay();
                updateSummaryTable();
            }
        }

        function saveCurrentBillSummary() {
            const selectedTime = document.getElementById('billTime').value;
            if (!selectedTime) { 
                alert('กรุณาเลือกเวลาก่อนบันทึก');
                return;
            }
            const total = calculateTotal(currentBillEntries);
            const time = billTimeInput.value || new Date().toLocaleTimeString('th-TH', { hour: '2-digit', minute: '2-digit' });
            allBills.push({
                time: time,
                grandTotalBeforeDiscount: parseFloat(total.grandTotalBeforeDiscount),
                singleDiscountTotalAmount: parseFloat(total.singleDiscountTotalAmount),
                total: parseFloat(total.total),
                winningAmount: 0,
                winningDetails: [],
                correctNumbers: {
                    threeDigitsDirect: correctThreeDigitsDirectInput.value.trim(),
                    threeDigitsTods: correctThreeDigitsTodsInput.value.trim(),
                    twoDigitsUpper: correctTwoDigitsUpperInput.value.trim(),
                    twoDigitsLower: correctTwoDigitsLowerInput.value.trim()
                },
                entries: currentBillEntries
            });
            currentBillEntries = [];
            saveData();
            updateDisplay();
            updateSummaryTable();
            alert('บันทึกยอดรวมแล้ว');
            updateCorrectNumbers();
            billTimeInput.value = '';
            phoyInput.value = '';
        }

        function updateSummaryTable() {
            const summaryData = allBills;
            summaryData.sort((a, b) => {
                const timeA = a.time.split(':').map(Number);
                const timeB = b.time.split(':').map(Number);
                if (timeA[0] !== timeB[0]) return timeA[0] - timeB[0];
                return timeA[1] - timeB[1];
            });
            let tableBodyHtml = '';
            let grandTotalBeforeDiscount = 0;
            let singleDiscountTotalAmount = 0;
            let totalSum = 0;
            let totalWinningAmount = 0;
            allBills.forEach((bill, index) => {
                const grandTotalBeforeDiscountText = Math.round(bill.grandTotalBeforeDiscount);
                const singleDiscountTotalAmountText = Math.round(bill.singleDiscountTotalAmount);
                const winningAmountText = Math.round(bill.winningAmount);
                const winningDetailsHtml = renderWinningDetails(bill.winningDetails);
                const winningClass = bill.winningAmount > 0 ? 'text-success' : 'text-danger';

                tableBodyHtml += `<tr>
                    <td style="text-align: center;">${bill.time}</td>
                    <td style="text-align: center;">${grandTotalBeforeDiscountText}</td>
                    <td class="td-33-percent" style="text-align: center;">${singleDiscountTotalAmountText}</td>
                    <td class="winning-numbers-cell">${winningDetailsHtml}</td>
                    <td class="${winningClass}" style="text-align: center;">${winningAmountText}</td>
                    <td style="text-align: center;"><button class="delete-summary-btn" onclick="deleteSummary(${index})">🗑</button></td>
                </tr>`;
                grandTotalBeforeDiscount += bill.grandTotalBeforeDiscount;
                singleDiscountTotalAmount += bill.singleDiscountTotalAmount;
                totalSum += bill.total;
                totalWinningAmount += bill.winningAmount;
            });
            timeBasedSummaryTableBody.innerHTML = tableBodyHtml;
            const netTotal = totalWinningAmount - totalSum;
            const netTotalClass = netTotal > 0 ? 'text-success' : 'text-danger';
            const tableFootHtml = `<tr>
                <th>รวมทั้งหมด:</th>
                <th style="text-align: center;">${Math.round(grandTotalBeforeDiscount)}</th>
                <th class="th-33-percent" style="text-align: center;">${Math.round(singleDiscountTotalAmount)}</th>
                <th></th>
                <th class="${netTotalClass}" style="text-align: center;">${Math.round(totalWinningAmount)}</th>
                <th></th>
            </tr>`;
            timeBasedSummaryTableFoot.innerHTML = tableFootHtml;
        }

        function deleteSummary(index) {
            if (confirm('ยืนยันการลบรายการสรุปนี้?')) {
                allBills.splice(index, 1);
                saveData();
                updateSummaryTable();
            }
        }

        function checkWinningNumbers() {
            const correctThreeDigitsDirect = correctThreeDigitsDirectInput.value.trim();
            const correctThreeDigitsTods = correctThreeDigitsTodsInput.value.trim();
            const correctTwoDigitsUpper = correctTwoDigitsUpperInput.value.trim();
            const correctTwoDigitsLower = correctTwoDigitsLowerInput.value.trim();
            let totalWinningAmount = 0;
            let winningDetails = [];
            currentBillEntries.forEach(entry => {
                let entryWinningAmount = 0;
                let winningTypes = [];
                if (correctThreeDigitsDirect && entry.number === correctThreeDigitsDirect && entry.upper > 0) { entryWinningAmount += entry.upper * 600; winningTypes.push('ตรง'); }
                if ((correctThreeDigitsDirect && arePermutations(entry.number, correctThreeDigitsDirect)) && entry.tods > 0) { entryWinningAmount += entry.tods * 150; winningTypes.push('โต๊ด'); }
                if (correctThreeDigitsTods && arePermutations(entry.number, correctThreeDigitsTods) && entry.tods > 0) { entryWinningAmount += entry.tods * 150; winningTypes.push('โต๊ด'); }
                if (correctTwoDigitsUpper && entry.number === correctTwoDigitsUpper && entry.upper > 0) { entryWinningAmount += entry.upper * 90; winningTypes.push('บน'); }
                if (correctTwoDigitsLower && entry.number === correctTwoDigitsLower && entry.lower > 0) { entryWinningAmount += entry.lower * 90; winningTypes.push('ล่าง'); }
                if (entryWinningAmount > 0) {
                    totalWinningAmount += entryWinningAmount;
                    winningDetails.push({ number: entry.number, types: winningTypes, purchase: { upper: entry.upper, lower: entry.lower, tods: entry.tods } });
                }
            });
            allBills.forEach(bill => {
                bill.winningAmount = 0;
                bill.winningDetails = [];
                const correct = bill.correctNumbers;
                bill.entries.forEach(entry => {
                    let entryWinningAmount = 0;
                    let winningTypes = [];
                    if (correct.threeDigitsDirect && entry.number === correct.threeDigitsDirect && entry.upper > 0) { entryWinningAmount += entry.upper * 500; winningTypes.push('ตรง'); }
                    if ((correct.threeDigitsDirect && arePermutations(entry.number, correct.threeDigitsDirect)) && entry.tods > 0) { entryWinningAmount += entry.tods * 100; winningTypes.push('โต๊ด'); }
                    if (correct.threeDigitsTods && arePermutations(entry.number, correct.threeDigitsTods) && entry.tods > 0) { entryWinningAmount += entry.tods * 100; winningTypes.push('โต๊ด'); }
                    if (correct.twoDigitsUpper && entry.number === correct.twoDigitsUpper && entry.upper > 0) { entryWinningAmount += entry.upper * 70; winningTypes.push('บน'); }
                    if (correct.twoDigitsLower && entry.number === correct.twoDigitsLower && entry.lower > 0) { entryWinningAmount += entry.lower * 70; winningTypes.push('ล่าง'); }
                    if (entryWinningAmount > 0) {
                        bill.winningAmount += entryWinningAmount;
                        bill.winningDetails.push({ number: entry.number, types: winningTypes, purchase: { upper: entry.upper, lower: entry.lower, tods: entry.tods } });
                    }
                });
            });
            saveData();
            updateSummaryTable();
            totalWinningAmountDisplay.textContent = totalWinningAmount.toFixed(2);
        }

        function updateWinningNumbersDisplay() {
            const correctThreeDigitsDirect = correctThreeDigitsDirectInput.value.trim();
            const correctThreeDigitsTods = correctThreeDigitsTodsInput.value.trim();
            const correctTwoDigitsUpper = correctTwoDigitsUpperInput.value.trim();
            const correctTwoDigitsLower = correctTwoDigitsLowerInput.value.trim();
            let winningNumbersHtml = '<h5>ตัวเลขที่ถูก:</h5>';
            const allCorrectNumbers = [];
            if (correctThreeDigitsDirect) allCorrectNumbers.push(`${correctThreeDigitsDirect} (3 ตัวตรง)`);
            if (correctThreeDigitsTods) allCorrectNumbers.push(`${correctThreeDigitsTods} (3 ตัวโต๊ด)`);
            if (correctTwoDigitsUpper) allCorrectNumbers.push(`${correctTwoDigitsUpper} (2 ตัวบน)`);
            if (correctTwoDigitsLower) allCorrectNumbers.push(`${correctTwoDigitsLower} (2 ตัวล่าง)`);
            if (allCorrectNumbers.length > 0) {
                winningNumbersHtml += `<p class="winning-info-text">${allCorrectNumbers.join(', ')}</p>`;
            } else {
                winningNumbersHtml += `<p class="winning-info-text">ยังไม่มีเลขถูก</p>`;
            }
            winningNumbersDisplay.innerHTML = winningNumbersHtml;
        }

        function generate6Reverse() {
            const numbers = inputNumberInput.value.trim().split(' ').map(n => n.trim()).filter(n => n.length === 2 || n.length === 3);
            if (numbers.length === 0) {
                alert('กรุณาใส่ตัวเลขอย่างน้อยหนึ่งตัวเพื่อกลับเลข');
                return;
            }
            const allReversedNumbers = new Set();
            numbers.forEach(num => {
                if (num.length === 2) {
                    allReversedNumbers.add(num);
                    allReversedNumbers.add(num.split('').reverse().join(''));
                } else if (num.length === 3) {
                    const numArr = num.split('');
                    const permutations = new Set();
                    if (new Set(numArr).size === 3) {
                        permutations.add(numArr[0] + numArr[1] + numArr[2]);
                        permutations.add(numArr[0] + numArr[2] + numArr[1]);
                        permutations.add(numArr[1] + numArr[0] + numArr[2]);
                        permutations.add(numArr[1] + numArr[2] + numArr[0]);
                        permutations.add(numArr[2] + numArr[0] + numArr[1]);
                        permutations.add(numArr[2] + numArr[1] + numArr[0]);
                    } else if (new Set(numArr).size === 2) {
                        const unique = [...new Set(numArr)];
                        permutations.add(unique[0] + unique[0] + unique[1]);
                        permutations.add(unique[0] + unique[1] + unique[0]);
                        permutations.add(unique[1] + unique[0] + unique[0]);
                        permutations.add(unique[1] + unique[1] + unique[0]);
                        permutations.add(unique[1] + unique[0] + unique[1]);
                        permutations.add(unique[0] + unique[1] + unique[1]);
                    }
                    permutations.forEach(p => allReversedNumbers.add(p));
                }
            });
            inputNumberInput.value = [...allReversedNumbers].join(' ');
            displayInputNumbers();
        }

        function reverseInputNumbers() {
            const numbers = inputNumberInput.value.trim().split(' ').map(n => n.trim()).filter(n => n.length > 0);
            if (numbers.length === 0) {
                alert('กรุณาใส่ตัวเลขเพื่อกลับเลข');
                return;
            }
            const reversedNumbers = numbers.map(num => num.split('').reverse().join(''));
            inputNumberInput.value = reversedNumbers.join(' ');
            displayInputNumbers();
        }

        function handleInputNumberKeydown(event) {
            if (event.key === 'Enter') {
                event.preventDefault();
                addNumber();
            }
        }

        function handleInputNumberBlur() {
            const numbers = inputNumberInput.value.trim().split(' ').map(n => n.trim()).filter(n => n.length > 0);
            inputNumberInput.value = numbers.join(' ');
        }

        function displayInputNumbers() {
            const numbers = inputNumberInput.value.trim().split(' ').map(n => n.trim()).filter(n => n.length > 0);
            let displayHtml = '';
            numbers.forEach(num => { displayHtml += `<span class="displayed-number">${num}</span>`; });
            inputNumbersDisplay.innerHTML = displayHtml;
        }
        
        function convertPhoy() {
            const phoyInput = document.getElementById('phoyInput');
            const textInput = document.getElementById('textInput');
            const phoyText = phoyInput.value.trim();
            if (phoyText === '') {
                alert('กรุณาวางโพยที่ต้องการแปลง');
                return;
            }
            const lines = phoyText.split('\n').map(line => line.trim()).filter(line => line !== '');
            const groupedPhoy = new Map();
            let lastAmount = null;
            lines.forEach(line => {
                line = line.replace(/[xX×+]+/g, '*');
                let numbersPart = '';
                let amountsPart = '';
                let foundAmount = false;
                const colonMatch = line.match(/^(.*?)\s*:\s*(.*?)$/);
                if (colonMatch) {
                    numbersPart = colonMatch[1].trim();
                    const rawAmounts = colonMatch[2].trim();
                    let upperPrice = 0;
                    let lowerPrice = 0;
                    const upperMatch = rawAmounts.match(/บน\s*(\d+)/);
                    const lowerMatch = rawAmounts.match(/ล่าง\s*(\d+)/);
                    if (upperMatch) upperPrice = parseInt(upperMatch[1], 10);
                    if (lowerMatch) lowerPrice = parseInt(lowerMatch[1], 10);
                    if (upperPrice > 0 || lowerPrice > 0) amountsPart = `${upperPrice}*${lowerPrice}`;
                    else amountsPart = rawAmounts.replace(/[xX×+]+/g, '*').replace(/\s+/g, '');
                    foundAmount = true;
                } else {
                    const combinedMatch = line.match(/^(.*?)\s*=\s*(.*?)$/);
                    if (combinedMatch) {
                        numbersPart = combinedMatch[1].trim();
                        amountsPart = combinedMatch[2].trim().replace(/\s+/g, '');
                        foundAmount = true;
                    } else {
                        const slashAmountMatch = line.match(/^(.*?)\s*([/])\s*([\d*xX×+]+)$/);
                        if (slashAmountMatch) {
                            numbersPart = slashAmountMatch[1].trim();
                            amountsPart = slashAmountMatch[3].trim().replace(/\s+/g, '');
                            foundAmount = true;
                        } else {
                            const standaloneAmountMatch = line.match(/^(.*?)\s+([\d*xX×+]+)$/);
                            if (standaloneAmountMatch) {
                                numbersPart = standaloneAmountMatch[1].trim();
                                amountsPart = standaloneAmountMatch[2].trim();
                                foundAmount = true;
                            } else {
                                if (lastAmount) {
                                    numbersPart = line.trim();
                                    amountsPart = lastAmount;
                                    foundAmount = true;
                                }
                            }
                        }
                    }
                }
                if (foundAmount) lastAmount = amountsPart;
                if (numbersPart) {
                    const numbers = numbersPart.replace(/[-/,.`()]+/g, ' ').trim().split(/\s+/).filter(n => n.length > 0);
                    if (numbers.length > 0 && amountsPart) {
                        if (groupedPhoy.has(amountsPart)) groupedPhoy.set(amountsPart, [...groupedPhoy.get(amountsPart), ...numbers]);
                        else groupedPhoy.set(amountsPart, numbers);
                    }
                }
            });
            let finalOutput = '';
            groupedPhoy.forEach((numbers, amountsPart) => {
                if (finalOutput !== '') finalOutput += '\n';
                finalOutput += `${numbers.join(' ')} = ${amountsPart.replace(/\s+/g, '')}`;
            });
            textInput.value = finalOutput.trim();
        }
        
        function processCopiedText() {
            const phoyText = textInput.value.trim();
            if (phoyText === '') {
                alert('กรุณาวางข้อความที่ต้องการคำนวณ');
                return;
            }
            const billTime = billTimeInput.value || new Date().toLocaleTimeString('th-TH', { hour: '2-digit', minute: '2-digit' });
            const lines = phoyText.split('\n').map(line => line.trim()).filter(line => line !== '');
            let totalProcessed = 0;
            lines.forEach(line => {
                const parts = line.split('=');
                if (parts.length !== 2) return;
                const numbersPart = parts[0].trim();
                const amountsPart = parts[1].trim();
                let upperAmount = 0;
                let lowerAmount = 0;
                let todsAmount = 0;
                const amounts = amountsPart.split('*').map(a => parseInt(a));
                if (amounts.length === 1) {
                    if (numbersPart.split(/\s+/).filter(n => n.length === 3).length > 0) todsAmount = amounts[0];
                    else upperAmount = amounts[0];
                } else if (amounts.length === 2) {
                    if (numbersPart.split(/\s+/).filter(n => n.length === 3).length > 0) { upperAmount = amounts[0]; todsAmount = amounts[1]; }
                    else { upperAmount = amounts[0]; lowerAmount = amounts[1]; }
                } else if (amounts.length === 3) {
                    upperAmount = amounts[0]; lowerAmount = amounts[1]; todsAmount = amounts[2];
                }
                const numbers = numbersPart.split(/[\s,/-]+/g).filter(n => n.length > 0);
                numbers.forEach(num => {
                    const newEntry = { time: billTime, number: num, upper: upperAmount, lower: lowerAmount, tods: todsAmount };
                    currentBillEntries.push(newEntry);
                    totalProcessed++;
                });
            });
            saveData();
            updateDisplay();
            textInput.value = '';
        }

        function arePermutations(str1, str2) {
            if (str1.length !== str2.length) return false;
            const sortedStr1 = str1.split('').sort().join('');
            const sortedStr2 = str2.split('').sort().join('');
            return sortedStr1 === sortedStr2;
        }

        function updateCorrectNumbers() {
            checkWinningNumbers();
            updateWinningNumbersDisplay();
        }

        function renderWinningDetails(details) {
            if (!details || details.length === 0) return '';
            return details.map(d => {
                let purchaseDetails = [];
                if (d.types.includes('ตรง')) purchaseDetails.push(`ตรง ${d.purchase['upper']}`);
                if (d.types.includes('โต๊ด')) purchaseDetails.push(`โต๊ด ${d.purchase['tods']}`);
                if (d.types.includes('บน')) purchaseDetails.push(`บน ${d.purchase['upper']}`);
                if (d.types.includes('ล่าง')) purchaseDetails.push(`ล่าง ${d.purchase['lower']}`);
                return `ถูก ${d.number} (${purchaseDetails.join(' และ ')})`;
            }).join('<br>');
        }

        function updateModalSummaryTable() {
            const tableBody = document.getElementById('modalTimeBasedSummaryTableBody');
            const tableFoot = document.getElementById('modalTimeBasedSummaryTableFoot');
            let tableBodyHtml = '';
            let grandTotalBeforeDiscount = 0;
            let singleDiscountTotalAmount = 0;
            let totalWinningAmount = 0;
            
            allBills.forEach((bill) => {
                const grandTotalBeforeDiscountText = Math.round(bill.grandTotalBeforeDiscount);
                const singleDiscountTotalAmountText = Math.round(bill.singleDiscountTotalAmount);
                const winningAmountText = Math.round(bill.winningAmount);
                const winningDetailsHtml = renderWinningDetails(bill.winningDetails);
                const winningClass = bill.winningAmount > 0 ? 'text-success' : 'text-danger';

                tableBodyHtml += `<tr>
                    <td style="text-align: center;">${bill.time}</td>
                    <td style="text-align: center;">${grandTotalBeforeDiscountText}</td>
                    <td class="td-33-percent" style="text-align: center;">${singleDiscountTotalAmountText}</td>
                    <td class="winning-numbers-cell">${winningDetailsHtml}</td>
                    <td class="${winningClass}" style="text-align: center;">${winningAmountText}</td>
                </tr>`;
                grandTotalBeforeDiscount += bill.grandTotalBeforeDiscount;
                singleDiscountTotalAmount += bill.singleDiscountTotalAmount;
                totalWinningAmount += bill.winningAmount;
            });

            tableBody.innerHTML = tableBodyHtml;
            
            const tableFootHtml = `<tr>
                <th>รวมทั้งหมด:</th>
                <th style="text-align: center;">${Math.round(grandTotalBeforeDiscount)}</th>
                <th class="th-33-percent" style="text-align: center;">${Math.round(singleDiscountTotalAmount)}</th>
                <th></th>
                <th style="text-align: center;">${Math.round(totalWinningAmount)}</th>
            </tr>`;
            tableFoot.innerHTML = tableFootHtml;
        }

        function showCombinedSummaryModal() {
            const overallTotalPurchase = allBills.reduce((sum, bill) => sum + bill.total, 0);
            const overallDiscountedPurchase = allBills.reduce((sum, bill) => sum + bill.singleDiscountTotalAmount, 0);
            const overallWinningAmount = allBills.reduce((sum, bill) => sum + bill.winningAmount, 0);
            const overallNetTotal = overallTotalPurchase - overallWinningAmount;

            document.getElementById('overallTotalPurchase').textContent = Math.round(overallTotalPurchase);
            document.getElementById('overallDiscountedPurchase').textContent = Math.round(overallDiscountedPurchase);
            document.getElementById('overallWinningAmount').textContent = Math.round(overallWinningAmount);
            document.getElementById('overallNetTotal').textContent = Math.round(overallNetTotal);
            
            const statusTextElement = document.getElementById('overallStatusText');
            if (overallNetTotal > 0) {
                statusTextElement.textContent = `ส่งเจ้า: ${Math.round(overallNetTotal)} บาท`;
                statusTextElement.classList.remove('negative');
                statusTextElement.classList.add('positive');
            } else if (overallNetTotal < 0) {
                statusTextElement.textContent = `โอนลูกค้า: ${Math.round(Math.abs(overallNetTotal))} บาท`;
                statusTextElement.classList.remove('positive');
                statusTextElement.classList.add('negative');
            } else {
                statusTextElement.textContent = `ยอดรวมเท่ากับ 0 บาท`;
                statusTextElement.classList.remove('positive', 'negative');
            }
            
            updateModalSummaryTable();
            const modal = new bootstrap.Modal(document.getElementById('overallSummaryModal'));
            modal.show();
        }

        function saveOverallSummaryAsImage() {
            const modalBody = document.querySelector('#overallSummaryModal .modal-body');
            html2canvas(modalBody).then(canvas => {
                const link = document.createElement('a');
                link.href = canvas.toDataURL('image/png');
                link.download = 'summary_overall.png';
                link.click();
            });
        }
        
        loadData();
        updateDisplay();
        updateSummaryTable();
        updateCorrectNumbers();
    </script>
</body>
</html>
