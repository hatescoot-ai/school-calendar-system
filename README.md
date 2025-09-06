# school-calendar-system
學校行事曆管理系統 - 支援班級事務、學年事務、課務提醒的完整解決方案
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>學校行事曆管理系統</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Microsoft JhengHei', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            backdrop-filter: blur(10px);
        }

        .header h1 {
            text-align: center;
            color: #4a5568;
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
        }

        .nav-tabs {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 10px;
        }

        .tab-btn {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        .tab-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }

        .tab-btn.active {
            background: linear-gradient(45deg, #48bb78, #38a169);
            transform: scale(1.05);
        }

        .main-content {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            backdrop-filter: blur(10px);
        }

        /* 日曆樣式 */
        .calendar-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 15px;
        }

        .calendar-nav {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .nav-btn {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 18px;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .nav-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
        }

        .current-month {
            font-size: 24px;
            font-weight: bold;
            color: #4a5568;
            min-width: 200px;
            text-align: center;
        }

        .view-selector {
            display: flex;
            gap: 10px;
        }

        .view-btn {
            background: #e2e8f0;
            color: #4a5568;
            border: none;
            padding: 8px 16px;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .view-btn.active {
            background: linear-gradient(45deg, #48bb78, #38a169);
            color: white;
        }

        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 2px;
            margin-bottom: 20px;
        }

        .calendar-day-header {
            background: #4a5568;
            color: white;
            padding: 15px 5px;
            text-align: center;
            font-weight: bold;
            border-radius: 8px;
        }

        .calendar-day {
            background: #f7fafc;
            min-height: 120px;
            padding: 8px;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            border: 2px solid transparent;
        }

        .calendar-day:hover {
            background: #e2e8f0;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
        }

        .calendar-day.other-month {
            opacity: 0.3;
        }

        .calendar-day.today {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-color: #48bb78;
        }

        .day-number {
            font-weight: bold;
            font-size: 16px;
            margin-bottom: 5px;
        }

        .event-item {
            background: linear-gradient(45deg, #48bb78, #38a169);
            color: white;
            padding: 2px 6px;
            margin: 2px 0;
            border-radius: 12px;
            font-size: 10px;
            text-overflow: ellipsis;
            overflow: hidden;
            white-space: nowrap;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .event-item:hover {
            transform: scale(1.02);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
        }

        .event-item.class-affairs {
            background: linear-gradient(45deg, #ed8936, #dd6b20);
        }

        .event-item.grade-affairs {
            background: linear-gradient(45deg, #3182ce, #2c5aa0);
        }

        .event-item.course-affairs {
            background: linear-gradient(45deg, #9f7aea, #805ad5);
        }

        /* 待辦事項區域 */
        .todo-section {
            display: none;
        }

        .todo-section.active {
            display: block;
        }

        .todo-categories {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .todo-category {
            background: linear-gradient(135deg, #f7fafc, #edf2f7);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
        }

        .todo-category:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
        }

        .category-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #e2e8f0;
        }

        .category-title {
            font-size: 18px;
            font-weight: bold;
            color: #4a5568;
        }

        .add-todo-btn {
            background: linear-gradient(45deg, #48bb78, #38a169);
            color: white;
            border: none;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 18px;
            transition: all 0.3s ease;
        }

        .add-todo-btn:hover {
            transform: scale(1.1);
        }

        .todo-item {
            display: flex;
            align-items: center;
            padding: 10px;
            margin: 8px 0;
            background: white;
            border-radius: 10px;
            border-left: 4px solid #48bb78;
            transition: all 0.3s ease;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }

        .todo-item:hover {
            transform: translateX(5px);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.15);
        }

        .todo-item.completed {
            opacity: 0.6;
            border-left-color: #9ca3af;
        }

        .todo-item.completed .todo-text {
            text-decoration: line-through;
        }

        .todo-checkbox {
            margin-right: 10px;
            transform: scale(1.2);
        }

        .todo-text {
            flex: 1;
            font-size: 14px;
        }

        .todo-date {
            font-size: 12px;
            color: #9ca3af;
            margin-left: 10px;
        }

        /* 後台管理 */
        .admin-section {
            display: none;
        }

        .admin-section.active {
            display: block;
        }

        .file-upload-area {
            border: 2px dashed #cbd5e0;
            border-radius: 15px;
            padding: 40px;
            text-align: center;
            margin-bottom: 30px;
            transition: all 0.3s ease;
            background: linear-gradient(135deg, #f7fafc, #edf2f7);
        }

        .file-upload-area:hover {
            border-color: #667eea;
            background: linear-gradient(135deg, #edf2f7, #e2e8f0);
            transform: scale(1.02);
        }

        .file-upload-area.dragover {
            border-color: #48bb78;
            background: linear-gradient(135deg, #f0fff4, #c6f6d5);
        }

        .upload-icon {
            font-size: 48px;
            color: #9ca3af;
            margin-bottom: 15px;
        }

        .upload-text {
            font-size: 18px;
            color: #4a5568;
            margin-bottom: 10px;
        }

        .upload-subtext {
            font-size: 14px;
            color: #9ca3af;
        }

        .file-input {
            display: none;
        }

        .upload-btn {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            margin: 10px;
            transition: all 0.3s ease;
        }

        .upload-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }

        /* Modal 樣式 */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(5px);
        }

        .modal-content {
            background: white;
            margin: 5% auto;
            padding: 30px;
            border-radius: 15px;
            width: 90%;
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            animation: modalSlideIn 0.3s ease;
        }

        @keyframes modalSlideIn {
            from {
                opacity: 0;
                transform: translateY(-50px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .close {
            color: #9ca3af;
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
            transition: color 0.3s ease;
        }

        .close:hover {
            color: #e53e3e;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #4a5568;
        }

        .form-input, .form-select, .form-textarea {
            width: 100%;
            padding: 12px;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            font-size: 16px;
            transition: all 0.3s ease;
        }

        .form-input:focus, .form-select:focus, .form-textarea:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .form-textarea {
            resize: vertical;
            min-height: 100px;
        }

        .form-btn {
            background: linear-gradient(45deg, #48bb78, #38a169);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
            transition: all 0.3s ease;
        }

        .form-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }

        /* 響應式設計 */
        @media (max-width: 768px) {
            .container {
                padding: 10px;
            }

            .header h1 {
                font-size: 2em;
            }

            .nav-tabs {
                flex-direction: column;
                align-items: center;
            }

            .calendar-header {
                flex-direction: column;
                text-align: center;
            }

            .calendar-nav {
                order: 2;
            }

            .current-month {
                order: 1;
            }

            .view-selector {
                order: 3;
            }

            .calendar-day {
                min-height: 80px;
                padding: 5px;
            }

            .day-number {
                font-size: 14px;
            }

            .event-item {
                font-size: 9px;
            }

            .todo-categories {
                grid-template-columns: 1fr;
            }

            .modal-content {
                margin: 10% auto;
                width: 95%;
                padding: 20px;
            }
        }

        /* 載入動畫 */
        .loading {
            display: none;
            text-align: center;
            padding: 20px;
        }

        .loading.show {
            display: block;
        }

        .spinner {
            width: 40px;
            height: 40px;
            border: 4px solid #e2e8f0;
            border-top: 4px solid #667eea;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* 成功/錯誤提示 */
        .alert {
            padding: 15px 20px;
            border-radius: 10px;
            margin: 15px 0;
            display: none;
        }

        .alert.success {
            background: linear-gradient(135deg, #c6f6d5, #9ae6b4);
            color: #1a365d;
            border-left: 4px solid #48bb78;
        }

        .alert.error {
            background: linear-gradient(135deg, #fed7d7, #feb2b2);
            color: #742a2a;
            border-left: 4px solid #e53e3e;
        }

        .alert.show {
            display: block;
            animation: slideInRight 0.3s ease;
        }

        @keyframes slideInRight {
            from {
                opacity: 0;
                transform: translateX(100%);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎓 學校行事曆管理系統</h1>
            <div class="nav-tabs">
                <button class="tab-btn active" onclick="showTab('calendar')">📅 日曆檢視</button>
                <button class="tab-btn" onclick="showTab('todos')">📝 待辦事項</button>
                <button class="tab-btn" onclick="showTab('admin')">⚙️ 後台管理</button>
            </div>
        </div>

        <div class="main-content">
            <!-- 日曆檢視 -->
            <div id="calendar-tab" class="tab-content">
                <div class="calendar-header">
                    <div class="calendar-nav">
                        <button class="nav-btn" onclick="previousMonth()">‹</button>
                        <span class="current-month" id="currentMonth"></span>
                        <button class="nav-btn" onclick="nextMonth()">›</button>
                    </div>
                    <div class="view-selector">
                        <button class="view-btn active" onclick="setView('month')">月檢視</button>
                        <button class="view-btn" onclick="setView('week')">週檢視</button>
                        <button class="view-btn" onclick="setView('day')">日檢視</button>
                    </div>
                </div>
                <div class="calendar-grid" id="calendarGrid"></div>
            </div>

            <!-- 待辦事項 -->
            <div id="todos-tab" class="tab-content todo-section">
                <div class="todo-categories">
                    <div class="todo-category">
                        <div class="category-header">
                            <span class="category-title">📚 班級事務</span>
                            <button class="add-todo-btn" onclick="openAddTodoModal('class-affairs')">+</button>
                        </div>
                        <div id="class-affairs-todos"></div>
                    </div>
                    <div class="todo-category">
                        <div class="category-header">
                            <span class="category-title">🏫 學年事務</span>
                            <button class="add-todo-btn" onclick="openAddTodoModal('grade-affairs')">+</button>
                        </div>
                        <div id="grade-affairs-todos"></div>
                    </div>
                    <div class="todo-category">
                        <div class="category-header">
                            <span class="category-title">📖 課務提醒</span>
                            <button class="add-todo-btn" onclick="openAddTodoModal('course-affairs')">+</button>
                        </div>
                        <div id="course-affairs-todos"></div>
                    </div>
                </div>
            </div>

            <!-- 後台管理 -->
            <div id="admin-tab" class="tab-content admin-section">
                <h2 style="margin-bottom: 30px; color: #4a5568;">📁 檔案匯入管理</h2>
                
                <div class="file-upload-area" id="uploadArea">
                    <div class="upload-icon">📁</div>
                    <div class="upload-text">拖拽檔案至此處或點擊選擇</div>
                    <div class="upload-subtext">支援格式: WORD, EXCEL, PDF, 圖片, 文字檔</div>
                    <input type="file" id="fileInput" class="file-input" multiple accept=".doc,.docx,.xls,.xlsx,.pdf,.txt,.jpg,.jpeg,.png,.gif">
                    <button class="upload-btn" onclick="document.getElementById('fileInput').click()">選擇檔案</button>
                    <button class="upload-btn" onclick="importSampleData()">匯入範例資料</button>
                </div>

                <div class="alert success" id="successAlert"></div>
                <div class="alert error" id="errorAlert"></div>
                <div class="loading" id="loadingIndicator">
                    <div class="spinner"></div>
                    <div>正在處理檔案...</div>
                </div>
            </div>
        </div>
    </div>

    <!-- 新增待辦事項 Modal -->
    <div id="todoModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeTodoModal()">&times;</span>
            <h2 style="margin-bottom: 20px; color: #4a5568;">➕ 新增待辦事項</h2>
            <form id="todoForm">
                <div class="form-group">
                    <label class="form-label">事項標題</label>
                    <input type="text" id="todoTitle" class="form-input" required>
                </div>
                <div class="form-group">
                    <label class="form-label">類別</label>
                    <select id="todoCategory" class="form-select" required>
                        <option value="class-affairs">班級事務</option>
                        <option value="grade-affairs">學年事務</option>
                        <option value="course-affairs">課務提醒</option>
                    </select>
                </div>
                <div class="form-group">
                    <label class="form-label">截止日期</label>
                    <input type="date" id="todoDate" class="form-input">
                </div>
                <div class="form-group">
                    <label class="form-label">詳細描述</label>
                    <textarea id="todoDescription" class="form-textarea" placeholder="選填..."></textarea>
                </div>
                <button type="submit" class="form-btn">新增事項</button>
            </form>
        </div>
    </div>

    <!-- 事件詳情 Modal -->
    <div id="eventModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeEventModal()">&times;</span>
            <h2 id="eventTitle" style="margin-bottom: 20px; color: #4a5568;"></h2>
            <div id="eventDetails"></div>
        </div>
    </div>

    <script>
        // 全域變數
        let currentDate = new Date();
        let currentView = 'month';
        let events = [];
        let todos = {
            'class-affairs': [],
            'grade-affairs': [],
            'course-affairs': []
        };

        // 初始化應用程式
        document.addEventListener('DOMContentLoaded', function() {
            initializeApp();
            setupEventListeners();
        });

        function initializeApp() {
            updateCalendar();
            renderTodos();
            loadSampleEvents();
        }

        function setupEventListeners() {
            // 檔案上傳拖拽功能
            const uploadArea = document.getElementById('uploadArea');
            const fileInput = document.getElementById('fileInput');

            uploadArea.addEventListener('dragover', function(e) {
                e.preventDefault();
                uploadArea.classList.add('dragover');
            });

            uploadArea.addEventListener('dragleave', function(e) {
                e.preventDefault();
                uploadArea.classList.remove('dragover');
            });

            uploadArea.addEventListener('drop', function(e) {
                e.preventDefault();
                uploadArea.classList.remove('dragover');
                const files = e.dataTransfer.files;
                handleFiles(files);
            });

            uploadArea.addEventListener('click', function() {
                fileInput.click();
            });

            fileInput.addEventListener('change', function(e) {
                handleFiles(e.target.files);
            });

            // 待辦事項表單提交
            document.getElementById('todoForm').addEventListener('submit', function(e) {
                e.preventDefault();
                addTodo();
            });
        }

        // 標籤切換功能
        function showTab(tabName) {
            // 隱藏所有標籤內容
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.style.display = 'none';
            });
            
            // 移除所有按鈕的 active 類別
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // 顯示選中的標籤
            document.getElementById(tabName + '-tab').style.display = 'block';
            
            // 添加 active 類別到點擊的按鈕
            event.target.classList.add('active');
            
            if (tabName === 'calendar') {
                updateCalendar();
            } else if (tabName === 'todos') {
                renderTodos();
            }
        }

        // 日曆功能
        function updateCalendar() {
            const year = currentDate.getFullYear();
            const month = currentDate.getMonth();
            
            document.getElementById('currentMonth').textContent = 
                `${year}年 ${month + 1}月`;
            
            renderCalendar(year, month);
        }

        function renderCalendar(year, month) {
            const grid = document.getElementById('calendarGrid');
            grid.innerHTML = '';
            
            // 添加星期標題
            const weekdays = ['日', '一', '二', '三', '四', '五', '六'];
            weekdays.forEach(day => {
                const header = document.createElement('div');
                header.className = 'calendar-day-header';
                header.textContent = day;
                grid.appendChild(header);
            });
            
            // 獲取月份的第一天和最後一天
            const firstDay = new Date(year, month, 1);
            const lastDay = new Date(year, month + 1, 0);
            const startDate = new Date(firstDay);
            startDate.setDate(startDate.getDate() - firstDay.getDay());
            
            // 渲染日期格子
            for (let i = 0; i < 42; i++) {
                const currentCellDate = new Date(startDate);
                currentCellDate.setDate(startDate.getDate() + i);
                
                const dayElement = document.createElement('div');
                dayElement.className = 'calendar-day';
                
                // 檢查是否為其他月份
                if (currentCellDate.getMonth() !== month) {
                    dayElement.classList.add('other-month');
                }
                
                // 檢查是否為今天
                const today = new Date();
                if (currentCellDate.toDateString() === today.toDateString()) {
                    dayElement.classList.add('today');
                }
                
                // 添加日期數字
                const dayNumber = document.createElement('div');
                dayNumber.className = 'day-number';
                dayNumber.textContent = currentCellDate.getDate();
                dayElement.appendChild(dayNumber);
                
                // 添加事件
                const dayEvents = getEventsForDate(currentCellDate);
                dayEvents.forEach(event => {
                    const eventElement = document.createElement('div');
                    eventElement.className = `event-item ${event.category}`;
                    eventElement.textContent = event.title;
                    eventElement.onclick = () => showEventDetails(event);
                    dayElement.appendChild(eventElement);
                });
                
                dayElement.onclick = () => selectDate(currentCellDate);
                grid.appendChild(dayElement);
            }
        }

        function previousMonth() {
            currentDate.setMonth(currentDate.getMonth() - 1);
            updateCalendar();
        }

        function nextMonth() {
            currentDate.setMonth(currentDate.getMonth() + 1);
            updateCalendar();
        }

        function setView(view) {
            currentView = view;
            document.querySelectorAll('.view-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            // 這裡可以添加不同視圖的邏輯
            updateCalendar();
        }

        function getEventsForDate(date) {
            return events.filter(event => {
                const eventDate = new Date(event.date);
                return eventDate.toDateString() === date.toDateString();
            });
        }

        function selectDate(date) {
            console.log('選中日期:', date);
        }

        function showEventDetails(event) {
            document.getElementById('eventTitle').textContent = event.title;
            document.getElementById('eventDetails').innerHTML = `
                <p><strong>📅 日期:</strong> ${event.date}</p>
                <p><strong>⏰ 時間:</strong> ${event.startTime || '全天'} ${event.endTime ? '- ' + event.endTime : ''}</p>
                <p><strong>📋 類別:</strong> ${getCategoryName(event.category)}</p>
                ${event.description ? `<p><strong>📝 描述:</strong> ${event.description}</p>` : ''}
                <p><strong>✅ 狀態:</strong> ${event.completed ? '已完成' : '進行中'}</p>
            `;
            document.getElementById('eventModal').style.display = 'block';
        }

        function getCategoryName(category) {
            const names = {
                'class-affairs': '班級事務',
                'grade-affairs': '學年事務',
                'course-affairs': '課務提醒'
            };
            return names[category] || category;
        }

        function closeEventModal() {
            document.getElementById('eventModal').style.display = 'none';
        }

        // 待辦事項功能
        function renderTodos() {
            Object.keys(todos).forEach(category => {
                const container = document.getElementById(category + '-todos');
                container.innerHTML = '';
                
                todos[category].forEach((todo, index) => {
                    const todoElement = document.createElement('div');
                    todoElement.className = 'todo-item' + (todo.completed ? ' completed' : '');
                    todoElement.innerHTML = `
                        <input type="checkbox" class="todo-checkbox" ${todo.completed ? 'checked' : ''} 
                               onchange="toggleTodo('${category}', ${index})">
                        <span class="todo-text">${todo.title}</span>
                        <span class="todo-date">${todo.date || ''}</span>
                        <button style="background: #e53e3e; color: white; border: none; border-radius: 50%; width: 25px; height: 25px; margin-left: 10px; cursor: pointer;" 
                                onclick="deleteTodo('${category}', ${index})">×</button>
                    `;
                    container.appendChild(todoElement);
                });
            });
        }

        function openAddTodoModal(category) {
            document.getElementById('todoCategory').value = category;
            document.getElementById('todoModal').style.display = 'block';
        }

        function closeTodoModal() {
            document.getElementById('todoModal').style.display = 'none';
            document.getElementById('todoForm').reset();
        }

        function addTodo() {
            const title = document.getElementById('todoTitle').value;
            const category = document.getElementById('todoCategory').value;
            const date = document.getElementById('todoDate').value;
            const description = document.getElementById('todoDescription').value;

            const newTodo = {
                title: title,
                date: date,
                description: description,
                completed: false,
                id: Date.now()
            };

            todos[category].push(newTodo);
            
            // 同時添加到日曆事件
            if (date) {
                events.push({
                    title: title,
                    date: date,
                    category: category,
                    description: description,
                    completed: false,
                    startTime: '',
                    endTime: '',
                    allDay: true
                });
            }

            renderTodos();
            updateCalendar();
            closeTodoModal();
            showAlert('success', '待辦事項新增成功！');
        }

        function toggleTodo(category, index) {
            todos[category][index].completed = !todos[category][index].completed;
            
            // 更新對應的日曆事件
            const todo = todos[category][index];
            const eventIndex = events.findIndex(e => e.title === todo.title && e.category === category);
            if (eventIndex !== -1) {
                events[eventIndex].completed = todo.completed;
            }
            
            renderTodos();
            updateCalendar();
        }

        function deleteTodo(category, index) {
            if (confirm('確定要刪除此待辦事項嗎？')) {
                const todo = todos[category][index];
                
                // 從日曆事件中移除
                const eventIndex = events.findIndex(e => e.title === todo.title && e.category === category);
                if (eventIndex !== -1) {
                    events.splice(eventIndex, 1);
                }
                
                todos[category].splice(index, 1);
                renderTodos();
                updateCalendar();
                showAlert('success', '待辦事項已刪除！');
            }
        }

        // 檔案處理功能
        function handleFiles(files) {
            showLoading(true);
            
            Array.from(files).forEach(file => {
                const reader = new FileReader();
                reader.onload = function(e) {
                    try {
                        if (file.name.toLowerCase().includes('.csv') || file.type.includes('excel')) {
                            parseCSVData(e.target.result);
                        } else if (file.type.includes('text')) {
                            parseTextData(e.target.result);
                        } else {
                            console.log('檔案類型:', file.type, '檔案名稱:', file.name);
                            showAlert('success', `檔案 "${file.name}" 上傳成功！`);
                        }
                    } catch (error) {
                        console.error('檔案處理錯誤:', error);
                        showAlert('error', `檔案 "${file.name}" 處理失敗！`);
                    }
                };
                
                if (file.type.includes('text') || file.name.toLowerCase().includes('.csv')) {
                    reader.readAsText(file, 'UTF-8');
                } else {
                    reader.readAsDataURL(file);
                }
            });
            
            setTimeout(() => {
                showLoading(false);
            }, 2000);
        }

        function parseCSVData(csvData) {
            const lines = csvData.split('\n');
            const headers = lines[0].split(',');
            
            for (let i = 1; i < lines.length; i++) {
                if (lines[i].trim() === '') continue;
                
                const values = lines[i].split(',');
                const event = {
                    title: values[0] || '',
                    date: formatDate(values[1]) || '',
                    startTime: values[2] || '',
                    endTime: values[4] || '',
                    allDay: values[5] === 'True',
                    category: determineCategory(values[0] || ''),
                    completed: false,
                    description: ''
                };
                
                if (event.title && event.date) {
                    events.push(event);
                }
            }
            
            updateCalendar();
            showAlert('success', 'CSV 資料匯入成功！');
        }

        function formatDate(dateString) {
            if (!dateString) return '';
            // 處理 2026/3/24 格式
            const parts = dateString.split('/');
            if (parts.length === 3) {
                const year = parts[0];
                const month = parts[1].padStart(2, '0');
                const day = parts[2].padStart(2, '0');
                return `${year}-${month}-${day}`;
            }
            return dateString;
        }

        function determineCategory(title) {
            if (title.includes('G10') || title.includes('G7') || title.includes('G12')) {
                return 'course-affairs';
            } else if (title.includes('學年') || title.includes('會議') || title.includes('MAP')) {
                return 'grade-affairs';
            } else {
                return 'class-affairs';
            }
        }

        function parseTextData(textData) {
            // 簡單的文字資料解析
            const lines = textData.split('\n');
            lines.forEach(line => {
                if (line.trim()) {
                    events.push({
                        title: line.trim(),
                        date: new Date().toISOString().split('T')[0],
                        category: 'class-affairs',
                        completed: false,
                        allDay: true
                    });
                }
            });
            updateCalendar();
            showAlert('success', '文字資料匯入成功！');
        }

        function importSampleData() {
            showLoading(true);
            
            const sampleData = `Subject,Start Date,Start Time,End Date,End Time,All day event
單車環台主活動,2026/3/24,,2026/4/2,,True
第一次行前訓練,2025/12/6,,,True
第二次行前訓練,2025/12/20,,,True
第三次行前訓練,2026/1/11,,,True
第四次行前訓練,2026/3/7,,,True
第五次行前訓練,2026/3/15,,,True
第六次行前訓練,2026/3/21,,,True
發放健康資料卡及同意書,2025/9/1,,,True
非住宿生資料繳回,2025/9/2,,,True
住宿生資料繳回,2025/9/8,,,True
發放尿管,2025/9/12,,,True
尿液檢體收集,2025/9/15,,2025/9/16,,True
繳交尿液檢體,2025/9/16,8:25 AM,2025/9/16,9:25 AM,False
正式體檢日,2025/10/13,,,True
班親會準備時程,2025/9/19,4:35 PM,2025/9/19,8:35 PM,False
班親會時間,2025/9/20,7:30 AM,2025/9/20,2:30 PM,False`;

            setTimeout(() => {
                parseCSVData(sampleData);
                showLoading(false);
                showAlert('success', '範例資料匯入完成！');
            }, 1500);
        }

        function loadSampleEvents() {
            // 載入一些預設事件作為示例
            const today = new Date();
            const tomorrow = new Date(today);
            tomorrow.setDate(tomorrow.getDate() + 1);
            
            events.push(
                {
                    title: '班級會議',
                    date: today.toISOString().split('T')[0],
                    category: 'class-affairs',
                    startTime: '14:00',
                    endTime: '15:00',
                    completed: false,
                    description: '討論本月班級活動安排'
                },
                {
                    title: 'G10 主題課程',
                    date: tomorrow.toISOString().split('T')[0],
                    category: 'course-affairs',
                    startTime: '12:10',
                    endTime: '12:55',
                    completed: false,
                    description: '霸凌防治與申訴'
                }
            );
            
            // 載入一些預設待辦事項
            todos['class-affairs'].push(
                { title: '準備班親會資料', date: '2024-09-15', completed: false, description: '整理學生成績單和活動照片' },
                { title: '更新班級公布欄', date: '2024-09-10', completed: false, description: '張貼新的活動海報' }
            );
            
            todos['grade-affairs'].push(
                { title: '參加學年主任會議', date: '2024-09-12', completed: false, description: '討論期中考事宜' },
                { title: '提交學年活動計畫', date: '2024-09-20', completed: false, description: '規劃下學期活動' }
            );
            
            todos['course-affairs'].push(
                { title: '準備G10主題課程教材', date: '2024-09-08', completed: false, description: '霸凌防治宣導資料' },
                { title: '批改G12模擬考試卷', date: '2024-09-15', completed: false, description: '統計成績並製作分析報告' }
            );
        }

        // 輔助功能
        function showAlert(type, message) {
            const alert = document.getElementById(type + 'Alert');
            alert.textContent = message;
            alert.classList.add('show');
            
            setTimeout(() => {
                alert.classList.remove('show');
            }, 3000);
        }

        function showLoading(show) {
            const loading = document.getElementById('loadingIndicator');
            if (show) {
                loading.classList.add('show');
            } else {
                loading.classList.remove('show');
            }
        }

        // Modal 點擊外部關閉
        window.onclick = function(event) {
            const todoModal = document.getElementById('todoModal');
            const eventModal = document.getElementById('eventModal');
            
            if (event.target == todoModal) {
                closeTodoModal();
            }
            if (event.target == eventModal) {
                closeEventModal();
            }
        }

        // 鍵盤快捷鍵
        document.addEventListener('keydown', function(e) {
            if (e.key === 'Escape') {
                closeTodoModal();
                closeEventModal();
            }
        });
    </script>
</body>
</html>
