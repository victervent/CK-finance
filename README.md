<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CK motkiki</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
     <link rel="icon" type="image/x-icon" href="https://cdn11.dienmaycholon.vn/filewebdmclnew/public/userupload/files/Image%20FP_2024/hinh-anime-2.jpg">
    <style>
        :root {
            --bg: #0d1117;
            --card: #161b22;
            --border: #30363d;
            --text: #c9d1d9;
            --green: #00ff9d;
            --red: #ff0066;
            --purple: #8b00ff;
            --blue: #1f6feb;
            --orange: #ff9500;
        }
        * { margin:0; padding:0; box-sizing:border-box; }
        body { font-family: 'Segoe UI', sans-serif; background: var(--bg); color: var(--text); height: 100vh; overflow: hidden; }
        header {
            background: #010409;
            padding: 14px;
            text-align: center;
            font-size: 19px;
            font-weight: bold;
            border-bottom: 1px solid var(--border);
            color: #58a6ff;
            text-shadow: 0 0 10px #58a6ff;
            letter-spacing: 1px;
            position: relative;
        }
        .reset-btn {
            position: absolute;
            left: 16px;
            top: 50%;
            transform: translateY(-50%);
            padding: 10px 20px;
            background: var(--orange);
            color: white;
            border: none;
            border-radius: 10px;
            font-weight: bold;
            cursor: pointer;
            font-size: 14px;
            z-index: 100;
            box-shadow: 0 4px 15px rgba(255,149,0,0.4);
            transition: all 0.3s;
        }
        .reset-btn:hover { transform: translateY(-50%) scale(1.05); box-shadow: 0 8px 25px rgba(255,149,0,0.6); }

        .container {
            display: grid;
            grid-template-columns: 1fr 420px;
            height: calc(100vh - 58px);
            gap: 1px;
            background: var(--border);
        }
        .chart-area { background: var(--card); padding: 16px; position: relative; }
        .sidebar { background: var(--card); padding: 18px; overflow-y: auto; display: flex; flex-direction: column; gap: 16px; }

        .price-box {
            background: linear-gradient(135deg, #1f6feb, #58a6ff);
            padding: 22px;
            border-radius: 16px;
            text-align: center;
            font-size: 46px;
            font-weight: bold;
            color: white;
            box-shadow: 0 8px 30px rgba(31,111,235,0.5);
        }
        .price-change { font-size: 19px; margin-top: 6px; font-weight: normal; }
        .up { color: var(--green); text-shadow: 0 0 15px var(--green); }
        .down { color: var(--red); text-shadow: 0 0 15px var(--red); }

        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
        .stat { background: #1f2329; padding: 14px; border-radius: 12px; border: 1px solid var(--border); }
        .stat-label { color: #8b949e; font-size: 13px; }
        .stat-value { font-weight: bold; font-size: 17px; margin-top: 4px; }

        .risk-bar { height: 12px; background: #333; border-radius: 6px; overflow: hidden; margin: 14px 0; border: 2px solid #ff3300; box-shadow: 0 0 15px rgba(255,51,0,0.4); }
        .risk-fill { height: 100%; width: 0%; background: linear-gradient(90deg, #ff6600, #ff0066); transition: width 0.9s ease; box-shadow: 0 0 20px #ff0066; }

        input { width: 100%; padding: 15px; background: #0d1117; border: 1px solid var(--border); border-radius: 12px; color: white; font-size: 16px; }
        .btn-group { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; }
        button { padding: 15px; border: none; border-radius: 12px; font-weight: bold; cursor: pointer; transition: all 0.3s; font-size: 15px; }
        .btn-buy { background: var(--green); color: black; }
        .btn-sell { background: var(--red); color: white; }
        .btn-all { background: #0066ff; color: white; }
        .btn-manip { background: var(--purple); color: white; position: relative; font-size: 13.5px; padding: 11px; text-align: left; }
        .btn-manip:hover { transform: translateY(-3px); box-shadow: 0 10px 25px rgba(139,0,255,0.5); }
        .btn-manip::after { content: attr(data-cost); position: absolute; top: 5px; right: 10px; font-size: 10px; background: rgba(0,0,0,0.7); padding: 3px 7px; border-radius: 5px; }

        .manip-section { background: linear-gradient(135deg, #1a0033, #2d004d); padding: 18px; border-radius: 14px; border: 2px solid #ff00ff40; }
        .manip-title { text-align: center; color: #ff00ff; font-size: 18px; font-weight: bold; margin-bottom: 14px; text-shadow: 0 0 12px #ff00ff; }
        .manip-grid { display: grid; gap: 9px; }

        .history { flex: 1; background: #0d1117; border-radius: 12px; padding: 14px; font-size: 13.5px; overflow-y: auto; border: 1px solid var(--border); min-height: 200px; }
        .history p { padding: 9px; background: #1f2329; border-radius: 8px; margin: 5px 0; }
        .event-auto { color: #888; }
        .event-success { background: #006400 !important; color: var(--green); font-weight: bold; }
        .event-fail { background: #8b0000 !important; color: #ff6666; font-weight: bold; animation: blink 0.6s infinite; }
        .event-busted { background: #ff0000 !important; color: white; font-size: 20px; text-align: center; animation: blink 0.3s infinite; }

        #playerTitle {
            text-align: center;
            font-size: 20px;
            font-weight: bold;
            margin: 12px 0 8px;
            padding: 12px;
            border-radius: 12px;
            background: linear-gradient(90deg, #1a0033, #2d004d);
            border: 2px solid #ff00ff60;
            text-shadow: 0 0 15px;
            animation: glow 2s infinite alternate;
        }

        @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0.4} }
        @keyframes glow { from { box-shadow: 0 0 15px #ff00ff40; } to { box-shadow: 0 0 35px #ff00ff90; } }
        canvas { border-radius: 14px; background: var(--card); }

        @media (max-width: 1024px) {
            .container { grid-template-columns: 1fr; }
            .sidebar { max-height: 50vh; }
            .reset-btn { position: static; transform: none; margin: 10px auto; display: block; width: fit-content; }
        }
    </style>
</head>
<body>
    <header>
        VCB REAL TRADER - By motkiki
        <button class="reset-btn" onclick="resetGame()">RESET GAME</button>
        <div style="position:absolute; top:12px; right:16px; z-index:999;">
            
    <button onclick="openBank()" 
            style="background:linear-gradient(90deg,#00ff9d,#00cc7a); 
                   color:black; 
                   font-weight:bold; 
                   padding:10px 20px; 
                   border:none; 
                   border-radius:12px; 
                   box-shadow:0 0 20px #00ff9d88, 0 4px 15px rgba(0,0,0,0.4);
                   cursor:pointer; 
                   font-size:15px;
                   transition:all 0.3s;">
        TIẾT KIỆM
    </button>
</div>
    </header>
    <!-- POPUP TIẾT KIỆM -->
<div id="bankModal" style="display:none; position:fixed; top:50%; left:50%; transform:translate(-50%,-50%); width:340px; max-width:90%; background:#0d1117; border:3px solid #00ff9d; border-radius:20px; padding:22px; z-index:10000; box-shadow:0 0 40px #00ff9d99;">
    <div style="text-align:center; color:#00ff9d; font-size:19px; font-weight:bold; margin-bottom:15px; text-shadow:0 0 10px #00ff9d;">
        GỬI TIẾT KIỆM VCB – LÃI SIÊU CAO
    </div>
    <input type="number" id="depositAmount" placeholder="Nhập số tiền (triệu)" style="width:100%; padding:12px; margin-bottom:15px; background:#161b22; border:2px solid #30363d; color:white; border-radius:12px; text-align:center; font-size:15px;">
    <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-bottom:15px;">
        <button onclick="depositMoney(30, 0.01)"  style="background:#00ff9d; color:black; padding:12px; border:none; border-radius:12px; font-weight:bold;">30s +1%</button>
        <button onclick="depositMoney(60, 0.02)"  style="background:#39ff14; color:black; padding:12px; border:none; border-radius:12px; font-weight:bold;">60s +2%</button>
        <button onclick="depositMoney(90, 0.03)"  style="background:#ffd700; color:black; padding:12px; border:none; border-radius:12px; font-weight:bold;">90s +3%</button>
        <button onclick="depositMoney(120, 0.04)" style="background:#ff9500; color:black; padding:12px; border:none; border-radius:12px; font-weight:bold;">2p +4%</button>
    </div>
    <div id="depositList" style="background:#161b22; padding:12px; border-radius:12px; max-height:200px; overflow-y:auto; margin-bottom:12px; color:#c9d1d9; font-size:14px;"></div>
    <button onclick="closeBank()" style="width:100%; padding:12px; background:#30363d; color:white; border:none; border-radius:12px; font-weight:bold;">ĐÓNG</button>
</div>
<div id="overlay" onclick="closeBank()" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.85); z-index:9999;"></div>
    <div class="container">
        <div class="chart-area">
            <canvas id="chart"></canvas>
        </div>

        <div class="sidebar">
            <div class="price-box" id="priceBox">90,000 ₫ <div class="price-change">0 (0.00%)</div></div>

            <div class="stats-grid">
                <div class="stat"><div class="stat-label">Vốn khả dụng</div><div class="stat-value" id="capital">50,000,000 ₫</div></div>
                <div class="stat"><div class="stat-label">Tổng tài sản</div><div class="stat-value" id="total">50,000,000 ₫</div></div>
                <div class="stat"><div class="stat-label">Sở hữu VCB</div><div class="stat-value" id="shares">0</div></div>
                <div class="stat"><div class="stat-label">Lãi/Lỗ</div><div class="stat-value" id="profit">0 ₫</div></div>
            </div>

            <div class="risk-bar"><div class="risk-fill" id="legalFill"></div></div>
            <div style="text-align:center; color:#ff3300; font-weight:bold; font-size:15px;">
                Mức nghi ngờ pháp lý: <span id="legalLevel">0</span>%
            </div>

            <input type="number" id="qty" placeholder="Số lượng cổ phiếu" min="1">

            <div class="btn-group">
                <button class="btn-buy" onclick="buy()">MUA NGAY</button>
                <button class="btn-sell" onclick="sell()">BÁN NGAY</button>
                <button class="btn-all" onclick="buyAll()">MUA HẾT TIỀN</button>
                <button class="btn-all" onclick="sellAll()">BÁN HẾT CỔ</button>
                
            </div>

            <div class="manip-section">
                <div class="manip-title">THAO TÚNG THỊ TRƯỜNG (80% THÀNH CÔNG)</div>
                <div class="manip-grid">
                    <button class="btn-manip" data-cost="45tr" onclick="playerManip(0)">Tin đồn VCB chia cổ tức 35%</button>
                    <button class="btn-manip" data-cost="50tr" onclick="playerManip(1)">Chủ tịch VCB bị bắt khẩn cấp!</button>
                    <button class="btn-manip" data-cost="120tr" onclick="playerManip(2)">Phanh phui rửa tiền 100.000 tỷ</button>
                    <button class="btn-manip" data-cost="160tr" onclick="playerManip(3)">Nhà nước bơm 200.000 tỷ vào VCB!</button>
                    <button class="btn-manip" data-cost="240tr" onclick="playerManip(4)">VCB trúng siêu dự án 500.000 tỷ</button>
                    <button class="btn-manip" data-cost="380tr" onclick="playerManip(5)">VCB lên sàn NYSE – FOMO toàn cầu</button>
                    <button class="btn-manip" data-cost="580tr" onclick="playerManip(6)">VCB lãi kỷ lục 1 triệu tỷ!</button>
                    <button class="btn-manip" data-cost="40tr" onclick="playerManip(7)">Tỷ phú âm thầm gom 15% cổ phần</button>
                    <button class="btn-manip" data-cost="90tr" onclick="playerManip(8)">Quỹ ngoại xả 30% cổ phần VCB</button>
                    <button class="btn-manip" data-cost="65tr" onclick="playerManip(9)">Hacker tấn công hệ thống VCB</button>
                    <button class="btn-manip" data-cost="110tr" onclick="playerManip(10)">VCB bị kiểm toán đột xuất</button>
                    <button class="btn-manip" data-cost="220tr" onclick="playerManip(11)">VCB được chọn làm ngân hàng quốc gia</button>
                    <button class="btn-manip" data-cost="140tr" onclick="playerManip(12)">Cổ đông lớn bán tháo trong đêm</button>
                   
                </div>
            </div>

            <div class="history" id="history">
                <p>Đại gia đã sẵn sàng thao túng VCB với 80% thành công!</p>
            </div>
        </div>
    </div>

<script>
    // ================== BIẾN TOÀN CỤC ==================
    let price = 90000, lastPrice = price;
    let shares = 0, capital = 60000000, totalStart = capital;
    let legalRisk = 0, ongoingEffect = 0, effectEndTime = 0;
    let currentVolumeBoost = 0;
    let currentTitleIndex = -1;
    let lastNaturalEventTime = 0;
    let lastLuckyTime = Date.now();
    let lastBadEventTime = Date.now();          
    let pendingNaturalEvent = null;

    let prices = [], volumes = [], ema20 = [], chart;
    let historyLog = [];

    // Biến animation mượt thanh Risk
    let displayedRisk = 0;
    const riskFill = document.getElementById('legalFill');
    const riskText = document.getElementById('legalLevel');

    // ================== DỮ LIỆU ==================
    const badEvents = [
        { text: "Nhà nước đánh thuế đột xuất cổ phiếu → Nộp ngay 8 triệu", cash: 8000000, risk: 20 },
        { text: "Hacker rút trộm tiền trong ví → Mất 15 triệu", cash: 15000000, risk: 12 },
        { text: "Công an mời lên uống nước → Nộp phạt 10 triệu", cash: 10000000, risk: 18 },
        { text: "Thuế truy thu lợi nhuận → Trừ ngay 30 triệu", cash: 30000000, risk: 15 },
        { text: "Nhóm Telegram bị sờ gáy → Mất 200 cổ tạm giữ", shares: 200, risk: 20 },
        { text: "Bạn bị lộ danh tính → Phải bán tháo gấp, mất 100 cổ", shares: 100, risk: 25 },
        { text: "Ngân hàng khóa tài khoản tạm thời → Rút phạt 12 triệu", cash: 12000000, risk: 10 },
        { text: "Tin đồn tiêu cực trên VnExpress → Mất 250 cổ do hoảng loạn", shares: 250, risk: 22 },
        { text: "Chuyển khoản nhầm cho công an → Mất 20 triệu", cash: 20000000, risk: 30 },
        { text: "Bị bạn thân lừa đảo → Mất 180 cổ VCB", shares: 180, risk: 28 },
         { text: "Bị nghi ngờ là người da đen ",  risk: 50 }
    ];

    function triggerBadEvent() {
        const now = Date.now();
        // Chỉ xảy ra sau 5–7 phút
        if (now - lastBadEventTime < 300000 + Math.random() * 120000) return; // 5–7 phút
        if (Math.random() > 0.55) return; // ~55% cơ hội khi đủ thời gian

        const ev = badEvents[Math.floor(Math.random() * badEvents.length)];
        lastBadEventTime = now;

        // CẢNH BÁO TRƯỚC 30 GIÂY
        addLog(`<p class="event-fail" style="font-size:18px; animation:blink 0.6s infinite; background:#440000;">
                CẢNH BÁO: SỰ KIỆN TIÊU CỰC SẮP XẢY RA SAU 30 GIÂY!</p>`);

        setTimeout(() => {
            let log = `<p class="event-busted" style="font-size:19px; background:#8b0000; color:white;">
                       SỰ KIỆN XẤU<br>${ev.text}</p>`;

            if (ev.cash) {
                if (capital >= ev.cash) {
                    capital -= ev.cash;
                    log += `<br>→ Trừ ${ (ev.cash/1000000).toFixed(0) } triệu vốn khả dụng`;
                } else {
                    capital = 1000000; // còn 1 triệu sống qua ngày
                    log += `<br>→ Hết tiền! Chỉ còn 1 triệu sống sót...`;
                }
            }
            if (ev.shares) {
                const lost = Math.min(shares, ev.shares);
                shares -= lost;
                log += `<br>→ Mất ${lost.toLocaleString()} cổ VCB`;
            }

            legalRisk = Math.min(100, legalRisk + ev.risk);
            addLog(log);
            updateLegalBar();
            updateStats();
        }, 30000); // đúng 30 giây sau cảnh báo
    }
    const naturalEvents = [
        { text: "Lãi suất tăng 1%", change: -0.08 },
        { text: "Ngân hàng Nhà nước bơm tiền", change: 0.06 },
        { text: "Đại gia NMN mua nhiều cổ phiếu", change: 0.12 },
        { text: "Khối ngoại mua ròng mạnh", change: 0.06 },
        { text: "VCB được nâng hạng tín nhiệm", change: 0.06 },
        { text: "Tin đồn kiểm toán đặc biệt", change: -0.07 },
        { text: "Chủ tịch VCB phát biểu tích cực", change: 0.05 },
        { text: "VCB trúng thầu lớn", change: 0.09 },
        { text: "Lạm phát tăng cao", change: -0.04 },
        { text: "Chứng khoán toàn cầu đỏ lửa", change: -0.09 },
        { text: "VCB phát hành thêm cổ phiếu", change: -0.03 },
        { text: "Cổ đông lớn thoái vốn", change: -0.02 },
        { text: "FOMO cực mạnh trên các nhóm", change: 0.06 },
        { text: "Margin call lan rộng", change: -0.05 }
    ];

    const playerEvents = [
        {text: 'Tin đồn VCB chia cổ tức 35%', change: 0.12, cost: 45000000, risk: 20},
        {text: 'Chủ tịch VCB bị bắt khẩn cấp!', change: -0.14, cost: 50000000, risk: 18},
        {text: 'Phanh phui rửa tiền 100.000 tỷ!', change: -0.18, cost: 120000000, risk: 30},
        {text: 'Nhà nước bơm 200.000 tỷ vào VCB!', change: 0.19, cost: 160000000, risk: 24},
        {text: 'VCB trúng siêu dự án 500.000 tỷ', change: 0.24, cost: 240000000, risk: 28},
        {text: 'VCB lên sàn NYSE – FOMO toàn cầu', change: 0.30, cost: 380000000, risk: 38},
        {text: 'VCB lãi kỷ lục 1 triệu tỷ!', change: 0.40, cost: 580000000, risk: 50},
        {text: 'Tỷ phú gom ngầm 15% cổ phần', change: 0.19, cost: 40000000, risk: 12},
        {text: 'Quỹ ngoại xả 30% cổ phần', change: -0.16, cost: 90000000, risk: 20},
        {text: 'Hacker tấn công hệ thống VCB', change: -0.15, cost: 65000000, risk: 18},
        {text: 'VCB bị kiểm toán đột xuất', change: -0.21, cost: 110000000, risk: 25},
        {text: 'VCB được chọn làm ngân hàng quốc gia', change: 0.22, cost: 220000000, risk: 26},
        {text: 'Cổ đông lớn bán tháo trong đêm', change: -0.23, cost: 140000000, risk: 28},
   
    
    ];

    const titles = [
        { min: 300000000000, title: "NIGGEST", color: "#FFFF00	" },
         { min: 200000000000, title: "ĐỈNH CAO MUÔN LOÀI", color: "#FF0000" },
        { min: 100000000000, title: "THẦN TÀI SỐNG", color: "#ff00ff" },
        { min: 50000000000, title: "ELON MUP", color: "#ffd700" },
        { min: 20000000000, title: "TỶ PHÚ TOP 1 THẾ GIỚI", color: "#ff00ff" },
        { min: 10000000000, title: "TỶ PHÚ TOP 10 THẾ GIỚI", color: "#ff9500" },
        { min: 5000000000, title: "TỶ PHÚ TOP 1 VIỆT NAM", color: "#ffd700" },
        { min: 3000000000, title: "TỶ PHÚ VIỆT NAM", color: "#ff00ff" },
        { min: 1000000000, title: "TỶ PHÚ MỚI NỔI", color: "#ff9500" },
        { min: 500000000, title: "CÁ MẬP", color: "#00ff9d" },
        { min: 300000000, title: "ĐẠI GIA SIÊU CẤP", color: "#58a6ff" },
        { min: 200000000, title: "ĐẠI GIA", color: "#8b00ff" },
        { min: 100000000, title: "TƯ DUY TUYỂN THỦ", color: "#39ff14" },
        { min: 80000000, title: "NHÀ ĐẦU TƯ", color: "#c9d1d9" }
    ];

    const luckyEvents = [
        { text: "Nhà nước hỗ trợ dân 5 triệu", cash: 5000000 },
        { text: "Tài xỉu + 36 triệu", cash: 36000000 },
        { text: "2 ngón + 18 triệu", cash: 18000000 },
        { text: "Chuyển nhầm tiền +17 triệu", cash: 17000000 },
        { text: "Bố mẹ cho thêm tiền +15 triệu", cash: 15000000 },
        { text: "Trúng thưởng chương trình +12 triệu", cash: 12000000 },
        { text: "Hoàn thuế thu nhập +10 triệu", cash: 10000000 },
        { text: "Trúng vé số giải phụ +20 triệu", cash: 20000000 },
        { text: "Chủ tịch VCB tặng +100 cổ", shares: 100 },
        { text: "Cổ đông lớn chuyển nhầm +150 cổ", shares: 150 },
        { text: "Trúng giải đặc biệt 10 triệu 80 cổ", cash: 10000000, shares: 80 },
        { text: "Tỷ phú ẩn danh hỗ trợ +15 triệu + 50 cổ!", cash: 15000000, shares: 50 },
        { text: "Đêm qua em đẹp lắm", cash: 10000000, shares: 100 },
        { text: "VCB chia tách 2:1 – Cổ nhân đôi!", shares: "double" },
    ];

    // ================== DỮ LIỆU LỊCH SỬ + EMA ==================
    let temp = 90000;
    for(let i = 0; i < 299; i++){
        temp *= (1 + (Math.random()-0.5)*0.04);
        temp = Math.max(70000, Math.min(120000, Math.round(temp/10)*10));
        prices.push(temp);
        volumes.push(Math.floor(Math.random()*40000000 + 40000000));
    }
    prices.push(90000);
    volumes.push(80000000);

    function calculateEMA(data, period){
        const k = 2/(period+1);
        let ema = data[0];
        const res = [ema];
        for(let i=1;i<data.length;i++){
            ema = data[i]*k + ema*(1-k);
            res.push(Math.round(ema));
        }
        return res;
    }
    ema20 = calculateEMA(volumes, 20);

    // ================== CHART ==================
    const ctx = document.getElementById('chart').getContext('2d');
    chart = new Chart(ctx, {
        type: 'line',
        data: {
            labels: Array(300).fill(''),
            datasets: [
                { label: 'VCB', data: prices, borderColor: '#00ff9d', backgroundColor: 'rgba(0,255,157,0.12)', borderWidth: 3, tension: 0.4, pointRadius: 0, fill: true, yAxisID: 'price' },
                { label: 'Volume', type: 'bar', data: volumes, backgroundColor: ctx => {
                    const prev = prices[ctx.dataIndex-1] || prices[0];
                    return prices[ctx.dataIndex] >= prev ? 'rgba(0,255,157,0.35)' : 'rgba(255,0,102,0.35)';
                }, borderRadius: 2, maxBarThickness: 8, barPercentage: 0.8, categoryPercentage: 0.9, yAxisID: 'volume' },
                { label: 'EMA20', data: ema20, borderColor: '#39ff14', borderWidth: 2, type: 'line', yAxisID: 'volume', pointRadius: 0, tension: 0.4, borderDash: [5, 5] }
            ]
        },
        options: {
            animation: { duration: 300 },
            maintainAspectRatio: false,
            interaction: { mode: 'index', intersect: false },
            scales: {
                x: { display: false, grid: { display: false } },
                price: { position: 'left', grid: { color: '#30363d', lineWidth: 0.5 }, ticks: { color: '#c9d1d9', font: { size: window.innerWidth <= 768 ? 11 : 12 }, callback: v => (v/1000)+'k' } },
                volume: { position: 'right', min: 0, max: 250000000, grid: { drawOnChartArea: false }, ticks: { color: '#666', font: { size: window.innerWidth <= 768 ? 9 : 10 }, callback: v => (v/1000000)+'M', maxTicksLimit: 5 } }
            },
            plugins: { 
                legend: { display: false },
                tooltip: {
                    backgroundColor: 'rgba(13,17,23,0.95)',
                    titleFont: { size: 13 },
                    bodyFont: { size: 12 },
                    callbacks: {
                        label: function(context) {
                            if (context.dataset.label === 'VCB') return ' Giá: ' + context.parsed.y.toLocaleString() + ' ₫';
                            if (context.dataset.label === 'Volume') return ' KL: ' + (context.parsed.y/1000000).toFixed(1) + 'M';
                            return context.dataset.label + ': ' + context.parsed.y;
                        }
                    }
                }
            }
        }
    });

    window.addEventListener('resize', () => {
        chart.options.scales.price.ticks.font.size = window.innerWidth <= 768 ? 11 : 12;
        chart.options.scales.volume.ticks.font.size = window.innerWidth <= 768 ? 9 : 10;
        chart.options.scales.volume.max = window.innerWidth <= 768 ? 200000000 : 300000000;
        chart.update('quiet');
    });

    function updateChartScales(){
        const maxP = Math.max(...prices), minP = Math.min(...prices);
        const pad = (maxP-minP)*0.25 || maxP*0.25;
        chart.options.scales.price.min = Math.floor((minP-pad)/10000)*10000;
        chart.options.scales.price.max = Math.ceil((maxP+pad)/10000)*10000;
        chart.update('quiet');
    }

    function updateEMA(){
        if(volumes.length < 20) return;
        const k = 2/21;
        const last = ema20[ema20.length-1];
        ema20.push(Math.round(volumes.at(-1)*k + last*(1-k)));
    }

    // ================== MUA BÁN ==================
    const buy = () => {
        const q = +document.getElementById('qty').value || 0;
        if(q < 1) return alert('Nhập số lượng!');
        const cost = price * q * 1.0015;
        if(cost > capital) return alert('Hết tiền!');
        capital -= cost; shares += q; currentVolumeBoost += q*1000;
        addLog(`MUA ${q.toLocaleString()} cổ @ ${price.toLocaleString()}₫`);
        impactFromTrade(q, true); updateStats();
    };

    const sell = () => {
        const q = +document.getElementById('qty').value || 0;
        if(q > shares || q < 1) return alert('Không đủ cổ!');
        capital += price * q * 0.9985; shares -= q; currentVolumeBoost += q*1000;
        const reduce = Math.floor(q/10);
        if(reduce > 0){
            legalRisk = Math.max(0, legalRisk - reduce);
            addLog(`BÁN ${q.toLocaleString()} cổ @ ${price.toLocaleString()}₫ → Giảm ${reduce}% nghi ngờ`);
        } else addLog(`BÁN ${q.toLocaleString()} cổ @ ${price.toLocaleString()}₫`);
        impactFromTrade(q, false); 
        updateLegalBar(); // cập nhật ngay khi bán giảm
        updateStats();
    };

    const buyAll = () => { const q = Math.floor(capital/(price*1.0015)); if(q<1) return alert('Hết tiền!'); document.getElementById('qty').value=q; buy(); };
    const sellAll = () => { if(shares<1) return alert('Không có gì bán!'); document.getElementById('qty').value=shares; sell(); };

    function impactFromTrade(qty, isBuy){
        if(isBuy && qty >= 10000){
            const pump = Math.min(qty/60000,1)*0.10;
            price *= (1+pump); price = Math.round(price/10)*10;
            addLog(`<span style="color:#00ff9d;font-weight:bold;">CÁ MẬP GOM MẠNH! FOMO bùng nổ!</span>`);
        }
        if(!isBuy){
            let drop = 0, msg = "";
            if(qty >= 80000) { drop = 0.35; msg = "CÁ MẬP XẢ HÀNG KHỦNG! VCB lao dốc!"; }
            else if(qty >= 60000) { drop = 0.25; msg = "Đại gia bán tháo – Hoảng loạn!"; }
            else if(qty >= 40000) { drop = 0.2; msg = "Đại gia bán tháo – Hoảng loạn!"; }
            else if(qty >= 25000) { drop = 0.15; msg = "Đại gia bán tháo – Hoảng loạn!"; }
            else if(qty >= 10000) { drop = 0.09; msg = "Cá mập xả 10k+ cổ – Rung lắc mạnh!"; }
             else if(qty >= 5000) { drop = 0.07; msg = "Tay to bán 5k+ cổ – Áp lực lớn!"; }
            else if(qty >= 2000) { drop = 0.05; msg = "Tay to bán 2k+ cổ – Áp lực lớn!"; }
            else if(qty >= 1000) { drop = 0.03; msg = "Chốt lời xuất hiện"; }
            if(drop>0){
                price *= (1-drop); price = Math.max(30000, Math.round(price/10)*10);
                addLog(`<span style="color:#ff0066;font-weight:bold;">${msg}</span>`);
            }
        }
    }

    
    // ================== SỰ KIỆN TỰ NHIÊN ==================
    function triggerNaturalEvent() {
        const now = Date.now();
        if (pendingNaturalEvent || now - lastNaturalEventTime < 20000) return;
        if (Math.random() > 0.28) return;

        const ev = naturalEvents[Math.floor(Math.random() * naturalEvents.length)];
        pendingNaturalEvent = ev;
        lastNaturalEventTime = now;

        addLog(`<span class="event-auto">SẮP XẢY RA TRONG 10 GIÂY: ${ev.text}</span>`);

        setTimeout(() => {
            if (!pendingNaturalEvent) return;
            const variance = 0.8 + Math.random() * 0.4;
            const changePercent = ev.change * variance;
            price *= (1 + changePercent);
            price = Math.max(30000, Math.round(price / 10) * 10);

            const sign = changePercent > 0 ? "+" : "";
            const color = changePercent > 0 ? "#00ff9d" : "#ff0066";

            addLog(`<span style="color:${color};font-weight:bold;">
                   ĐÃ XẢY RA: ${ev.text} → Giá ${sign}${(changePercent*100).toFixed(1)}%
                   </span>`);

            pushToChart();
            updateStats();
            pendingNaturalEvent = null;
        }, 10000);
    }

    // ================== CẬP NHẬT GIÁ ==================
    function updatePrice(){
        lastPrice = price; currentVolumeBoost = 0;

        let change = (Math.random()-0.5)*0.045 + Math.sin(Date.now()/180000)*0.012;
        if(Date.now() < effectEndTime) change += ongoingEffect;
        else ongoingEffect = 0;
        price *= (1 + change);
        price = Math.max(30000, Math.round(price/10)*10);

        pushToChart();

        const diff = price - lastPrice;
        const pct = ((diff/lastPrice)*100).toFixed(2);
        document.getElementById('priceBox').innerHTML = 
            `${price.toLocaleString()}₫ <div class="price-change ${diff>=0?'up':'down'}">
            ${diff>0?'+' : ''}${diff.toLocaleString()} (${pct}%)</div>`;
        updateStats();
    }

    function pushToChart(){
        prices.push(price);
        volumes.push(Math.random()*90000000 + 30000000 + currentVolumeBoost);
        updateEMA();
        if(prices.length > 500){
            prices.shift(); volumes.shift(); ema20.shift();
            chart.data.labels.shift();
        }
        chart.data.labels.push('');
        chart.data.datasets[0].data = prices;
        chart.data.datasets[1].data = volumes;
        chart.data.datasets[2].data = ema20.slice(-prices.length);
        updateChartScales();
        chart.update('none');
    }

    // ================== DANH HIỆU ==================
    function updateTitle(){
        const total = capital + shares*price;
        let newIdx = titles.length-1;
        for(let i=0;i<titles.length;i++){
            if(total >= titles[i].min){ newIdx = i; break; }
        }
        if(newIdx !== currentTitleIndex){
            currentTitleIndex = newIdx;
            const t = titles[newIdx];
            let el = document.getElementById('playerTitle');
            if(!el){
                el = document.createElement('div'); el.id='playerTitle';
                document.querySelector('.price-box').after(el);
            }
            el.innerHTML = `<i class="fas fa-crown" style="margin-right:8px"></i>${t.title}`;
            el.style.color = t.color;
            if(total >= 100000000){
                addLog(`<span style="color:${t.color};font-size:18px;font-weight:bold;">
                    CHÚC MỪNG! BẠN ĐÃ LÊN HẠNG:<br>→ ${t.title} ←</span>`);
            }
        }
    }

    // ================== SỰ KIỆN MAY MẮN ==================
    function triggerLuckyEvent(){
        const now = Date.now();
        if(now - lastLuckyTime < 60000) return;
        if(Math.random() > 0.045) return;

        const ev = luckyEvents[Math.floor(Math.random()*luckyEvents.length)];
        let log = `<span style="color:#ffd700;font-weight:bold;font-size:16px;">SỰ KIỆN TRỌNG ĐẠI<br>${ev.text}`;
        if(ev.shares === "double"){
            shares = Math.floor(shares*2);
            log += "<br>→ Số cổ VCB đã được nhân đôi!";
        }else{
            capital += ev.cash || 0;
            shares += ev.shares || 0;
            if(ev.cash) log += `<br>→ +${(ev.cash/1000000).toLocaleString()} triệu vốn`;
            if(ev.shares) log += `<br>→ +${ev.shares.toLocaleString()} cổ VCB`;
        }
        log += "</span>";
        addLog(log);
        updateStats();
        lastLuckyTime = now;
    }

    // ================== THAO TÚNG ==================
    function playerManip(i){
        const ev = playerEvents[i];
        if(capital < ev.cost) return alert(`Không đủ tiền! Cần ${ev.cost.toLocaleString()}₫`);
        capital -= ev.cost;
        legalRisk += ev.risk; 
        if(legalRisk > 100) legalRisk = 100;

        const success = Math.random() < 0.8;
        if(success){
            price *= (1 + ev.change);
            ongoingEffect = ev.change > 0 ? 0.028 : -0.028;
            effectEndTime = Date.now() + 45000;
            addLog(`<span class="event-success">THAO TÚNG THÀNH CÔNG! ${ev.text}</span>`);
        }else{
            price *= (1 - ev.change*1.5);
            legalRisk += 20; 
            if(legalRisk > 100) legalRisk = 100;
            addLog(`<span class="event-fail">THAO TÚNG THẤT BẠI → Giá đi ngược mạnh! ${ev.text}</span>`);
        }
        price = Math.max(30000, Math.round(price/10)*10);
        
        updateLegalBar(); // cập nhật ngay lập tức
        checkBusted(); 
        updateStats();
    }

    // ================== THANH NGHI NGỜ PHÁP LÝ - ==================
    function updateLegalBar() {
        legalRisk = Math.max(0, Math.min(100, legalRisk));
        displayedRisk = legalRisk;
        riskFill.style.width = legalRisk + '%';
        riskText.textContent = legalRisk;
    }

    // Giảm 1% mỗi 10 giây với animation
    function startRiskDecay() {
        if (legalRisk <= 0) return;

        legalRisk = Math.max(0, legalRisk - 1);

        const startTime = Date.now();
        const startValue = displayedRisk;

        function animate() {
            const elapsed = Date.now() - startTime;
            const progress = Math.min(elapsed / 10000, 1);
            const current = Math.round(startValue + (legalRisk - startValue) * progress);

            displayedRisk = current;
            riskFill.style.width = current + '%';
            riskText.textContent = current;

            if (progress < 1) {
                requestAnimationFrame(animate);
            }
        }
        requestAnimationFrame(animate);
    }

    setInterval(() => {
        if (legalRisk > 0) {
            startRiskDecay();
        }
    }, 10000);

    function checkBusted() {
        if (legalRisk >= 100) {
            addLog(`<p class="event-busted">BẠN ĐÃ BỊ CÔNG AN BẮT! GAME OVER!</p>`);
            setTimeout(() => alert("Bạn đã bị bắt vì thao túng quá đà!"), 500);
            document.querySelectorAll('button').forEach(b => b.disabled = true);
        }
    }

    // ================== CẬP NHẬT THỐNG KÊ ==================
    function updateStats(){
        const total = capital + shares*price;
        const profit = total - totalStart;
        document.getElementById('capital').textContent = capital.toLocaleString() + ' ₫';
        document.getElementById('total').textContent = total.toLocaleString() + ' ₫';
        document.getElementById('shares').textContent = shares.toLocaleString();
        document.getElementById('profit').innerHTML = profit >= 0 ?
            `<span style="color:#00ff9d">+${profit.toLocaleString()} ₫</span>` :
            `<span style="color:#ff0066">${profit.toLocaleString()} ₫</span>`;
        updateTitle();
    }

    function addLog(m){
        const t = new Date().toLocaleTimeString('vi-VN',{hour:'2-digit',minute:'2-digit',second:'2-digit'});
        historyLog.unshift(`<p>[${t}] ${m}</p>`);
        if(historyLog.length > 30) historyLog.pop();
        document.getElementById('history').innerHTML = historyLog.join('');
        document.getElementById('history').scrollTop = 0;
    }

    function resetGame(){ if(confirm('Chơi lại từ đầu?')) location.reload(); }
let deposits = []; // {amount, interest, endTime}

function openBank() {
    document.getElementById('bankModal').style.display = 'block';
    document.getElementById('overlay').style.display = 'block';
    updateDepositList();
}

function closeBank() {
    document.getElementById('bankModal').style.display = 'none';
    document.getElementById('overlay').style.display = 'none';
}

// Gửi tiền tiết kiệm
function depositMoney(seconds, rate) {
    const input = document.getElementById('depositAmount');
    let amount = Math.floor((parseFloat(input.value) || 0) * 1000000);
    
    if (amount < 1000000) return alert("Nhập ít nhất 1 triệu!");
    if (amount > capital) return alert("Không đủ vốn khả dụng!");

    capital -= amount;
    const interest = amount * rate;
    const endTime = Date.now() + seconds * 1000;

    deposits.push({ amount, interest, endTime, seconds, rate });
    input.value = '';
    updateStats();
    addLog(`Gửi tiết kiệm ${(amount/1000000).toFixed(0)} triệu – ${seconds}s lãi ${rate*100}%`);
    updateDepositList();
}

// Cập nhật danh sách + đồng hồ đếm ngược
function updateDepositList() {
    const list = document.getElementById('depositList');
    if (deposits.length === 0) {
        list.innerHTML = '<div style="text-align:center;color:#666;padding:15px;">Chưa có gói tiết kiệm nào</div>';
        return;
    }

    list.innerHTML = deposits.map((d, i) => {
        const remain = Math.max(0, Math.ceil((d.endTime - Date.now()) / 1000));
        const isDone = remain <= 0;

        return `
            <div style="background:#1a2332;padding:12px;border-radius:10px;margin:8px 0;text-align:center;">
                <div style="color:#00ff9d;font-weight:bold;font-size:15px;">
                    ${(d.amount/1000000).toFixed(0)} triệu → +${(d.interest/1000000).toFixed(1)} triệu lãi
                </div>
                <div style="color:${isDone?'#ffd700':'#ff9500'};margin:8px 0;">
                    ${isDone ? 'ĐÃ HOÀN TẤT!' : `Còn ${remain} giây`}
                </div>
                ${isDone ? 
                    `<button onclick="withdrawDeposit(${i})" 
                             style="width:100%;background:#ffd700;color:black;border:none;padding:10px;border-radius:8px;font-weight:bold;">
                        RÚT VỐN + LÃI
                     </button>` : ''
                }
            </div>`;
    }).join('');
}

// Rút tiền khi hết giờ
function withdrawDeposit(index) {
    const d = deposits[index];
    if (!d || Date.now() < d.endTime) return;

    capital += d.amount + d.interest;
    deposits.splice(index, 1);
    addLog(`RÚT TIẾT KIỆM → +${(d.amount/1000000).toFixed(0)}tr vốn + ${(d.interest/1000000).toFixed(1)}tr lãi!`);
    updateStats();
    updateDepositList();
}

// Tự động cập nhật đồng hồ mỗi 0.5s
setInterval(() => {
    if (deposits.length > 0) updateDepositList();
}, 500);    
    // ================== KHỞI ĐỘNG ==================
    updateChartScales();
    updateStats();
    updateLegalBar();
    addLog('Đại gia đã sẵn sàng thao túng VCB với 80% thành công!');

    setInterval(updatePrice, 2000);
    setInterval(triggerNaturalEvent, 2000);
    setInterval(triggerLuckyEvent, 8000);
    setInterval(triggerBadEvent, 15000);                           // Bad Event mỗi ~5–7 phút + cảnh báo 30s
    setInterval(() => { if (legalRisk > 0) startRiskDecay(); }, 10000); // Giảm risk mượt mỗi 10s
</script>
</body>
</html>
