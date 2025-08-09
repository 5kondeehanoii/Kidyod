<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <title>ระบบคีย์เลขหวย</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
    <style>
        body {
            padding: 20px;
        }
        .number-box {
            display: inline-block;
            background-color: #c0392b;
            color: white;
            padding: 6px 12px;
            margin: 4px;
            border-radius: 5px;
            font-weight: bold;
            font-size: 16px;
        }
        .btn-yellow { background-color: #ffc107; color: #000; font-weight: bold; }
        .btn-teal { background-color: #20c997; color: #fff; font-weight: bold; }
        .btn-red { background-color: #dc3545; color: white; font-weight: bold; }
        .btn-blue { background-color: #0d6efd; color: white; font-weight: bold; }
        .btn-purple { background-color: #6f42c1; color: white; font-weight: bold; }
        .btn-dark-green { background-color: #198754; color: white; font-weight: bold; }
        .winning-info-text {
            color: green;
            font-weight: bold;
            margin-left: 10px;
        }
        .winning-amount-display {
            color: darkgreen;
            font-weight: bold;
            font-size: 1.2em;
        }
        .correct-numbers-row .col-md-3 {
            flex: 0 0 auto;
            width: 25%;
        }
        @media (max-width: 767.98px) {
            .correct-numbers-row .col-md-3 {
                width: 50%;
            }
        }
        .input-amounts-group .col-auto {
            padding-right: 5px;
        }
        .input-amounts-group .col-auto:last-child {
            padding-right: 0;
        }
        .summary-layout {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-top: 20px;
        }
        .main-bill-summary {
            flex: 1;
            margin-right: 20px;
        }
        .bill-entry-old {
            margin-bottom: 5px;
            border-bottom: 1px dashed #ccc;
            padding-bottom: 5px;
        }
        .total-time-display {
            font-size: 0.9em;
            color: #555;
            display: block;
            margin-bottom: 5px;
            font-weight: normal;
        }
        .winning-number-detail {
            font-size: 0.9em;
            color: green;
            margin-left: 0;
            margin-top: 3px;
        }
        #inputNumbersDisplay {
            margin-top: 10px;
            margin-bottom: 10px;
            padding: 5px;
            border: 1px dashed #ddd;
            min-height: 30px;
            background-color: #f9f9f9;
            border-radius: 5px;
        }
        .displayed-number {
            display: inline-block;
            background-color: #e0e0e0;
            padding: 3px 8px;
            margin: 2px;
            border-radius: 3px;
            font-weight: bold;
            cursor: pointer;
        }
        .displayed-number:hover {
            background-color: #ccc;
        }
        #timeBasedSummaryTableContainer {
            margin-top: 20px;
            width: 100%;
            overflow-x: auto;
        }
        #timeBasedSummaryTable {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
        }
        #timeBasedSummaryTable th, #timeBasedSummaryTable td {
            border: 1px solid #dee2e6;
            padding: 8px;
            text-align: right;
            white-space: nowrap;
        }
        #timeBasedSummaryTable th {
            background-color: #f8f9fa;
            text-align: center;
        }
        #timeBasedSummaryTable td:first-child {
            text-align: center;
            font-weight: bold;
        }
        #timeBasedSummaryTable tfoot tr {
            font-weight: bold;
            background-color: #e9ecef;
        }
        #timeBasedSummaryTable td.winning-numbers-cell {
            text-align: left;
            white-space: normal;
            font-size: 0.85em;
        }
        .delete-summary-btn {
            background: none;
            border: none;
            color: #dc3545;
            cursor: pointer;
            font-size: 1.2em;
            padding: 0;
            margin-left: 5px;
            vertical-align: middle;
        }
        .delete-summary-btn:hover {
            color: #bd2130;
        }
        #currentBillDisplay {
            margin-top: 20px;
            border: 1px solid #ddd;
            padding: 15px;
            background-color: #f8f9fa;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.05);
        }
        .ticket-group {
            border: 1px solid #cce5ff;
            background-color: #e6f7ff;
            padding: 10px;
            margin-bottom: 15px;
            border-radius: 5px;
        }
        .ticket-header {
            font-weight: bold;
            font-size: 1.1em;
            margin-bottom: 5px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .ticket-item {
            display: flex;
            justify-content: space-between;
            padding: 2px 0;
            border-bottom: 1px dotted #a8dafc;
        }
        .ticket-item:last-child {
            border-bottom: none;
        }
        .ticket-number-type {
            font-weight: bold;
        }
        .ticket-amount {
            color: #007bff;
        }
        .ticket-delete-btn {
            background: none;
            border: none;
            color: #dc3545;
            cursor: pointer;
            font-size: 1em;
            margin-left: 10px;
            padding: 0;
        }
        .ticket-delete-btn:hover {
            color: #bd2130;
        }
        .hide-for-single-discount {
            display: none !important;
        }
        .th-33-percent, .td-33-percent {
            color: purple;
            font-weight: bold;
        }
        .overall-summary-table th, .overall-summary-table td {
            text-align: right;
            padding: 8px;
            border: 1px solid #dee2e6;
        }
        .overall-summary-table th:first-child, .overall-summary-table td:first-child {
            text-align: left;
        }
        .overall-summary-table tfoot th, .overall-summary-table tfoot td {
            font-weight: bold;
            background-color: #e9ecef;
        }
        .overall-summary-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 15px;
            border: 1px solid #ddd;
            padding: 10px;
            border-radius: 5px;
        }
        .overall-summary-grid strong {
            justify-self: start;
        }
        .overall-summary-grid .value {
            justify-self: end;
        }
        .overall-summary-footer {
            margin-top: 20px;
            padding-top: 10px;
            border-top: 1px solid #ccc;
        }
        .summary-total-footer {
            font-size: 1.2em;
            font-weight: bold;
        }
        .summary-total-footer.negative {
            color: red;
        }
        .summary-total-footer.positive {
            color: green;
        }
        .summary-total-status {
            font-size: 1.1em;
            font-weight: bold;
            margin-top: 5px;
        }
        .summary-total-status.negative {
            color: red;
        }
        .summary-total-status.positive {
            color: green;
        }
        .specific-summary-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 1.2em;
        }
        .specific-summary-table td {
            padding: 8px 0;
            border-bottom: 1px solid #eee;
        }
        .specific-summary-table td:first-child {
            font-weight: bold;
        }
        .specific-summary-table td:last-child {
            text-align: right;
            font-weight: bold;
        }
        .specific-summary-table tr:last-child td {
            border-bottom: none;
        }
        .specific-summary-table .negative-value {
            color: #dc3545;
        }
        .specific-summary-table .positive-value {
            color: green;
        }
        .specific-summary-table .total-row td {
            border-top: 2px solid #000;
        }
        .summary-status-row td {
            padding-top: 15px;
        }
        .summary-status-row .status-text {
            color: green;
            font-weight: bold;
        }
        .summary-status-row .status-text.negative {
            color: #dc3545;
        }
        .summary-table-container {
            padding: 20px;
        }
        /* เพิ่ม CSS นี้เพื่อซ่อนคอลัมน์ */
        .hide-grand-total {
            display: none !important;
        }
    </style>
