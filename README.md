<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ศูนย์รวมของหาย-เก็บได้ (Lost & Found)</title>
    <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        /* --- ตั้งค่าโทนสีและการจัดวาง (Theme: Green-Yellow) --- */
        :root {
            --primary-green: #2E7D32;  /* เขียวเข้ม */
            --light-green: #E8F5E9;    /* เขียวอ่อนพื้นหลัง */
            --accent-yellow: #FFEA00;  /* เหลืองสด */
            --dark-yellow: #FBC02D;    /* เหลืองเข้มสำหรับปุ่ม */
            --text-color: #1B5E20;     /* สีตัวอักษรเขียวแก่ */
            --white: #ffffff;
        }

        body {
            font-family: 'Sarabun', sans-serif;
            background-color: var(--light-green);
            color: var(--text-color);
            margin: 0;
            padding: 0;
            line-height: 1.6;
        }

        /* --- Header --- */
        header {
            background-color: var(--primary-green);
            color: var(--white);
            padding: 1rem 2rem;
            text-align: center;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        header h1 { margin: 0; font-size: 1.8rem; }
        header p { margin: 5px 0 0; font-size: 0.9rem; color: var(--accent-yellow); }

        /* --- Container --- */
        .container {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
        }

        /* --- Tab Navigation --- */
        .tabs {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
        }
        .tab-btn {
            background-color: var(--white);
            border: 2px solid var(--primary-green);
            color: var(--primary-green);
            padding: 10px 20px;
            cursor: pointer;
            font-weight: bold;
            flex: 1;
            transition: 0.3s;
        }
        .tab-btn:first-child { border-radius: 10px 0 0 10px; }
        .tab-btn:last-child { border-radius: 0 10px 10px 0; }
        .tab-btn.active {
            background-color: var(--primary-green);
            color: var(--accent-yellow);
        }

        /* --- Forms & Cards --- */
        .card {
            background: var(--white);
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
            margin-bottom: 20px;
            border-left: 10px solid var(--primary-green);
        }

        h2 { border-bottom: 2px solid var(--accent-yellow); padding-bottom: 10px; display: inline-block; }

        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: bold; }
        input, textarea, select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box; /* สำคัญ */
            font-family: 'Sarabun', sans-serif;
        }
        
        button.submit-btn {
            background-color: var(--dark-yellow);
            color: #333;
            border: none;
            padding: 12px 24px;
            font-size: 1rem;
            font-weight: bold;
            border-radius: 30px;
            cursor: pointer;
            width: 100%;
            transition: 0.3s;
            box-shadow: 0 4px 0 #F57F17;
        }
        button.submit-btn:hover { background-color: var(--accent-yellow); transform: translateY(-2px); }
        button.submit-btn:active { transform: translateY(2px); box-shadow: none; }

        /* --- List Items --- */
        .item-list { list-style: none; padding: 0; }
        .item-card {
            background: var(--white);
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }
        .item-card.lost { border-left: 8px solid #D32F2F; } /* สีแดงสำหรับของหาย */
        .item-card.found { border-left: 8px solid var(--primary-green); } /* สีเขียวสำหรับเจอ */
        
        .tag {
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: bold;
            color: white;
        }
        .tag.lost { background-color: #D32F2F; }
        .tag.found { background-color: var(--primary-green); }

        /* --- Utility --- */
        .hidden { display: none; }
        
        footer {
            text-align: center;
            padding: 20px;
            font-size: 0.8rem;
            color: var(--primary-green);
            opacity: 0.8;
        }
    </style>
</head>
<body>

    <header>
        <h1>Lost & Found Hub</h1>
        <p>ศูนย์รวมแจ้งของหาย และ ส่งคืนของที่เก็บได้</p>
    </header>

    <div class="container">
        <div class="tabs">
            <button class="tab-btn active" onclick="showSection('report')">แจ้งข้อมูล</button>
            <button class="tab-btn" onclick="showSection('list')">รายการล่าสุด</button>
        </div>

        <div id="report-section">
            <div class="card">
                <h2>📝 แบบฟอร์มแจ้งข้อมูล</h2>
                <form id="lostFoundForm" onsubmit="handleSubmit(event)">
                    <div class="form-group">
                        <label>ประเภทการแจ้ง:</label>
                        <select id="type" required>
                            <option value="lost">ของหาย (Looking for)</option>
                            <option value="found">เก็บได้ (Found it)</option>
                        </select>
                    </div>

                    <div class="form-group">
                        <label>ชื่อสิ่งของ:</label>
                        <input type="text" id="itemName" placeholder="เช่น กระเป๋าสตางค์, กุญแจรถ" required>
                    </div>

                    <div class="form-group">
                        <label>สถานที่ (หาย/เก็บได้):</label>
                        <input type="text" id="location" placeholder="เช่น โรงอาหาร, ตึก 5" required>
                    </div>

                    <div class="form-group">
                        <label>รายละเอียดเพิ่มเติม:</label>
                        <textarea id="description" rows="3" placeholder="สี, จุดสังเกต, หรือเบอร์ติดต่อกลับ"></textarea>
                    </div>

                    <button type="submit" class="submit-btn">บันทึกข้อมูล</button>
                </form>
            </div>
        </div>

        <div id="list-section" class="hidden">
            <div class="card" style="border-left-color: var(--dark-yellow);">
                <h2>📋 รายการแจ้งล่าสุด</h2>
                <ul id="itemsContainer" class="item-list">
                    <li class="item-card lost">
                        <div>
                            <strong>กุญแจรถ Honda</strong><br>
                            <small>📍 โรงอาหารกลาง</small>
                            <p style="margin: 5px 0; font-size:0.9em;">พวงกุญแจสีแดง หายช่วงพักเที่ยง</p>
                        </div>
                        <span class="tag lost">ของหาย</span>
                    </li>
                    <li class="item-card found">
                        <div>
                            <strong>บัตรนักเรียน</strong><br>
                            <small>📍 หน้าห้องสมุด</small>
                            <p style="margin: 5px 0; font-size:0.9em;">ชื่อ สมชาย ใจดี ส่งคืนที่ป้อมยามแล้ว</p>
                        </div>
                        <span class="tag found">เก็บได้</span>
                    </li>
                </ul>
            </div>
        </div>
    </div>

    <footer>
        &copy; 2024 Lost & Found Web App (Green-Yellow Theme)
    </footer>

    <script>
        // ฟังก์ชันสลับหน้า (Tabs)
        function showSection(sectionId) {
            // ซ่อนทุกส่วน
            document.getElementById('report-section').classList.add('hidden');
            document.getElementById('list-section').classList.add('hidden');
            
            // แสดงส่วนที่เลือก
            document.getElementById(sectionId + '-section').classList.remove('hidden');

            // จัดการปุ่ม Active
            const buttons = document.querySelectorAll('.tab-btn');
            buttons.forEach(btn => btn.classList.remove('active'));
            if(sectionId === 'report') buttons[0].classList.add('active');
            else buttons[1].classList.add('active');
        }

        // ฟังก์ชันจำลองการบันทึกข้อมูล (ไม่มี Backend จริง)
        function handleSubmit(event) {
            event.preventDefault(); // ป้องกันเว็บรีเฟรช

            // รับค่าจากฟอร์ม
            const type = document.getElementById('type').value;
            const name = document.getElementById('itemName').value;
            const loc = document.getElementById('location').value;
            const desc = document.getElementById('description').value;

            // สร้าง HTML ของการ์ดใหม่
            const newItemHTML = `
                <li class="item-card ${type}">
                    <div>
                        <strong>${name}</strong><br>
                        <small>📍 ${loc}</small>
                        <p style="margin: 5px 0; font-size:0.9em;">${desc}</p>
                    </div>
                    <span class="tag ${type}">${type === 'lost' ? 'ของหาย' : 'เก็บได้'}</span>
                </li>
            `;

            // เพิ่มลงในรายการ (แทรกด้านบนสุด)
            const listContainer = document.getElementById('itemsContainer');
            listContainer.insertAdjacentHTML('afterbegin', newItemHTML);

            // แจ้งเตือนและเคลียร์ค่า
            alert('บันทึกข้อมูลเรียบร้อย! (ข้อมูลจำลอง)');
            document.getElementById('lostFoundForm').reset();
            
            // สลับไปหน้าแสดงรายการอัตโนมัติ
            showSection('list');
        }
    </script>
</body>
</html>
