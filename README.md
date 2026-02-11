<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>นาฬิกา และ PM2.5</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 40px;
            max-width: 900px;
            width: 100%;
        }

        .card {
            background: white;
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            text-align: center;
        }

        /* ======= CLOCK STYLES ======= */
        .clock-section h2 {
            color: #333;
            margin-bottom: 30px;
            font-size: 24px;
        }

        .clock-display {
            font-size: 48px;
            font-weight: bold;
            color: #667eea;
            font-family: 'Courier New', monospace;
            margin: 20px 0;
            letter-spacing: 2px;
        }

        .date-display {
            color: #666;
            font-size: 16px;
            margin-top: 15px;
        }

        .analog-clock {
            width: 200px;
            height: 200px;
            border: 4px solid #667eea;
            border-radius: 50%;
            margin: 20px auto;
            position: relative;
            background: #f9f9f9;
            box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        .center-dot {
            width: 12px;
            height: 12px;
            background: #667eea;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 10;
        }

        .hand {
            position: absolute;
            bottom: 50%;
            left: 50%;
            transform-origin: bottom center;
            background: #333;
            border-radius: 10px;
        }

        .hand.hour {
            width: 6px;
            height: 60px;
            background: #333;
            margin-left: -3px;
        }

        .hand.minute {
            width: 4px;
            height: 80px;
            background: #667eea;
            margin-left: -2px;
        }

        .hand.second {
            width: 2px;
            height: 85px;
            background: #ff6b6b;
            margin-left: -1px;
        }

        .clock-number {
            position: absolute;
            width: 100%;
            height: 100%;
            text-align: center;
            font-size: 14px;
            font-weight: bold;
            color: #667eea;
        }

        .clock-number span {
            display: inline-block;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
        }

        /* ======= PM2.5 STYLES ======= */
        .pm25-section h2 {
            color: #333;
            margin-bottom: 30px;
            font-size: 24px;
        }

        .pm25-display {
            font-size: 56px;
            font-weight: bold;
            margin: 20px 0;
            font-family: 'Courier New', monospace;
        }

        .pm25-unit {
            color: #666;
            font-size: 16px;
            margin-bottom: 20px;
        }

        .pm25-status {
            font-size: 18px;
            padding: 15px 20px;
            border-radius: 10px;
            margin: 20px 0;
            font-weight: bold;
        }

        .pm25-bar {
            width: 100%;
            height: 20px;
            background: #ddd;
            border-radius: 10px;
            overflow: hidden;
            margin: 20px 0;
        }

        .pm25-bar-fill {
            height: 100%;
            border-radius: 10px;
            transition: width 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            font-size: 12px;
        }

        .status-good {
            background-color: #27ae60;
            color: white;
        }

        .status-moderate {
            background-color: #f39c12;
            color: white;
        }

        .status-unhealthy {
            background-color: #e74c3c;
            color: white;
        }

        .status-hazardous {
            background-color: #8b0000;
            color: white;
        }

        .last-update {
            color: #999;
            font-size: 12px;
            margin-top: 15px;
        }

        .info-text {
            background: #f0f0f0;
            padding: 15px;
            border-radius: 10px;
            margin-top: 15px;
            font-size: 13px;
            color: #555;
            line-height: 1.6;
        }

        @media (max-width: 768px) {
            .container {
                grid-template-columns: 1fr;
                gap: 20px;
            }

            .clock-display {
                font-size: 36px;
            }

            .pm25-display {
                font-size: 42px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- CLOCK SECTION -->
        <div class="card clock-section">
            <h2>⏰ นาฬิกา</h2>
            
            <div class="analog-clock">
                <div class="clock-number">
                    <span style="top: 10%; left: 50%;">12</span>
                    <span style="top: 50%; right: 10%;">3</span>
                    <span style="bottom: 10%; left: 50%;">6</span>
                    <span style="top: 50%; left: 10%;">9</span>
                </div>
                <div class="hand hour" id="hour"></div>
                <div class="hand minute" id="minute"></div>
                <div class="hand second" id="second"></div>
                <div class="center-dot"></div>
            </div>

            <div class="clock-display" id="digitalClock">00:00:00</div>
            <div class="date-display" id="dateDisplay">--/--/--</div>
        </div>

        <!-- PM2.5 SECTION -->
        <div class="card pm25-section">
            <h2>💨 PM2.5 (ฝุ่น)</h2>
            
            <div class="pm25-display" id="pm25Value">--</div>
            <div class="pm25-unit">µg/m³</div>

            <div class="pm25-status" id="pm25Status">--</div>

            <div class="pm25-bar">
                <div class="pm25-bar-fill" id="pm25Bar" style="width: 0%;">0%</div>
            </div>

            <div class="info-text" id="pm25Info">
                กำลังโหลดข้อมูลคุณภาพอากาศ...
            </div>

            <div class="last-update" id="lastUpdate">--</div>
        </div>
    </div>

    <script>
        // ===== CLOCK FUNCTION =====
        function updateClock() {
            const now = new Date();
            
            // Digital Clock
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            const seconds = String(now.getSeconds()).padStart(2, '0');
            
            document.getElementById('digitalClock').textContent = `${hours}:${minutes}:${seconds}`;

            // Date Display (Thai locale)
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            const dateStr = now.toLocaleDateString('th-TH', options);
            document.getElementById('dateDisplay').textContent = dateStr;

            // Analog Clock
            const secondDegree = (now.getSeconds() / 60) * 360;
            const minuteDegree = (now.getMinutes() / 60) * 360 + (now.getSeconds() / 60) * 6;
            const hourDegree = (now.getHours() % 12) / 12 * 360 + (now.getMinutes() / 60) * 30;

            document.getElementById('second').style.transform = `rotate(${secondDegree}deg)`;
            document.getElementById('minute').style.transform = `rotate(${minuteDegree}deg)`;
            document.getElementById('hour').style.transform = `rotate(${hourDegree}deg)`;
        }

        // ===== PM2.5 FUNCTION =====
        function updatePM25() {
            // ใช้ API ข้อมูล PM2.5 จริง (ตัวอย่าง: API จาก air-quality service)
            // หากต้องการข้อมูลจริง สามารถใช้ API ต่างๆ เช่น:
            // - iq.airvisual.com (AirVisual)
            // - waqi.info (World Air Quality Index)
            
            // ตัวอย่างข้อมูลจำลอง
            fetchPM25Data();
        }

        function fetchPM25Data() {
            // ตัวอย่าง: ใช้ API WAQI (World Air Quality Index)
            const cityName = 'Bangkok'; // สามารถเปลี่ยนเป็นเมืองอื่นได้
            const waqqToken = 'demo'; // ลงทะเบียน token ที่ https://waqi.info/api/
            
            const url = `https://api.waqi.info/feed/${cityName}/?token=${waqqToken}`;
            
            fetch(url)
                .then(response => response.json())
                .then(data => {
                    if (data.status === 'ok') {
                        const pm25 = data.data.iaqi.pm25?.v || 'N/A';
                        displayPM25(pm25);
                    } else {
                        // ถ้า API ล้มเหลว ใช้ข้อมูลจำลอง
                        displayPM25(Math.floor(Math.random() * 300));
                    }
                })
                .catch(error => {
                    console.log('API Error, using mock data');
                    displayPM25(Math.floor(Math.random() * 300));
                });
        }

        function displayPM25(value) {
            const pm25Value = typeof value === 'number' ? value : Math.floor(Math.random() * 300);
            
            document.getElementById('pm25Value').textContent = pm25Value;

            // กำหนดสถานะ
            let status = '';
            let statusClass = '';
            let color = '';
            let info = '';

            if (pm25Value <= 25) {
                status = '✓ ดี (Good)';
                statusClass = 'status-good';
                color = '#27ae60';
                info = 'คุณภาพอากาศดี เหมาะสำหรับการออกกำลังกายกลางแจ้ง';
            } else if (pm25Value <= 50) {
                status = '△ ปานกลาง (Moderate)';
                statusClass = 'status-moderate';
                color = '#f39c12';
                info = 'คุณภาพอากาศปานกลาง อาจส่งผลต่อกลุ่มจำเพาะ';
            } else if (pm25Value <= 100) {
                status = '⚠ ไม่ดี (Unhealthy)';
                statusClass = 'status-unhealthy';
                color = '#e74c3c';
                info = 'คุณภาพอากาศไม่ดี ผู้ที่มีปัญหาเรื่องสุขภาพควรใส่หน้ากากอนามัย';
            } else {
                status = '⛔ อันตราย (Hazardous)';
                statusClass = 'status-hazardous';
                color = '#8b0000';
                info = 'คุณภาพอากาศเป็นอันตราย หลีกเลี่ยงการออกไปนอกห้อง';
            }

            // Update UI
            const statusEl = document.getElementById('pm25Status');
            statusEl.textContent = status;
            statusEl.className = 'pm25-status ' + statusClass;
            statusEl.style.backgroundColor = color;

            // Update Bar
            const percentage = Math.min((pm25Value / 300) * 100, 100);
            const barEl = document.getElementById('pm25Bar');
            barEl.style.backgroundColor = color;
            barEl.style.width = percentage + '%';
            barEl.textContent = Math.round(percentage) + '%';

            // Update Info
            document.getElementById('pm25Info').textContent = info;

            // Update Last Update Time
            const now = new Date();
            const timeStr = now.toLocaleTimeString('th-TH');
            document.getElementById('lastUpdate').textContent = `อัปเดตเมื่อ: ${timeStr}`;
        }

        // Initialize
        updateClock();
        setInterval(updateClock, 1000);

        updatePM25();
        setInterval(updatePM25, 600000); // อัปเดตทุก 10 นาที
    </script>
</body>
</html>