</head>
<body>
    <h4 class="mb-3 p-4">ระบบคีย์เลขหวย</h4>

    <div class="container-fluid">
        <div class="row">
            <div class="col-12">
                
                <div class="mb-3 d-flex gap-2 align-items-center">
                    <label for="userSelect">👤 เลือกลูกค้า:</label>
                    <select id="userSelect" class="form-select w-auto" onchange="switchUser()">
                        
                    </select>
                </div>
                
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

                
                <div id="discountInputGroup" class="mb-3">
                    <label for="discountInput"><span id="specialDiscountLabel">🔻 ตั้งเลขเฉพาะลด 15%</span> (คั่นด้วย space, enter หรือ ,):</label>
                    <input type="text" id="discountInput" class="form-control" placeholder="เช่น 13 14 15" oninput="updateDiscountList()">
                </div>

                <div id="normalDiscountOptionGroup" class="mb-3 d-flex gap-2 align-items-center" style="display: none;">
                    <label for="discountOption">📉 ส่วนลดเลขปกติ:</label>
                    <select id="discountOption" class="form-select w-auto" onchange="updateNormalDiscountDisplay(); updateTableHeaderDiscount();">
                        <option value="0">ไม่ลด</option>
                        <option value="17">ลด 17%</option>
                        <option value="19">ลด 19%</option>
                        <option value="20">ลด 20%</option>
                        <option value="23">ลด 23%</option>
                        <option value="25">ลด 25%</option>
                        <option value="26">ลด 26%</option>
                        <option value="27">ลด 27%</option>
                        <option value="28">ลด 28%</option>
                        <option value="33">ลด 33%</option>
                    </select>
                </div>

                <div class="d-flex justify-content-start gap-2 mt-3">
                    <button class="btn btn-yellow" onclick="generate6Reverse()">6 กลับ</button>
                    <button class="btn btn-yellow" onclick="reverseInputNumbers()">กลับเลข</button>
                </div>

                <div class="row g-2 align-items-center mb-3 input-amounts-group">
                    <div class="col-auto">
                        <input type="text" id="billTime" class="form-control" placeholder="เช่น 15:30">
                    </div>
                    <div class="col-auto">
                        <input type="text" id="inputNumber" class="form-control" placeholder="ใส่เลข เช่น 13 31 23" 
                            oninput="displayInputNumbers()" 
                            onkeydown="handleInputNumberKeydown(event)" 
                            onblur="handleInputNumberBlur()"
                            autocomplete="off">
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

                <div class="d-flex justify-content-start gap-2 mt-3">
                    <button class="btn btn-red" onclick="clearBill()">🗑 ล้างบิล</button>
                    <button class="btn btn-blue" onclick="removeDuplicates()">🚫 ลบเลขซ้ำ</button>
                    <button class="btn btn-purple" onclick="clearAllData()">🧹 ล้างเลขทั้งหมด</button>
                    <div class="ms-auto d-flex gap-2">
                        <button class="btn btn-dark-green" onclick="saveCurrentBillSummary()">💾 บันทึกยอดรวม</button>
                        <button class="btn btn-primary" onclick="showOverallSummary()">📊 สรุปยอดทั้งหมด</button>
                        <button class="btn btn-info" onclick="showTimeBasedSummaryModal()">📸 บันทึกตารางสรุปเป็นรูปภาพ</button>
                    </div>
                </div>

                <div id="currentBillDisplay" class="mt-4">
                    <h5>รายการบิล</h5>
                </div>

                <div class="summary-layout">
                    <div class="main-bill-summary">
                        <h5 class="mt-2 mb-2"><span id="totalTime" class="total-time-display"></span></h5>
                        <h5 class="mb-2">💲 รวมยอดซื้อทั้งหมด (ก่อนหัก %): <span id="grandTotalBeforeDiscount">0</span> บาท</h5>
                        
                        <h5 class="mb-2" id="specialTotalRow"><span id="mainSpecialDiscountText">🟢 รวมยอดเลขเฉพาะลด 15%</span>: <span id="discountTotal">0</span> บาท</h5>
                        <h5 class="mb-2" id="normalDiscountTotalRow" style="display: none;">🔵 รวมยอดซื้อส่วนลด <span id="normalDiscountRateDisplay"></span>: <span id="normalDiscountTotal">0</span> บาท</h5>
                        <h5 class="mb-2" id="singleDiscountRow" style="display:none;">🎉 ยอดซื้อรวม (หลังหักส่วนลด <span id="singleDiscountRateDisplay"></span>): <span id="singleDiscountTotalAmount">0</span> บาท</h5>

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
                                <th class="hide-grand-total">ยอดรวมก่อนหัก %</th>
                                <th id="singleDiscountTableHeader" style="display:none;">ยอดซื้อส่วนลด</th>
                                <th id="specialTotalTableHeader">ยอดเฉพาะลด 15%</th>
                                <th id="normalDiscountTableHeader">ยอดซื้อส่วนลด % ตามชื่อลูกค้า</th>
                                <th>ยอดรวมทั้งหมด</th>
                                <th class="text-center">ตัวเลขที่ถูก (จำนวนซื้อ)</th>
                                <th>ยอดถูกรางวัล</th>
                                <th>ลบ</th>
                            </tr>
                        </thead>
                        <tbody id="timeBasedSummaryTableBody">
                        </tbody>
                        <tfoot id="timeBasedSummaryTableFoot">
                        </tfoot>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <div class="modal fade" id="overallSummaryModal" tabindex="-1" aria-labelledby="overallSummaryModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg modal-dialog-centered modal-dialog-scrollable">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="overallSummaryModalLabel">📊 สรุปยอดทั้งหมด</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div id="overallSummaryHeader">
                        <p class="mb-1"><strong>ลูกค้า:</strong> <span id="overallSummaryCustomerName"></span></p>
                    </div>
                    <div class="modal-body-content">
                        <table class="specific-summary-table">
                            <tbody>
                                <tr>
                                    <td>ยอดซื้อทั้งหมด</td>
                                    <td><span id="summaryTotalPurchase"></span></td>
                                </tr>
                                <tr id="summaryRow15">
                                    <td>ยอดซื้อ 15%</td>
                                    <td><span id="summaryPurchase15"></span></td>
                                </tr>
                                <tr id="summaryRow28">
                                    <td>ยอดซื้อ 28%</td>
                                    <td><span id="summaryPurchase28"></span></td>
                                </tr>
                                <tr id="summaryRow33">
                                    <td>ยอดซื้อ 33%</td>
                                    <td><span id="summaryPurchase33"></span></td>
                                </tr>
                                <tr id="summaryRowOther">
                                    <td>ยอดซื้อส่วนลดอื่น ๆ</td>
                                    <td><span id="summaryPurchaseOther"></span></td>
                                </tr>
                                <tr>
                                    <td>ยอดถูก</td>
                                    <td><span id="summaryWinningAmount"></span></td>
                                </tr>
                                <tr class="total-row">
                                    <td>สรุปยอดหักยอดถูก</td>
                                    <td><span id="summaryNetTotal"></span></td>
                                </tr>
                                <tr class="summary-status-row">
                                    <td colspan="2"><span id="summaryStatusText" class="status-text"></span></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">ปิด</button>
                    <button type="button" class="btn btn-primary" onclick="printOverallSummary()">พิมพ์</button>
                    <button type="button" class="btn btn-info" onclick="saveOverallSummaryAsImage()">📸 บันทึกเป็นรูปภาพ</button>
                </div>
            </div>
        </div>
    </div>
    
    <div class="modal fade" id="timeBasedSummaryModal" tabindex="-1" aria-labelledby="timeBasedSummaryModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-xl modal-dialog-centered modal-dialog-scrollable">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="timeBasedSummaryModalLabel">📸 สรุปยอดรวมตามเวลา</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="summary-table-container">
                        <div id="timeBasedSummaryTableContainerModal">
                            </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">ปิด</button>
                    <button type="button" class="btn btn-info" onclick="saveTimeBasedSummaryAsImage()">💾 บันทึกรูปตารางสรุป</button>
                </div>
            </div>
        </div>
    </div>


    <script>
        const users = {
            "น้อง17%": { group: "กลุ่ม A" },
            "พี่วาด17%": { group: "กลุ่ม A" },
            "AaAnn19%": { group: "กลุ่ม B" },
            "a_akharadet19%": { group: "กลุ่ม B" },
            "กิ๊ฟ 20%": { group: "กลุ่ม C" },
            "น้องหมวย15/20%": { group: "กลุ่ม D" },
            "พี่บูม17/23%": { group: "กลุ่ม E" },
            "พี่หน่อย17/23%": { group: "กลุ่ม E" },
            "พี่เมย์17/23%": { group: "กลุ่ม E" },
            "น้องคิม15/25": { group: "กลุ่ม F" },
            "น้องแคท15/25": { group: "กลุ่ม F" },
            "Gorn15/25": { group: "กลุ่ม F" },
            "มอสGorn15/25": { group: "กลุ่ม F" },
            "น้องส้มGorn15/25": { group: "กลุ่ม F" },
            "น้องเอ็ม15/25": { group: "กลุ่ม F" },
            "แคททรียา15/25": { group: "กลุ่ม F" },
            "Best15/25": { group: "กลุ่ม F" },
            "N'ปาร์ม15/25": { group: "กลุ่ม F" },
            "พี่กล้วย15/25": { group: "กลุ่ม F" },
            "อีฟ15/25": { group: "กลุ่ม F" },
            "น้องเกด15/25": { group: "กลุ่ม F" },
            "น้องเมย์สวย15/25": { group: "กลุ่ม F" },
            "ตาล15/25": { group: "กลุ่ม F" },
            "พี่อี๊ด15/25": { group: "กลุ่ม F" },
            "น้องมด15/25": { group: "กลุ่ม F" },
            "เมแวงใหญ่15/25": { group: "กลุ่ม F" },
            "Kong15/25": { group: "กลุ่ม F" },
            "ฝน15/25": { group: "กลุ่ม F" },
            "พีท15/25": { group: "กลุ่ม F" },
            "Artty15/25": { group: "กลุ่ม F" },
            "ต่าย15/25": { group: "กลุ่ม F" },
            "น้องหญิง15/25": { group: "กลุ่ม F" },
            "น้ำฝน15/25": { group: "กลุ่ม F" },
            "Ploy15/25": { group: "กลุ่ม F" },
            "หมวย15/25": { group: "กลุ่ม F" },
            "น้องมุก15/25": { group: "กลุ่ม F" },
            "คุณรุ้ง15/25": { group: "กลุ่ม F" },
            "บิ๊กแอน15/25": { group: "กลุ่ม F" },
            "ปุ๋ย15/26": { group: "กลุ่ม G" },
            "ยุ้ย15/26": { group: "กลุ่ม G" },
            "น้องอุ้ม15/26": { group: "กลุ่ม G" },
            "เตย15/27": { group: "กลุ่ม H" },
            "พี่จ๋อม15/28": { group: "กลุ่ม I" },
            "พี่โอ๊ต15/28/33": { group: "กลุ่ม J" },
            "Joy15/28/33": { group: "กลุ่ม J" },
            "Boss15/28/33": { group: "กลุ่ม J" },
            "พี่ใหม่15/28/33": { group: "กลุ่ม J" },
            "Deaw15/28/33": { group: "กลุ่ม J" },
            "พี่กวาง 30%": { group: "กลุ่ม K" },
            "ลูกค้าทั่วไป": { group: "ลูกค้าทั่วไป" }
        };
        const discountGroups = {
            "กลุ่ม A": {
                twoDigitNormalDiscount: 17,
                threeDigitNormalDiscount: 17,
                specialDiscount: 17,
                isSingleDiscountType: true
            },
            "กลุ่ม B": {
                twoDigitNormalDiscount: 19,
                threeDigitNormalDiscount: 19,
                specialDiscount: 19,
                isSingleDiscountType: true
            },
            "กลุ่ม C": {
                twoDigitNormalDiscount: 20,
                threeDigitNormalDiscount: 20,
                specialDiscount: 20,
                isSingleDiscountType: true
            },
            "กลุ่ม D": {
                twoDigitNormalDiscount: 20,
                threeDigitNormalDiscount: 20,
                specialDiscount: 15,
                isSingleDiscountType: false
            },
            "กลุ่ม E": {
                twoDigitNormalDiscount: 23,
                threeDigitNormalDiscount: 23,
                specialDiscount: 17,
                isSingleDiscountType: false
            },
            "กลุ่ม F": {
                twoDigitNormalDiscount: 25,
                threeDigitNormalDiscount: 25,
                specialDiscount: 15,
                isSingleDiscountType: false
            },
            "กลุ่ม G": {
                twoDigitNormalDiscount: 26,
                threeDigitNormalDiscount: 26,
                specialDiscount: 15,
                isSingleDiscountType: false
            },
            "กลุ่ม H": {
                twoDigitNormalDiscount: 27,
                threeDigitNormalDiscount: 27,
                specialDiscount: 15,
                isSingleDiscountType: false
            },
            "กลุ่ม I": {
                twoDigitNormalDiscount: 28,
                threeDigitNormalDiscount: 28,
                specialDiscount: 15,
                isSingleDiscountType: false
            },
            "กลุ่ม J": {
                twoDigitNormalDiscount: 28,
                threeDigitNormalDiscount: 33,
                specialDiscount: 15,
                isSingleDiscountType: false
            },
            "กลุ่ม K": {
                twoDigitNormalDiscount: 30,
                threeDigitNormalDiscount: 30,
                specialDiscount: 30,
                isSingleDiscountType: true
            },
            "ลูกค้าทั่วไป": {
                twoDigitNormalDiscount: 0,
                threeDigitNormalDiscount: 0,
                specialDiscount: 0,
                isSingleDiscountType: false
            }
        };
        let currentUserName = localStorage.getItem('selectedUser');
        if (!currentUserName || !users[currentUserName]) {
            currentUserName = Object.keys(users)[0];
            localStorage.setItem('selectedUser', currentUserName);
        }
        let currentUserGroup = users[currentUserName] ? discountGroups[users[currentUserName].group] : discountGroups["ลูกค้าทั่วไป"];
        if (!currentUserGroup) {
            currentUserGroup = discountGroups["ลูกค้าทั่วไป"];
        }

        let discountNumbers = [];
        let correctTwoDigitsUpper = [];
        let correctTwoDigitsLower = [];
        let correctThreeDigitsDirect = [];
        let correctThreeDigitsTods = [];
        const payoutRates = { '2-digit-upper': 70, '2-digit-lower': 70, '3-digit-direct': 500, '3-digit-tods': 100 };
        const userSelect = document.getElementById('userSelect');
        const specialDiscountLabel = document.getElementById('specialDiscountLabel');
        const mainSpecialDiscountText = document.getElementById('mainSpecialDiscountText');
        const specialTotalTableHeader = document.getElementById('specialTotalTableHeader');
        const discountInputGroup = document.getElementById('discountInputGroup');
        const normalDiscountOptionGroup = document.getElementById('normalDiscountOptionGroup');
        const normalDiscountTotalRow = document.getElementById('normalDiscountTotalRow');
        const normalDiscountRateDisplaySpan = document.getElementById('normalDiscountRateDisplay');
        const normalDiscountTotalSpan = document.getElementById('normalDiscountTotal');
        const specialTotalRow = document.getElementById('specialTotalRow');
        const singleDiscountRow = document.getElementById('singleDiscountRow');
        const singleDiscountRateDisplay = document.getElementById('singleDiscountRateDisplay');
        const singleDiscountTotalAmount = document.getElementById('singleDiscountTotalAmount');
        const singleDiscountTableHeader = document.getElementById('singleDiscountTableHeader');
        const numberList = document.getElementById('numberList');
        const currentBillDisplayDiv = document.getElementById('currentBillDisplay');
        const totalDisplaySpan = document.getElementById('totalDisplay');
        const discountTotalSpan = document.getElementById('discountTotal');
        const totalTimeSpan = document.getElementById('totalTime');
        const grandTotalBeforeDiscountSpan = document.getElementById('grandTotalBeforeDiscount');
        const inputNumberInput = document.getElementById('inputNumber');
        const inputNumbersDisplayDiv = document.getElementById('inputNumbersDisplay');
        const totalWinningAmountSpan = document.getElementById('totalWinningAmount');
        const winningNumbersDisplayDiv = document.getElementById('winningNumbersDisplay');
        const timeBasedSummaryTableBody = document.getElementById('timeBasedSummaryTableBody');
        const timeBasedSummaryTableFoot = document.getElementById('timeBasedSummaryTableFoot');
        const normalDiscountTableHeader = document.getElementById('normalDiscountTableHeader');
        const upperAmountInput = document.getElementById('upperAmount');
        const lowerAmountInput = document.getElementById('lowerAmount');
        const todsAmountInput = document.getElementById('todsAmount');
        const billTimeInput = document.getElementById('billTime');
        const correctThreeDigitsDirectInput = document.getElementById('correctThreeDigitsDirectInput');
        const correctThreeDigitsTodsInput = document.getElementById('correctThreeDigitsTodsInput');
        const correctTwoDigitsUpperInput = document.getElementById('correctTwoDigitsUpperInput');
        const correctTwoDigitsLowerInput = document.getElementById('correctTwoDigitsLowerInput');
        const discountInput = document.getElementById('discountInput');
        const discountOptionSelect = document.getElementById('discountOption');
        const overallSummaryGrid = document.getElementById('overallSummaryGrid');
        const overallSummaryNetTotalDiv = document.getElementById('overallSummaryNetTotal');
        const overallSummaryStatusDiv = document.getElementById('overallSummaryStatus');
        const timeBasedSummaryTableContainerModal = document.getElementById('timeBasedSummaryTableContainerModal');


        let total = 0;
        let normalDiscountTotal = 0;
        let discountTotal = 0;
        let grandTotalBeforeDiscount = 0;
        let currentWinningDetails = [];
        let lastBillTime = '';
        let currentBillEntries = [];
        let timeBasedSummaries = [];

        function populateUserSelect() {
            userSelect.innerHTML = '';
            for (const userName in users) {
                const option = document.createElement('option');
                option.value = userName;
                option.textContent = userName;
                userSelect.appendChild(option);
            }
            userSelect.value = currentUserName;
        }

        function loadUserData() {
            const userBillEntriesKey = `billEntries_${currentUserName}`;
            const userSummariesKey = `summaries_${currentUserName}`;
            const userDiscountInputKey = `discountInput_${currentUserName}`;
            const correctThreeDigitsDirectKey = `correctThreeDigitsDirect_${currentUserName}`;
            const correctThreeDigitsTodsKey = `correctThreeDigitsTods_${currentUserName}`;
            const correctTwoDigitsUpperKey = `correctTwoDigitsUpper_${currentUserName}`;
            const correctTwoDigitsLowerKey = `correctTwoDigitsLower_${currentUserName}`;
            
            currentBillEntries = JSON.parse(localStorage.getItem(userBillEntriesKey)) || [];
            timeBasedSummaries = JSON.parse(localStorage.getItem(userSummariesKey)) || [];
            discountInput.value = localStorage.getItem(userDiscountInputKey) || '';
            correctThreeDigitsDirectInput.value = localStorage.getItem(correctThreeDigitsDirectKey) || '';
            correctThreeDigitsTodsInput.value = localStorage.getItem(correctThreeDigitsTodsKey) || '';
            correctTwoDigitsUpperInput.value = localStorage.getItem(correctTwoDigitsUpperKey) || '';
            correctTwoDigitsLowerInput.value = localStorage.getItem(correctTwoDigitsLowerKey) || '';
            const userGroup = users[currentUserName] ? users[currentUserName].group : "ลูกค้าทั่วไป";
            currentUserGroup = discountGroups[userGroup] ? discountGroups[userGroup] : discountGroups["ลูกค้าทั่วไป"];
            sortSummariesByTime();
        }

        function saveUserData() {
            const userBillEntriesKey = `billEntries_${currentUserName}`;
            const userSummariesKey = `summaries_${currentUserName}`;
            const userDiscountInputKey = `discountInput_${currentUserName}`;
            const correctThreeDigitsDirectKey = `correctThreeDigitsDirect_${currentUserName}`;
            const correctThreeDigitsTodsKey = `correctThreeDigitsTods_${currentUserName}`;
            const correctTwoDigitsUpperKey = `correctTwoDigitsUpper_${currentUserName}`;
            const correctTwoDigitsLowerKey = `correctTwoDigitsLower_${currentUserName}`;

            localStorage.setItem(userBillEntriesKey, JSON.stringify(currentBillEntries));
            localStorage.setItem(userSummariesKey, JSON.stringify(timeBasedSummaries));
            localStorage.setItem(userDiscountInputKey, discountInput.value);
            localStorage.setItem(correctThreeDigitsDirectKey, correctThreeDigitsDirectInput.value);
            localStorage.setItem(correctThreeDigitsTodsKey, correctThreeDigitsTodsInput.value);
            localStorage.setItem(correctTwoDigitsUpperKey, correctTwoDigitsUpperInput.value);
            localStorage.setItem(correctTwoDigitsLowerKey, correctTwoDigitsLowerInput.value);
        }

        function switchUser() {
            const newUserName = userSelect.value;
            if (newUserName !== currentUserName) {
                saveUserData();
                currentUserName = newUserName;
                localStorage.setItem('selectedUser', currentUserName);
                loadUserData();
                updateDisplay();
            }
        }

        function initializeApp() {
            populateUserSelect();
            loadUserData();
            updateDiscountList();
            updateCorrectNumbers();
            updateDisplay();
        }
        
        function sortSummariesByTime() {
            timeBasedSummaries.sort((a, b) => {
                const timeA = a.time.split(':').map(Number);
                const timeB = b.time.split(':').map(Number);
                if (timeA.length === 2 && timeB.length === 2) {
                    if (timeA[0] !== timeB[0]) {
                        return timeA[0] - timeB[0];
                    }
                    return timeA[1] - timeB[1];
                }
                return 0;
            });
        }

        function updateDisplayBasedOnDiscountGroup() {
            const isSingle = currentUserGroup.isSingleDiscountType;
            const isGroupJ = users[currentUserName]?.group === "กลุ่ม J";

            discountInputGroup.style.display = isSingle ? 'none' : 'block';
            normalDiscountOptionGroup.style.display = 'none';
            specialTotalRow.style.display = isSingle ? 'none' : 'block';
            normalDiscountTotalRow.style.display = (isSingle || isGroupJ) ? 'none' : 'block';
            singleDiscountRow.style.display = isSingle ? 'block' : 'none';
            specialTotalTableHeader.style.display = isSingle ? 'none' : '';
            normalDiscountTableHeader.style.display = (isSingle || isGroupJ) ? 'none' : '';
            singleDiscountTableHeader.style.display = isSingle ? '' : 'none';

            if (isSingle) {
                const rate = currentUserGroup.twoDigitNormalDiscount;
                singleDiscountRateDisplay.textContent = `${rate}%`;
            } else {
                const normalRate = currentUserGroup.twoDigitNormalDiscount;
                const specialRate = currentUserGroup.specialDiscount;
                normalDiscountRateDisplaySpan.textContent = `(2 ตัว ${normalRate}%, 3 ตัว ${currentUserGroup.threeDigitNormalDiscount}%)`;
                specialDiscountLabel.innerHTML = `🔻 ตั้งเลขเฉพาะลด ${specialRate}%</span> (คั่นด้วย space, enter หรือ ,):`;
                mainSpecialDiscountText.innerHTML = `🟢 รวมยอดเลขเฉพาะลด ${specialRate}%`;
                specialTotalTableHeader.textContent = `ยอดเฉพาะลด ${specialRate}%`;
                updateTableHeaderDiscount();
            }
        }
        
        function updateTableHeaderDiscount() {
            const isGroupJ = users[currentUserName]?.group === "กลุ่ม J";
            const isSingle = currentUserGroup.isSingleDiscountType;
            const theadRow = document.querySelector('#timeBasedSummaryTable thead tr');
            
            theadRow.innerHTML = ''; // Clear existing headers

            // Add common headers
            theadRow.innerHTML += '<th>เวลา</th>';
            theadRow.innerHTML += '<th class="hide-grand-total">ยอดรวมก่อนหัก %</th>';

            // Add headers based on discount type
            if (isSingle) {
                const rate = currentUserGroup.twoDigitNormalDiscount;
                theadRow.innerHTML += `<th id="singleDiscountTableHeader">ยอดซื้อส่วนลด ${rate}%</th>`;
            } else if (isGroupJ) {
                theadRow.innerHTML += `<th>ยอดเฉพาะลด ${currentUserGroup.specialDiscount}%</th>`;
                theadRow.innerHTML += `<th class="th-28-percent">ยอดซื้อส่วนลด ${currentUserGroup.twoDigitNormalDiscount}%</th>`;
                theadRow.innerHTML += `<th class="th-33-percent">ยอดซื้อส่วนลด ${currentUserGroup.threeDigitNormalDiscount}%</th>`;
            } else { // Normal discount group
                theadRow.innerHTML += `<th>ยอดเฉพาะลด ${currentUserGroup.specialDiscount}%</th>`;
                theadRow.innerHTML += `<th>ยอดซื้อส่วนลด ${currentUserGroup.twoDigitNormalDiscount}%</th>`;
            }
            
            // Add common headers
            theadRow.innerHTML += '<th>ยอดรวมทั้งหมด</th>';
            theadRow.innerHTML += '<th class="text-center">ตัวเลขที่ถูก (จำนวนซื้อ)</th>';
            theadRow.innerHTML += '<th>ยอดถูกรางวัล</th>';
            theadRow.innerHTML += '<th>ลบ</th>';
        }

        function updateNormalDiscountDisplay() {}
        function generate6Reverse() {
            const inputNum = inputNumberInput.value.trim();
            if (inputNum.length !== 2 && inputNum.length !== 3) {
                alert("กรุณาใส่เลข 2 หรือ 3 หลักเพื่อกลับ 6 กลับ");
                return;
            }
            const numbers = inputNum.split(/\s*,\s*|\s+/).filter(n => n);
            const generatedNumbers = new Set();
            numbers.forEach(num => {
                if (num.length === 3) {
                    const digits = num.split('').map(Number);
                    const uniqueDigits = new Set(digits);
                    if (uniqueDigits.size < 3) {
                        alert("เลข 3 ตัวที่มีเลขซ้ำไม่สามารถใช้ 6 กลับได้");
                        return;
                    }
                    const permutations = [
                        [digits[0], digits[1], digits[2]],
                        [digits[0], digits[2], digits[1]],
                        [digits[1], digits[0], digits[2]],
                        [digits[1], digits[2], digits[0]],
                        [digits[2], digits[0], digits[1]],
                        [digits[2], digits[1], digits[0]]
                    ];
                    permutations.forEach(p => generatedNumbers.add(p.join('')));
                }
            });
            inputNumberInput.value = [...generatedNumbers].join(' ');
            displayInputNumbers();
        }
        function reverseInputNumbers() {
            const input = inputNumberInput.value.trim();
            if (!input) return;
            const numbers = input.split(/\s*,\s*|\s+/).filter(n => n);
            const reversedNumbers = numbers.map(n => n.split('').reverse().join(''));
            const allNumbers = [...new Set([...numbers, ...reversedNumbers])].filter(n => n);
            inputNumberInput.value = allNumbers.join(' ');
            displayInputNumbers();
        }
        function handleInputNumberKeydown(event) {
            if (event.key === ' ' || event.key === ',' || event.key === 'Enter') {
                event.preventDefault();
                const currentInput = inputNumberInput.value.trim();
                if (currentInput.endsWith(' ')) {
                } else {
                    inputNumberInput.value = currentInput + ' ';
                }
                displayInputNumbers();
            }
        }
        function handleInputNumberBlur() {
            displayInputNumbers();
        }
        function displayInputNumbers() {
            const input = inputNumberInput.value.trim();
            inputNumbersDisplayDiv.innerHTML = '';
            if (input) {
                const numbers = input.split(/\s*,\s*|\s+/).filter(n => n);
                const uniqueNumbers = [...new Set(numbers)];
                uniqueNumbers.forEach(num => {
                    const span = document.createElement('span');
                    span.className = 'displayed-number';
                    span.textContent = num;
                    span.setAttribute('data-number', num);
                    span.onclick = () => removeDisplayedNumber(num);
                    inputNumbersDisplayDiv.appendChild(span);
                });
            }
        }
        function removeDisplayedNumber(numberToRemove) {
            const currentInput = inputNumberInput.value.trim();
            const numbers = currentInput.split(/\s*,\s*|\s+/).filter(n => n);
            const updatedNumbers = numbers.filter(n => n !== numberToRemove);
            inputNumberInput.value = updatedNumbers.join(' ');
            displayInputNumbers();
        }
        function addNumber() {
            const billTime = billTimeInput.value.trim();
            const numbersInput = inputNumberInput.value.trim();
            const upperAmount = parseFloat(upperAmountInput.value) || 0;
            const lowerAmount = parseFloat(lowerAmountInput.value) || 0;
            const todsAmount = parseFloat(todsAmountInput.value) || 0;
            if (!billTime || !numbersInput) {
                alert("กรุณาใส่เวลาและตัวเลข");
                return;
            }
            if (upperAmount === 0 && lowerAmount === 0 && todsAmount === 0) {
                alert("กรุณาใส่จำนวนเงินอย่างน้อยหนึ่งช่อง");
                return;
            }
            const newEntries = numbersInput.split(/\s*,\s*|\s+/).filter(n => n).map(num => ({ time: billTime, number: num, upper: upperAmount, lower: lowerAmount, tods: todsAmount }));
            currentBillEntries.push(...newEntries);
            saveUserData();
            lastBillTime = billTime;
            clearInputFields();
            billTimeInput.value = billTime;
            updateDisplay();
        }
        function clearInputFields() {
            inputNumberInput.value = '';
            upperAmountInput.value = '';
            lowerAmountInput.value = '';
            todsAmountInput.value = '';
            inputNumbersDisplayDiv.innerHTML = '';
        }
        function removeBillEntry(index) {
            currentBillEntries.splice(index, 1);
            saveUserData();
            updateDisplay();
        }
        function clearBill() {
            currentBillEntries = [];
            saveUserData();
            updateDisplay();
        }
        function removeDuplicates() {
            const uniqueNumbers = new Set();
            currentBillEntries = currentBillEntries.filter(entry => {
                const key = entry.number;
                if (!uniqueNumbers.has(key)) {
                    uniqueNumbers.add(key);
                    return true;
                }
                return false;
            });
            saveUserData();
            updateDisplay();
        }
        function clearAllData() {
            if (confirm("คุณต้องการล้างข้อมูลทั้งหมดสำหรับผู้ใช้นี้หรือไม่? รวมถึงสรุปยอดและข้อมูลเลขถูกด้วย")) {
                currentBillEntries = [];
                timeBasedSummaries = [];
                discountNumbers = [];
                correctThreeDigitsDirect = [];
                correctThreeDigitsTods = [];
                correctTwoDigitsUpper = [];
                correctTwoDigitsLower = [];
                discountInput.value = '';
                correctThreeDigitsDirectInput.value = '';
                correctThreeDigitsTodsInput.value = '';
                correctTwoDigitsUpperInput.value = '';
                correctTwoDigitsLowerInput.value = '';
                saveUserData();
                updateDisplay();
            }
        }
        function updateDiscountList() {
            const input = discountInput.value;
            discountNumbers = input.split(/\s*,\s*|\s+/).filter(n => n).map(n => n.trim());
            updateTotals();
        }
        function updateCorrectNumbers() {
            correctThreeDigitsDirect = correctThreeDigitsDirectInput.value.split(/\s*,\s*|\s+/).filter(n => n).map(n => n.trim());
            correctThreeDigitsTods = correctThreeDigitsTodsInput.value.split(/\s*,\s*|\s+/).filter(n => n).map(n => n.trim());
            correctTwoDigitsUpper = correctTwoDigitsUpperInput.value.split(/\s*,\s*|\s+/).filter(n => n).map(n => n.trim());
            correctTwoDigitsLower = correctTwoDigitsLowerInput.value.split(/\s*,\s*|\s+/).filter(n => n).map(n => n.trim());
            saveUserData();
            updateDisplay();
        }
        function calculateWinnings(entries) {
            let totalWinnings = 0;
            currentWinningDetails = [];
            entries.forEach(entry => {
                const num = entry.number;
                if (num.length === 3 && correctThreeDigitsDirect.includes(num) && entry.upper > 0) {
                    const winningAmount = entry.upper * payoutRates['3-digit-direct'];
                    totalWinnings += winningAmount;
                    currentWinningDetails.push({ number: num, type: '3 ตัวตรง', amount: winningAmount, purchase: entry.upper });
                }
                if (num.length === 3 && correctThreeDigitsTods.some(correctNum => arePermutations(num, correctNum)) && entry.tods > 0) {
                    const winningAmount = entry.tods * payoutRates['3-digit-tods'];
                    totalWinnings += winningAmount;
                    currentWinningDetails.push({ number: num, type: '3 ตัวโต๊ด', amount: winningAmount, purchase: entry.tods });
                }
                if (num.length === 2 && correctTwoDigitsUpper.includes(num) && entry.upper > 0) {
                    const winningAmount = entry.upper * payoutRates['2-digit-upper'];
                    totalWinnings += winningAmount;
                    currentWinningDetails.push({ number: num, type: '2 ตัวบน', amount: winningAmount, purchase: entry.upper });
                }
                if (num.length === 2 && correctTwoDigitsLower.includes(num) && entry.lower > 0) {
                    const winningAmount = entry.lower * payoutRates['2-digit-lower'];
                    totalWinnings += winningAmount;
                    currentWinningDetails.push({ number: num, type: '2 ตัวล่าง', amount: winningAmount, purchase: entry.lower });
                }
            });
            return totalWinnings;
        }

        function updateTotals() {
            total = 0;
            normalDiscountTotal = 0;
            discountTotal = 0;
            grandTotalBeforeDiscount = 0;
            let currentTotalWinningAmount = 0;
            
            const isSingle = currentUserGroup.isSingleDiscountType;
            const normalRate = currentUserGroup.twoDigitNormalDiscount;
            const threeDigitNormalRate = currentUserGroup.threeDigitNormalDiscount;
            const specialRate = currentUserGroup.specialDiscount;

            const isGroupJ = users[currentUserName]?.group === "กลุ่ม J";
            
            let specialTotal = 0;
            let normalTwoDigitAmount = 0;
            let normalThreeDigitAmount = 0;
            let normalAmountForNonJGroup = 0;
            
            currentBillEntries.forEach(entry => {
                const num = entry.number;
                const upper = entry.upper;
                const lower = entry.lower;
                const tods = entry.tods;
                const totalEntryAmount = upper + lower + tods;

                grandTotalBeforeDiscount += totalEntryAmount;

                if (isSingle) {
                    // All purchases fall under the single discount rate
                    normalAmountForNonJGroup += totalEntryAmount;
                } else if (discountNumbers.includes(num)) {
                    specialTotal += totalEntryAmount;
                } else {
                    if (isGroupJ) {
                        if (num.length === 2) {
                            normalTwoDigitAmount += (entry.upper + entry.lower + entry.tods);
                        } else if (num.length === 3) {
                            normalThreeDigitAmount += (entry.upper + entry.tods);
                        }
                    } else {
                         normalAmountForNonJGroup += totalEntryAmount;
                    }
                }
            });

            currentTotalWinningAmount = calculateWinnings(currentBillEntries);

            if (isSingle) {
                total = normalAmountForNonJGroup * (1 - normalRate / 100);
                singleDiscountRateDisplay.textContent = `${normalRate}%`;
                singleDiscountTotalAmount.textContent = total.toFixed(2);
            } else if (isGroupJ) {
                const discountedSpecialTotal = specialTotal * (1 - specialRate / 100);
                const discountedNormalTwoDigitTotal = normalTwoDigitAmount * (1 - normalRate / 100);
                const discountedNormalThreeDigitTotal = normalThreeDigitAmount * (1 - threeDigitNormalRate / 100);
                total = discountedSpecialTotal + discountedNormalTwoDigitTotal + discountedNormalThreeDigitTotal;
                normalDiscountTotalRow.style.display = 'block';
                normalDiscountTotalSpan.textContent = (discountedNormalTwoDigitTotal + discountedNormalThreeDigitTotal).toFixed(2);
            } else {
                const discountedSpecialTotal = specialTotal * (1 - specialRate / 100);
                const discountedNormalTotal = normalAmountForNonJGroup * (1 - normalRate / 100);
                total = discountedSpecialTotal + discountedNormalTotal;
                normalDiscountTotalRow.style.display = 'block';
                normalDiscountTotalSpan.textContent = discountedNormalTotal.toFixed(2);
            }
            
            discountTotalSpan.textContent = specialTotal.toFixed(2);
            grandTotalBeforeDiscountSpan.textContent = grandTotalBeforeDiscount.toFixed(2);
            totalDisplaySpan.textContent = total.toFixed(2);
            totalWinningAmountSpan.textContent = currentTotalWinningAmount.toFixed(2);

            renderCurrentBill();
            renderTimeBasedSummary();
            updateWinningNumbersDisplay();
            saveUserData();
        }

        function saveCurrentBillSummary() {
            if (currentBillEntries.length === 0) {
                alert("ไม่มีรายการบิลที่จะบันทึก");
                return;
            }
            
            // Recalculate totals from scratch to avoid any potential state issues
            let summaryGrandTotalBeforeDiscount = 0;
            let summarySpecialTotal = 0;
            let summaryNormalTwoDigitAmount = 0;
            let summaryNormalThreeDigitAmount = 0;
            let summaryNormalAmountForNonJGroup = 0;
            
            const isGroupJ = users[currentUserName]?.group === "กลุ่ม J";

            currentBillEntries.forEach(entry => {
                const num = entry.number;
                const upper = entry.upper;
                const lower = entry.lower;
                const tods = entry.tods;
                const totalEntryAmount = upper + lower + tods;
                summaryGrandTotalBeforeDiscount += totalEntryAmount;

                if (currentUserGroup.isSingleDiscountType) {
                    summaryNormalAmountForNonJGroup += totalEntryAmount;
                } else if (discountNumbers.includes(num)) {
                    summarySpecialTotal += totalEntryAmount;
                } else {
                    if (isGroupJ) {
                        if (num.length === 2) {
                            summaryNormalTwoDigitAmount += (entry.upper + entry.lower + entry.tods);
                        } else if (num.length === 3) {
                            summaryNormalThreeDigitAmount += (entry.upper + entry.tods);
                        }
                    } else {
                         summaryNormalAmountForNonJGroup += totalEntryAmount;
                    }
                }
            });

            const summaryTotalWinning = calculateWinnings(currentBillEntries);

            const newSummary = {
                time: lastBillTime,
                grandTotalBeforeDiscount: summaryGrandTotalBeforeDiscount,
                discountTotal: summarySpecialTotal,
                normalTwoDigitAmount: summaryNormalTwoDigitAmount,
                normalThreeDigitAmount: summaryNormalThreeDigitAmount,
                normalAmountForNonJGroup: summaryNormalAmountForNonJGroup,
                twoDigitNormalDiscount: currentUserGroup.twoDigitNormalDiscount,
                threeDigitNormalDiscount: currentUserGroup.threeDigitNormalDiscount,
                specialDiscount: currentUserGroup.specialDiscount,
                isSingleDiscountType: currentUserGroup.isSingleDiscountType,
                winningAmount: summaryTotalWinning,
                winningDetails: currentWinningDetails,
                userGroup: users[currentUserName]?.group || "ลูกค้าทั่วไป"
            };

            timeBasedSummaries.push(newSummary);
            saveUserData();
            clearBill();
            billTimeInput.value = '';
            updateDisplay();
        }
        
        function renderTimeBasedSummary() {
            timeBasedSummaryTableBody.innerHTML = '';
            let overallGrandTotalBeforeDiscount = 0;
            let overallTotalWinning = 0;
            sortSummariesByTime();
            
            const isCurrentGroupJ = users[currentUserName]?.group === "กลุ่ม J";
            const currentDiscountTypeIsSingle = currentUserGroup.isSingleDiscountType;
            
            let footSpecialTotal = 0;
            let footNormal2DigitTotal = 0;
            let footNormal3DigitTotal = 0;
            let footSingleDiscountTotal = 0;
            let footTotalAfterDiscount = 0;

            timeBasedSummaries.forEach((summary, index) => {
                const row = document.createElement('tr');
                const isSummaryGroupJ = summary.userGroup === "กลุ่ม J";
                const isSummarySingle = summary.isSingleDiscountType;

                let discountedSpecialTotal = 0;
                let discountedNormal2DigitTotal = 0;
                let discountedNormal3DigitTotal = 0;
                let discountedSingleTotal = 0;
                let totalAfterDiscount = 0;

                if (isSummarySingle) {
                    const discountRate = summary.twoDigitNormalDiscount / 100;
                    discountedSingleTotal = summary.normalAmountForNonJGroup * (1 - discountRate);
                    totalAfterDiscount = discountedSingleTotal;
                } else {
                    const specialRate = summary.specialDiscount / 100;
                    discountedSpecialTotal = summary.discountTotal * (1 - specialRate);
                    if (isSummaryGroupJ) {
                        const normal2Rate = summary.twoDigitNormalDiscount / 100;
                        const normal3Rate = summary.threeDigitNormalDiscount / 100;
                        discountedNormal2DigitTotal = summary.normalTwoDigitAmount * (1 - normal2Rate);
                        discountedNormal3DigitTotal = summary.normalThreeDigitAmount * (1 - normal3Rate);
                        totalAfterDiscount = discountedSpecialTotal + discountedNormal2DigitTotal + discountedNormal3DigitTotal;
                    } else {
                        const normalRate = summary.twoDigitNormalDiscount / 100;
                        discountedNormal2DigitTotal = summary.normalAmountForNonJGroup * (1 - normalRate);
                        totalAfterDiscount = discountedSpecialTotal + discountedNormal2DigitTotal;
                    }
                }
                
                let rowContent = `<td>${summary.time}</td>
                                 <td class="hide-grand-total">${summary.grandTotalBeforeDiscount.toFixed(2)}</td>`;

                if (currentDiscountTypeIsSingle) {
                    rowContent += `<td class="td-single-discount">${discountedSingleTotal.toFixed(2)}</td>`;
                } else if (isCurrentGroupJ) {
                    rowContent += `<td class="td-special-discount">${discountedSpecialTotal.toFixed(2)}</td>`;
                    rowContent += `<td class="td-normal-28">${discountedNormal2DigitTotal.toFixed(2)}</td>`;
                    rowContent += `<td class="td-normal-33">${discountedNormal3DigitTotal.toFixed(2)}</td>`;
                } else {
                    rowContent += `<td class="td-special-discount">${discountedSpecialTotal.toFixed(2)}</td>`;
                    rowContent += `<td class="td-normal-rate">${discountedNormal2DigitTotal.toFixed(2)}</td>`;
                }
                
                rowContent += `<td>${totalAfterDiscount.toFixed(2)}</td>
                               <td class="winning-numbers-cell">${renderWinningDetails(summary.winningDetails)}</td>
                               <td>${summary.winningAmount.toFixed(2)}</td>
                               <td><button class="delete-summary-btn" onclick="deleteSummary(${index})">&times;</button></td>`;
                
                row.innerHTML = rowContent;
                timeBasedSummaryTableBody.appendChild(row);

                overallGrandTotalBeforeDiscount += summary.grandTotalBeforeDiscount;
                overallTotalWinning += parseFloat(summary.winningAmount);
                
                if (isSummaryGroupJ) { // Corrected logic here
                    footSpecialTotal += discountedSpecialTotal;
                    footNormal2DigitTotal += discountedNormal2DigitTotal;
                    footNormal3DigitTotal += discountedNormal3DigitTotal;
                } else if (isSummarySingle) { // Corrected logic here
                    footSingleDiscountTotal += discountedSingleTotal;
                } else {
                    footSpecialTotal += discountedSpecialTotal;
                    footNormal2DigitTotal += discountedNormal2DigitTotal;
                }
                footTotalAfterDiscount += totalAfterDiscount;
            });
            
            const footRow = document.createElement('tr');
            let footContent = `<td>รวม</td>
                               <td class="hide-grand-total">${overallGrandTotalBeforeDiscount.toFixed(2)}</td>`;

            if (currentDiscountTypeIsSingle) {
                footContent += `<td class="td-single-discount">${footSingleDiscountTotal.toFixed(2)}</td>`;
            } else if (isCurrentGroupJ) {
                footContent += `<td class="td-special-discount">${footSpecialTotal.toFixed(2)}</td>`;
                footContent += `<td class="td-normal-28">${footNormal2DigitTotal.toFixed(2)}</td>`;
                footContent += `<td class="td-normal-33">${footNormal3DigitTotal.toFixed(2)}</td>`;
            } else {
                footContent += `<td class="td-special-discount">${footSpecialTotal.toFixed(2)}</td>`;
                footContent += `<td class="td-normal-rate">${footNormal2DigitTotal.toFixed(2)}</td>`;
            }

            footContent += `<td>${footTotalAfterDiscount.toFixed(2)}</td>
                            <td></td>
                            <td>${overallTotalWinning.toFixed(2)}</td>
                            <td></td>`;
            
            footRow.innerHTML = footContent;
            timeBasedSummaryTableFoot.innerHTML = '';
            timeBasedSummaryTableFoot.appendChild(footRow);
            
            updateTableHeaderDiscount();
        }


        function deleteSummary(index) {
            if (confirm("คุณต้องการลบสรุปยอดนี้หรือไม่?")) {
                timeBasedSummaries.splice(index, 1);
                saveUserData();
                updateDisplay();
            }
        }

        function renderCurrentBill() {
            currentBillDisplayDiv.innerHTML = '<h5>รายการบิล</h5>';
            if (currentBillEntries.length === 0) {
                currentBillDisplayDiv.innerHTML += '<p class="text-muted">ยังไม่มีรายการบิล</p>';
                return;
            }

            const groupedByTime = currentBillEntries.reduce((acc, entry) => {
                if (!acc[entry.time]) {
                    acc[entry.time] = [];
                }
                acc[entry.time].push(entry);
                return acc;
            }, {});

            let totalUpper = 0;
            let totalLower = 0;
            let totalTods = 0;
            let totalCount = 0;

            for (const time in groupedByTime) {
                const groupDiv = document.createElement('div');
                groupDiv.className = 'ticket-group';
                
                const groupHeader = document.createElement('div');
                groupHeader.className = 'ticket-header';
                groupHeader.innerHTML = `
                    <span>เวลา: ${time} (${groupedByTime[time].length} รายการ)</span>
                    <span>
                        <button class="ticket-delete-btn" onclick="deleteTimeGroup('${time}')">&times;</button>
                    </span>
                `;
                groupDiv.appendChild(groupHeader);
                
                groupedByTime[time].forEach((entry, index) => {
                    const originalIndex = currentBillEntries.indexOf(entry);
                    const itemDiv = document.createElement('div');
                    itemDiv.className = 'ticket-item';
                    let amounts = [];
                    if (entry.upper > 0) amounts.push(`บน: ${entry.upper}`);
                    if (entry.lower > 0) amounts.push(`ล่าง: ${entry.lower}`);
                    if (entry.tods > 0) amounts.push(`โต๊ด: ${entry.tods}`);

                    itemDiv.innerHTML = `
                        <span class="ticket-number-type">${entry.number}</span>
                        <span class="ticket-amount">${amounts.join(' / ')}</span>
                        <span>
                            <button class="ticket-delete-btn" onclick="removeBillEntry(${originalIndex})">&times;</button>
                        </span>
                    `;
                    groupDiv.appendChild(itemDiv);

                    totalUpper += entry.upper;
                    totalLower += entry.lower;
                    totalTods += entry.tods;
                    totalCount++;
                });

                currentBillDisplayDiv.appendChild(groupDiv);
            }
            const totalsDiv = document.createElement('div');
            totalsDiv.className = 'alert alert-secondary mt-3';
            totalsDiv.innerHTML = `
                <p class="mb-1">รวมยอดบน: ${totalUpper.toFixed(2)}</p>
                <p class="mb-1">รวมยอดล่าง: ${totalLower.toFixed(2)}</p>
                <p class="mb-1">รวมยอดโต๊ด: ${totalTods.toFixed(2)}</p>
                <p class="mb-0">จำนวนเลขทั้งหมด: ${totalCount} ตัว</p>
            `;
            currentBillDisplayDiv.appendChild(totalsDiv);
        }

        function deleteTimeGroup(timeToDelete) {
            if (confirm(`คุณต้องการลบรายการบิลทั้งหมดของเวลา ${timeToDelete} หรือไม่?`)) {
                currentBillEntries = currentBillEntries.filter(entry => entry.time !== timeToDelete);
                saveUserData();
                updateDisplay();
            }
        }
        function updateWinningNumbersDisplay() {
            winningNumbersDisplayDiv.innerHTML = '';
            if (currentWinningDetails.length > 0) {
                const title = document.createElement('h5');
                title.className = 'mt-3 mb-1';
                title.textContent = "🏆 เลขที่ถูกรางวัล:";
                winningNumbersDisplayDiv.appendChild(title);

                const list = document.createElement('ul');
                list.className = 'list-unstyled';
                currentWinningDetails.forEach(detail => {
                    const item = document.createElement('li');
                    item.className = 'winning-number-detail';
                    item.textContent = `เลข ${detail.number} (${detail.type}) ซื้อ ${detail.purchase} ถูก ${detail.amount.toFixed(2)} บาท`;
                    list.appendChild(item);
                });
                winningNumbersDisplayDiv.appendChild(list);
            }
        }

        function updateDisplay() {
            updateDisplayBasedOnDiscountGroup();
            renderCurrentBill();
            updateTotals();
            renderTimeBasedSummary();
            updateWinningNumbersDisplay();
        }
        
        function showOverallSummary() {
            let totalsByRate = {
                '15': 0, '17': 0, '19': 0, '20': 0, '23': 0, '25': 0, '26': 0, '27': 0, '28': 0, '30': 0, '33': 0, 'other': 0
            };
            let totalGrossPurchase = 0;
            let totalWinning = 0;

            timeBasedSummaries.forEach(summary => {
                totalGrossPurchase += summary.grandTotalBeforeDiscount;
                totalWinning += summary.winningAmount;

                if (summary.isSingleDiscountType) {
                    const rate = summary.twoDigitNormalDiscount;
                    const discountedAmount = summary.grandTotalBeforeDiscount * (1 - rate / 100);
                    totalsByRate[rate.toString()] += discountedAmount;
                } else {
                    const specialRate = summary.specialDiscount;
                    const discountedSpecialAmount = summary.discountTotal * (1 - specialRate / 100);
                    totalsByRate[specialRate.toString()] += discountedSpecialAmount;
                    
                    if (summary.userGroup === "กลุ่ม J") {
                        const normalRate2 = summary.twoDigitNormalDiscount;
                        const normalRate3 = summary.threeDigitNormalDiscount;
                        
                        const discountedNormal28Amount = summary.normalTwoDigitAmount * (1 - normalRate2 / 100);
                        const discountedNormal33Amount = summary.normalThreeDigitAmount * (1 - normalRate3 / 100);

                        totalsByRate[normalRate2.toString()] += discountedNormal28Amount;
                        totalsByRate[normalRate3.toString()] += discountedNormal33Amount;
                    } else {
                        const normalRate = summary.twoDigitNormalDiscount;
                        const discountedNormalAmount = summary.normalAmountForNonJGroup * (1 - normalRate / 100);
                        totalsByRate[normalRate.toString()] += discountedNormalAmount;
                    }
                }
            });
            
            const totalAfterDiscount = Object.values(totalsByRate).reduce((acc, val) => acc + val, 0);
            const finalNetTotal = totalAfterDiscount - totalWinning;
            
            document.getElementById('summaryTotalPurchase').textContent = totalGrossPurchase.toFixed(2);

            document.getElementById('summaryPurchase15').textContent = totalsByRate['15'].toFixed(2);
            document.getElementById('summaryRow15').style.display = totalsByRate['15'] > 0 ? '' : 'none';

            document.getElementById('summaryPurchase28').textContent = totalsByRate['28'].toFixed(2);
            document.getElementById('summaryRow28').style.display = totalsByRate['28'] > 0 ? '' : 'none';

            document.getElementById('summaryPurchase33').textContent = totalsByRate['33'].toFixed(2);
            document.getElementById('summaryRow33').style.display = totalsByRate['33'] > 0 ? '' : 'none';

            let otherTotal = 0;
            let otherRates = [];
            for(const rate in totalsByRate) {
                if(rate !== '15' && rate !== '28' && rate !== '33' && totalsByRate[rate] > 0) {
                    otherTotal += totalsByRate[rate];
                    otherRates.push(rate);
                }
            }

            const summaryRowOther = document.getElementById('summaryRowOther');
            const summaryPurchaseOther = document.getElementById('summaryPurchaseOther');
            if (otherTotal > 0) {
                summaryRowOther.style.display = '';
                summaryRowOther.querySelector('td:first-child').textContent = `ยอดซื้อส่วนลด ${otherRates.join(', ')}%`;
                summaryPurchaseOther.textContent = otherTotal.toFixed(2);
            } else {
                summaryRowOther.style.display = 'none';
            }

            const summaryWinningAmountSpan = document.getElementById('summaryWinningAmount');
            summaryWinningAmountSpan.textContent = totalWinning.toFixed(2);
            summaryWinningAmountSpan.classList.add('positive-value');
            
            document.getElementById('summaryNetTotal').textContent = finalNetTotal.toFixed(2);

            const summaryStatusText = document.getElementById('summaryStatusText');
            summaryStatusText.classList.remove('positive', 'negative');
            
            if (finalNetTotal > 0) {
                summaryStatusText.textContent = 'ส่งเจ้า';
                summaryStatusText.classList.add('positive');
            } else {
                summaryStatusText.textContent = 'โอนลูกค้า';
                summaryStatusText.classList.add('negative');
            }
            
            document.getElementById('overallSummaryCustomerName').textContent = currentUserName;

            const overallSummaryModal = new bootstrap.Modal(document.getElementById('overallSummaryModal'));
            overallSummaryModal.show();
        }

        function printOverallSummary() {
             const printContent = document.getElementById('overallSummaryModal').querySelector('.modal-body-content').innerHTML;
             const printWindow = window.open('', '', 'height=600,width=800');
             printWindow.document.write('<html><head><title>สรุปยอดทั้งหมด</title>');
             printWindow.document.write('<style>');
             printWindow.document.write('body { font-family: \'Arial\', sans-serif; font-size: 14px; padding: 20px; }');
             printWindow.document.write('.specific-summary-table { width: 100%; border-collapse: collapse; font-size: 1.2em; }');
             printWindow.document.write('.specific-summary-table td { padding: 8px 0; border-bottom: 1px solid #eee; }');
             printWindow.document.write('.specific-summary-table td:first-child { font-weight: bold; }');
             printWindow.document.write('.specific-summary-table td:last-child { text-align: right; font-weight: bold; }');
             printWindow.document.write('.specific-summary-table tr:last-child td { border-bottom: none; }');
             printWindow.document.write('.specific-summary-table .negative-value { color: #dc3545; }');
             printWindow.document.write('.specific-summary-table .positive-value { color: green; }');
             printWindow.document.write('.specific-summary-table .total-row td { border-top: 2px solid #000; }');
             printWindow.document.write('.summary-status-row td { padding-top: 15px; }');
             printWindow.document.write('.summary-status-row .status-text { color: green; font-weight: bold; }');
             printWindow.document.write('.summary-status-row .status-text.negative { color: #dc3545; }');
             printWindow.document.write('p { margin-bottom: 5px; }');
             printWindow.document.write('</style>');
             printWindow.document.write('</head><body>');
             printWindow.document.write(`<h5>สรุปยอดทั้งหมด - ลูกค้า: ${currentUserName}</h5>`);
             printWindow.document.write(printContent);
             printWindow.document.write('</body></html>');
             printWindow.document.close();
             printWindow.focus();
             printWindow.print();
        }
        
        // New function to show the time-based summary modal
        function showTimeBasedSummaryModal() {
            const container = document.getElementById('timeBasedSummaryTableContainerModal');
            // Clone the table from the main page to show it in the modal
            const table = document.getElementById('timeBasedSummaryTable').cloneNode(true);
            table.style.display = 'table'; // Make sure the table is visible
            
            // Re-render the header and footer to ensure they are correct based on current user settings
            updateTableHeaderDiscountForModal(table);
            
            container.innerHTML = '';
            container.appendChild(table);
            
            const timeBasedSummaryModal = new bootstrap.Modal(document.getElementById('timeBasedSummaryModal'));
            timeBasedSummaryModal.show();
        }

        // Helper function to update table header for the modal, similar to the main one
        function updateTableHeaderDiscountForModal(table) {
            const isGroupJ = users[currentUserName]?.group === "กลุ่ม J";
            const isSingle = currentUserGroup.isSingleDiscountType;
            const theadRow = table.querySelector('thead tr');
            
            theadRow.innerHTML = ''; // Clear existing headers

            // Add common headers
            theadRow.innerHTML += '<th>เวลา</th>';

            // Add headers based on discount type
            if (isSingle) {
                const rate = currentUserGroup.twoDigitNormalDiscount;
                theadRow.innerHTML += `<th id="singleDiscountTableHeader">ยอดซื้อส่วนลด ${rate}%</th>`;
            } else if (isGroupJ) {
                theadRow.innerHTML += `<th>ยอดเฉพาะลด ${currentUserGroup.specialDiscount}%</th>`;
                theadRow.innerHTML += `<th class="th-28-percent">ยอดซื้อส่วนลด ${currentUserGroup.twoDigitNormalDiscount}%</th>`;
                theadRow.innerHTML += `<th class="th-33-percent">ยอดซื้อส่วนลด ${currentUserGroup.threeDigitNormalDiscount}%</th>`;
            } else { // Normal discount group
                theadRow.innerHTML += `<th>ยอดเฉพาะลด ${currentUserGroup.specialDiscount}%</th>`;
                theadRow.innerHTML += `<th>ยอดซื้อส่วนลด ${currentUserGroup.twoDigitNormalDiscount}%</th>`;
            }
            
            // Add common headers
            theadRow.innerHTML += '<th>ยอดรวมทั้งหมด</th>';
            theadRow.innerHTML += '<th class="text-center">ตัวเลขที่ถูก (จำนวนซื้อ)</th>';
            theadRow.innerHTML += '<th>ยอดถูกรางวัล</th>';
            
            // Remove the 'ยอดรวมก่อนหัก %' header from the cloned table
            const grandTotalHeader = theadRow.querySelector('.hide-grand-total');
            if (grandTotalHeader) {
                grandTotalHeader.remove();
            }

            // Remove the 'ลบ' (Delete) column as it's not needed in the summary modal
            const deleteHeader = theadRow.querySelector('th:last-child');
            if (deleteHeader && deleteHeader.textContent === 'ลบ') {
                deleteHeader.remove();
            }
        }
        

        // New function to save the time-based summary from the modal
        function saveTimeBasedSummaryAsImage() {
            const modalBody = document.getElementById('timeBasedSummaryModal').querySelector('.modal-body');
            html2canvas(modalBody).then(canvas => {
                const link = document.createElement('a');
                link.download = `สรุปยอดตามเวลา_${currentUserName}.png`;
                link.href = canvas.toDataURL('image/png');
                link.click();
            });
        }
        
        function arePermutations(str1, str2) {
            if (str1.length !== str2.length) {
                return false;
            }
            const arr1 = str1.split('').sort();
            const arr2 = str2.split('').sort();
            return arr1.join('') === arr2.join('');
        }
        
        function renderWinningDetails(details) {
            if (!details || details.length === 0) {
                return '';
            }
            return details.map(d => `${d.number} (${d.type}) ซื้อ ${d.purchase}`).join('<br>');
        }
        
        initializeApp();
    </script>
</body>
</html>
