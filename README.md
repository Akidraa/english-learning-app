<!doctype html>
<html lang="ru">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>English Learning Adventure</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <script src="/_sdk/data_sdk.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <style>
        body {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Inter', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
            background-attachment: fixed;
            min-height: 100%;
            color: #1a202c;
            overflow-x: hidden;
            position: relative;
        }

        .container {
            max-width: 1400px;
            margin: 10px auto;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            border: 2px solid rgba(255,255,255,0.3);
            position: relative;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
            position: relative;
        }

        .title {
            font-size: 36px;
            background: linear-gradient(135deg, #667eea, #764ba2, #f093fb);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin: 0;
            font-weight: 900;
            letter-spacing: -0.03em;
            line-height: 1.1;
            display: inline-block;
        }

        .subtitle {
            font-size: 16px;
            color: #64748b;
            margin: 12px 0;
            font-weight: 500;
            opacity: 0.9;
        }

        .nav-tabs {
            display: flex;
            justify-content: center;
            gap: 6px;
            margin: 20px 0;
            flex-wrap: wrap;
            background: rgba(255, 255, 255, 0.6);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 6px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
            border: 1px solid rgba(255,255,255,0.2);
        }

        .nav-tab {
            background: transparent;
            color: #64748b;
            border: none;
            border-radius: 12px;
            padding: 8px 12px;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            white-space: nowrap;
        }

        .nav-tab:hover {
            background: rgba(102, 126, 234, 0.1);
            color: #667eea;
            transform: translateY(-1px);
        }

        .nav-tab.active {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            box-shadow: 
                0 4px 20px rgba(102, 126, 234, 0.4),
                0 0 0 1px rgba(255,255,255,0.1);
            transform: translateY(-1px);
        }

        .content-section {
            display: none;
            animation: fadeIn 0.5s ease;
        }

        .content-section.active {
            display: block;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 16px;
            margin: 30px 0;
        }

        .stat-card {
            background: rgba(255,255,255,0.95);
            backdrop-filter: blur(15px);
            border-radius: 16px;
            padding: 20px 15px;
            text-align: center;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            border: 2px solid rgba(255,255,255,0.3);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .stat-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 35px rgba(0,0,0,0.15);
        }

        .stat-number {
            font-size: 32px;
            font-weight: 900;
            background: linear-gradient(135deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 6px;
            line-height: 1;
        }

        .stat-label {
            font-size: 13px;
            color: #64748b;
            font-weight: 600;
            margin-top: 6px;
            letter-spacing: 0.025em;
        }

        .daily-goal {
            background: linear-gradient(135deg, rgba(255,255,255,0.95), rgba(248,250,255,0.95));
            backdrop-filter: blur(20px);
            border-radius: 20px;
            padding: 20px 24px;
            text-align: center;
            margin: 24px 0;
            box-shadow: 
                0 16px 32px rgba(0,0,0,0.08),
                0 0 0 1px rgba(255,255,255,0.1),
                inset 0 1px 0 rgba(255,255,255,0.2);
            border: 1px solid rgba(255,255,255,0.2);
            position: relative;
            overflow: hidden;
        }

        .daily-goal::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 2px;
            background: linear-gradient(90deg, #f093fb, #f5576c);
        }

        .daily-goal-text {
            font-size: 16px;
            font-weight: 700;
            color: #1a202c;
            margin-bottom: 6px;
        }

        .daily-goal .save-info {
            font-size: 12px;
            color: #64748b;
            margin-top: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 6px;
            font-weight: 500;
        }

        #save-indicator {
            background: linear-gradient(135deg, #10b981, #059669);
            color: white;
            padding: 3px 6px;
            border-radius: 10px;
            font-size: 11px;
            font-weight: 600;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .week-selector {
            display: flex;
            gap: 6px;
            margin: 16px 0;
            flex-wrap: wrap;
            justify-content: center;
        }

        .week-btn {
            background: rgba(255,255,255,0.8);
            border: 2px solid rgba(102, 126, 234, 0.3);
            border-radius: 10px;
            padding: 8px 12px;
            font-size: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            color: #667eea;
        }

        .week-btn:hover {
            background: rgba(102, 126, 234, 0.1);
            border-color: #667eea;
            transform: translateY(-2px);
        }

        .week-btn.active {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-color: transparent;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.3);
        }

        .week-content {
            background: rgba(255,255,255,0.95);
            border-radius: 16px;
            padding: 20px;
            margin: 16px 0;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }

        .daily-tasks {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 12px;
            margin: 16px 0;
        }

        .day-card {
            background: linear-gradient(135deg, #ffffff, #f8faff);
            border-radius: 12px;
            padding: 16px 12px;
            text-align: center;
            border: 2px solid rgba(102, 126, 234, 0.1);
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
        }

        .day-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.1);
            border-color: #667eea;
        }

        .day-card.completed {
            background: linear-gradient(135deg, #11998e, #38ef7d);
            color: white;
            border-color: #11998e;
        }

        .day-card.current {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-color: #667eea;
            animation: pulse 2s infinite;
        }

        .day-number {
            font-size: 16px;
            font-weight: 700;
            margin-bottom: 6px;
        }

        .day-task {
            font-size: 11px;
            opacity: 0.9;
        }

        .sentence-builder {
            background: rgba(255,255,255,0.95);
            border-radius: 16px;
            padding: 20px;
            margin: 16px 0;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }

        .word-bank {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin: 16px 0;
            padding: 16px;
            background: rgba(248, 250, 255, 0.8);
            border-radius: 12px;
            border: 2px dashed rgba(102, 126, 234, 0.3);
        }

        .word-token {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 8px 12px;
            border-radius: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            user-select: none;
            box-shadow: 0 4px 10px rgba(102, 126, 234, 0.3);
            font-size: 14px;
        }

        .word-token:hover {
            transform: translateY(-2px) scale(1.05);
            box-shadow: 0 6px 15px rgba(102, 126, 234, 0.4);
        }

        .sentence-area {
            min-height: 60px;
            background: rgba(255,255,255,0.9);
            border: 3px dashed rgba(102, 126, 234, 0.3);
            border-radius: 12px;
            padding: 16px;
            margin: 16px 0;
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }

        .sentence-area:hover {
            border-color: #667eea;
            background: rgba(248, 250, 255, 0.9);
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        .categories-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 16px;
            margin: 24px 0;
        }

        .category-card {
            background: linear-gradient(135deg, #ffffff, #f8faff);
            border-radius: 16px;
            padding: 20px;
            text-align: center;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            border: 2px solid rgba(255,255,255,0.5);
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .category-card:hover {
            transform: translateY(-3px) scale(1.02);
            box-shadow: 0 15px 35px rgba(0,0,0,0.15);
        }

        .category-emoji {
            font-size: 48px;
            margin-bottom: 12px;
        }

        .category-name {
            font-size: 20px;
            font-weight: 700;
            color: #2C3E50;
            margin-bottom: 8px;
        }

        .category-progress {
            font-size: 14px;
            color: #6c757d;
        }

        .word-card {
            background: linear-gradient(135deg, rgba(255,255,255,0.98), rgba(248,250,255,0.98));
            backdrop-filter: blur(30px);
            border-radius: 24px;
            padding: 32px 24px;
            text-align: center;
            margin: 24px 0;
            box-shadow: 
                0 24px 48px rgba(0,0,0,0.08),
                0 0 0 1px rgba(255,255,255,0.1),
                inset 0 1px 0 rgba(255,255,255,0.2);
            border: 1px solid rgba(255,255,255,0.2);
            position: relative;
            overflow: hidden;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .word-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 3px;
            background: linear-gradient(90deg, #667eea, #764ba2, #f093fb);
            opacity: 0.8;
        }

        .word-card:hover {
            transform: translateY(-2px) scale(1.01);
            box-shadow: 
                0 32px 64px rgba(0,0,0,0.12),
                0 0 0 1px rgba(255,255,255,0.2);
        }

        .word-emoji {
            font-size: 80px;
            margin-bottom: 16px;
            filter: drop-shadow(0 8px 24px rgba(0,0,0,0.15));
            transition: all 0.3s ease;
        }

        .word-card:hover .word-emoji {
            transform: scale(1.05);
            filter: drop-shadow(0 12px 32px rgba(0,0,0,0.2));
        }

        .word-english {
            font-size: 36px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            font-weight: 900;
            margin: 16px 0;
            letter-spacing: -0.02em;
            line-height: 1.1;
        }

        .word-russian {
            font-size: 20px;
            color: #64748b;
            margin: 12px 0;
            font-weight: 600;
        }

        .word-pronunciation {
            font-size: 16px;
            color: #94a3b8;
            font-style: italic;
            margin: 10px 0;
            font-weight: 500;
        }

        .action-buttons {
            display: flex;
            justify-content: center;
            gap: 12px;
            margin: 24px 0;
            flex-wrap: wrap;
        }

        .action-btn {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border: none;
            border-radius: 12px;
            padding: 12px 20px;
            font-size: 14px;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 8px 20px rgba(102, 126, 234, 0.3);
            display: flex;
            align-items: center;
            gap: 8px;
            white-space: nowrap;
        }

        .action-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 12px 30px rgba(102, 126, 234, 0.4);
        }

        .action-btn:active {
            transform: translateY(-1px);
        }

        .action-btn.favorite {
            background: linear-gradient(135deg, #f093fb, #f5576c);
            box-shadow: 
                0 8px 24px rgba(240, 147, 251, 0.25),
                0 0 0 1px rgba(255,255,255,0.1);
        }

        .action-btn.favorite:hover {
            box-shadow: 
                0 12px 32px rgba(240, 147, 251, 0.35),
                0 0 0 1px rgba(255,255,255,0.2);
        }

        .action-btn.speak {
            background: linear-gradient(135deg, #4facfe, #00f2fe);
            box-shadow: 
                0 8px 24px rgba(79, 172, 254, 0.25),
                0 0 0 1px rgba(255,255,255,0.1);
        }

        .action-btn.speak:hover {
            box-shadow: 
                0 12px 32px rgba(79, 172, 254, 0.35),
                0 0 0 1px rgba(255,255,255,0.2);
        }

        .quiz-options {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
            margin: 24px 0;
        }

        .quiz-option {
            background: linear-gradient(135deg, #4facfe, #00f2fe);
            color: white;
            border: none;
            border-radius: 12px;
            padding: 16px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(79, 172, 254, 0.3);
        }

        .quiz-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(79, 172, 254, 0.4);
        }

        .quiz-option.correct {
            background: linear-gradient(135deg, #11998e, #38ef7d);
            animation: pulse-success 0.6s ease;
        }

        .quiz-option.incorrect {
            background: linear-gradient(135deg, #ff416c, #ff4b2b);
            animation: shake 0.6s ease;
        }

        .progress-section {
            background: rgba(255,255,255,0.9);
            border-radius: 16px;
            padding: 20px;
            margin: 16px 0;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }

        .progress-bar-container {
            background: rgba(236, 240, 241, 0.8);
            border-radius: 12px;
            height: 16px;
            margin: 12px 0;
            overflow: hidden;
            position: relative;
        }

        .progress-bar {
            background: linear-gradient(90deg, #11998e, #38ef7d);
            height: 100%;
            width: 0%;
            transition: width 0.8s ease;
            border-radius: 12px;
        }

        .achievements {
            display: flex;
            gap: 12px;
            margin: 16px 0;
            flex-wrap: wrap;
            justify-content: center;
        }

        .achievement {
            background: linear-gradient(135deg, #ffd89b, #19547b);
            color: white;
            padding: 12px 16px;
            border-radius: 20px;
            font-weight: 600;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 14px;
        }

        .mode-selector {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
            gap: 16px;
            margin: 24px 0;
        }

        .mode-card {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-radius: 16px;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 10px 25px rgba(102, 126, 234, 0.3);
        }

        .mode-card:hover {
            transform: translateY(-3px) scale(1.02);
            box-shadow: 0 15px 35px rgba(102, 126, 234, 0.4);
        }

        .mode-icon {
            font-size: 36px;
            margin-bottom: 12px;
        }

        .mode-title {
            font-size: 16px;
            font-weight: 700;
            margin-bottom: 8px;
        }

        .mode-description {
            font-size: 12px;
            opacity: 0.9;
        }

        .hidden {
            display: none;
        }

        .loading {
            text-align: center;
            color: #6c757d;
            font-size: 16px;
            padding: 16px;
        }

        @keyframes pulse-success {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
            20%, 40%, 60%, 80% { transform: translateX(5px); }
        }

        /* Telegram-specific optimizations */
        @media (max-width: 480px) {
            .container {
                margin: 5px;
                padding: 15px;
                border-radius: 16px;
            }
            
            .title {
                font-size: 28px;
            }
            
            .nav-tabs {
                gap: 4px;
                padding: 4px;
            }
            
            .nav-tab {
                padding: 6px 8px;
                font-size: 11px;
            }
            
            .quiz-options {
                grid-template-columns: 1fr;
            }
            
            .categories-grid {
                grid-template-columns: 1fr;
            }
            
            .stats-grid {
                grid-template-columns: repeat(2, 1fr);
                gap: 12px;
            }
            
            .mode-selector {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .daily-tasks {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .word-emoji {
                font-size: 60px;
            }
            
            .word-english {
                font-size: 28px;
            }
            
            .action-buttons {
                gap: 8px;
            }
            
            .action-btn {
                padding: 10px 16px;
                font-size: 13px;
            }
        }

        /* Fix for Telegram viewport */
        html, body {
            height: 100%;
            overflow-x: hidden;
        }
        
        body {
            padding-bottom: env(safe-area-inset-bottom);
        }
    </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <div class="container">
   <div class="header">
    <h1 class="title" id="app-title">English Learning Adventure</h1>
    <p class="subtitle" id="welcome-message">Давайте изучать английский вместе!</p>
   </div>
   <div class="daily-goal">
    <div class="daily-goal-text" id="daily-goal">
     Ежедневная цель: Изучить 5 новых слов
    </div>
    <div class="save-info"><span>💾</span> <span>Прогресс сохраняется автоматически</span> <span id="save-indicator" style="display: none;">Сохранено</span>
    </div>
   </div>
   <div class="nav-tabs"><button class="nav-tab active" onclick="showSection('dashboard')">📊 Главная</button> <button class="nav-tab" onclick="showSection('categories')">📚 Категории</button> <button class="nav-tab" onclick="showSection('learn')">🎓 Изучение</button> <button class="nav-tab" onclick="showSection('quiz')">🎯 Викторина</button> <button class="nav-tab" onclick="showSection('sentences')">📝 Предложения</button> <button class="nav-tab" onclick="showSection('listening')">🎧 Аудирование</button> <button class="nav-tab" onclick="showSection('speaking')">🗣️ Говорение</button> <button class="nav-tab" onclick="showSection('favorites')">⭐ Избранное</button> <button class="nav-tab" onclick="showSection('study-plan')">📅 План обучения</button>
   </div><!-- Dashboard Section -->
   <div id="dashboard" class="content-section active">
    <div class="stats-grid">
     <div class="stat-card">
      <div class="stat-number" id="total-words">
       0
      </div>
      <div class="stat-label">
       Всего слов
      </div>
     </div>
     <div class="stat-card">
      <div class="stat-number" id="learned-words">
       0
      </div>
      <div class="stat-label">
       Изучено
      </div>
     </div>
     <div class="stat-card">
      <div class="stat-number" id="accuracy-rate">
       0%
      </div>
      <div class="stat-label">
       Точность
      </div>
     </div>
     <div class="stat-card">
      <div class="stat-number" id="streak-days">
       0
      </div>
      <div class="stat-label">
       Дней подряд
      </div>
     </div>
    </div>
    <div class="achievements">
     <div class="achievement">
      🏆 Первые шаги
     </div>
     <div class="achievement">
      🌟 Знаток животных
     </div>
     <div class="achievement">
      🎯 Меткий стрелок
     </div>
    </div>
    <div class="mode-selector">
     <div class="mode-card" onclick="showSection('learn')">
      <div class="mode-icon">
       🎓
      </div>
      <div class="mode-title">
       Изучение
      </div>
      <div class="mode-description">
       Изучайте новые слова с примерами
      </div>
     </div>
     <div class="mode-card" onclick="showSection('quiz')">
      <div class="mode-icon">
       🎯
      </div>
      <div class="mode-title">
       Викторина
      </div>
      <div class="mode-description">
       Проверьте свои знания
      </div>
     </div>
     <div class="mode-card" onclick="showSection('sentences')">
      <div class="mode-icon">
       📝
      </div>
      <div class="mode-title">
       Предложения
      </div>
      <div class="mode-description">
       Составляйте предложения
      </div>
     </div>
     <div class="mode-card" onclick="showSection('listening')">
      <div class="mode-icon">
       🎧
      </div>
      <div class="mode-title">
       Аудирование
      </div>
      <div class="mode-description">
       Тренируйте восприятие на слух
      </div>
     </div>
     <div class="mode-card" onclick="showSection('speaking')">
      <div class="mode-icon">
       🗣️
      </div>
      <div class="mode-title">
       Говорение
      </div>
      <div class="mode-description">
       Улучшайте произношение
      </div>
     </div>
     <div class="mode-card" onclick="showSection('categories')">
      <div class="mode-icon">
       📚
      </div>
      <div class="mode-title">
       Категории
      </div>
      <div class="mode-description">
       Изучайте по темам
      </div>
     </div>
    </div>
   </div><!-- Categories Section -->
   <div id="categories" class="content-section">
    <div class="categories-grid" id="categories-grid"><!-- Categories will be generated here -->
    </div>
   </div><!-- Learn Section -->
   <div id="learn" class="content-section">
    <div class="progress-section">
     <h3>Прогресс изучения</h3>
     <div class="progress-bar-container">
      <div class="progress-bar" id="learn-progress"></div>
     </div>
     <p id="learn-progress-text">0 из 0 слов изучено</p>
    </div>
    <div class="word-card" id="learn-word-card">
     <div class="word-emoji" id="learn-emoji">
      🍎
     </div>
     <div class="word-english" id="learn-english">
      Apple
     </div>
     <div class="word-russian" id="learn-russian">
      Яблоко
     </div>
     <div class="word-pronunciation" id="learn-pronunciation">
      [ˈæpəl]
     </div>
     <div class="action-buttons"><button class="action-btn speak" onclick="speakWord()"> 🔊 Произнести </button> <button class="action-btn favorite" onclick="toggleFavorite()"> ⭐ В избранное </button> <button class="action-btn" onclick="nextLearnWord()"> ➡️ Следующее </button>
     </div>
    </div>
   </div><!-- Quiz Section -->
   <div id="quiz" class="content-section">
    <div class="progress-section">
     <h3>Викторина</h3>
     <div id="quiz-score">
      Счёт: 0/0
     </div>
     <div class="progress-bar-container">
      <div class="progress-bar" id="quiz-progress"></div>
     </div>
    </div>
    <div class="word-card" id="quiz-word-card">
     <div class="word-emoji" id="quiz-emoji">
      🍎
     </div>
     <div class="word-english" id="quiz-english">
      Apple
     </div>
     <div class="quiz-options" id="quiz-options"><!-- Quiz options will be generated here -->
     </div>
     <div class="action-buttons"><button class="action-btn" onclick="startQuiz()">🎯 Начать викторину</button> <button class="action-btn" onclick="nextQuizWord()" id="next-quiz-btn" style="display: none;">➡️ Следующий вопрос</button>
     </div>
    </div>
   </div><!-- Sentences Section -->
   <div id="sentences" class="content-section">
    <div class="sentence-builder">
     <h3>Составьте предложение</h3>
     <div class="word-bank" id="word-bank"><!-- Word tokens will be generated here -->
     </div>
     <div class="sentence-area" id="sentence-area">
      <p style="color: #95a5a6;">Перетащите слова сюда</p>
     </div>
     <div class="action-buttons"><button class="action-btn" onclick="checkSentence()">✅ Проверить</button> <button class="action-btn" onclick="clearSentence()">🗑️ Очистить</button> <button class="action-btn" onclick="newSentenceExercise()">🔄 Новое упражнение</button>
     </div>
    </div>
   </div><!-- Favorites Section -->
   <div id="favorites" class="content-section">
    <div id="favorites-list"><!-- Favorite words will be displayed here -->
    </div>
   </div><!-- Listening Section -->
   <div id="listening" class="content-section">
    <div class="progress-section">
     <h3>🎧 Тренировка аудирования</h3>
     <p style="color: #64748b; margin: 15px 0;">Слушайте слова и выбирайте правильный перевод</p>
     <div id="listening-score">
      Счёт: 0/0
     </div>
    </div>
    <div class="word-card" id="listening-card">
     <div class="word-emoji" id="listening-emoji">
      🎧
     </div>
     <div style="font-size: 20px; color: #64748b; margin: 16px 0;">
      Нажмите кнопку, чтобы прослушать слово
     </div>
     <div class="action-buttons" style="margin: 24px 0;"><button class="action-btn speak" onclick="playListeningWord()" id="play-listening-btn"> 🔊 Прослушать слово </button> <button class="action-btn" onclick="repeatListening()" id="repeat-listening-btn" style="display: none;"> 🔄 Повторить </button>
     </div>
     <div class="quiz-options" id="listening-options" style="display: none;"><!-- Listening options will be generated here -->
     </div>
     <div class="action-buttons"><button class="action-btn" onclick="startListening()">🎧 Начать тренировку</button> <button class="action-btn" onclick="nextListeningWord()" id="next-listening-btn" style="display: none;">➡️ Следующее слово</button>
     </div>
    </div>
   </div><!-- Speaking Section -->
   <div id="speaking" class="content-section">
    <div class="progress-section">
     <h3>🗣️ Тренировка произношения</h3>
     <p style="color: #64748b; margin: 15px 0;">Повторяйте слова и улучшайте произношение</p>
     <div id="speaking-score">
      Слов произнесено: 0
     </div>
    </div>
    <div class="word-card" id="speaking-card">
     <div class="word-emoji" id="speaking-emoji">
      🗣️
     </div>
     <div class="word-english" id="speaking-english">
      Speak
     </div>
     <div class="word-russian" id="speaking-russian">
      Говорить
     </div>
     <div class="word-pronunciation" id="speaking-pronunciation">
      [spiːk]
     </div>
     <div style="background: rgba(79, 172, 254, 0.1); border-radius: 12px; padding: 16px; margin: 16px 0; border-left: 4px solid #4facfe;">
      <div style="font-size: 16px; color: #2C3E50; margin-bottom: 10px; font-weight: 600;">
       🎯 Инструкция:
      </div>
      <div style="font-size: 14px; color: #64748b; line-height: 1.5;">
       1. Нажмите "Прослушать" чтобы услышать правильное произношение<br>
        2. Нажмите "Записать" и произнесите слово<br>
        3. Сравните своё произношение с оригиналом
      </div>
     </div>
     <div class="action-buttons"><button class="action-btn speak" onclick="playOriginalWord()"> 🔊 Прослушать оригинал </button> <button class="action-btn" onclick="startRecording()" id="record-btn"> 🎤 Записать произношение </button> <button class="action-btn" onclick="nextSpeakingWord()"> ➡️ Следующее слово </button>
     </div>
     <div id="recording-status" style="display: none; text-align: center; margin: 16px 0; padding: 12px; background: rgba(240, 147, 251, 0.1); border-radius: 10px; color: #f093fb; font-weight: 600;">
      🎤 Запись... Произнесите слово чётко
     </div>
    </div>
   </div><!-- Study Plan Section -->
   <div id="study-plan" class="content-section">
    <div class="progress-section">
     <h3>📅 Трёхмесячный план изучения английского (12 недель)</h3>
     <div class="progress-bar-container">
      <div class="progress-bar" id="month-progress"></div>
     </div>
     <p id="month-progress-text">День 1 из 30</p>
    </div>
    <div class="study-plan-container">
     <div class="week-selector"><button class="week-btn active" onclick="showWeek(1)">Неделя 1</button> <button class="week-btn" onclick="showWeek(2)">Неделя 2</button> <button class="week-btn" onclick="showWeek(3)">Неделя 3</button> <button class="week-btn" onclick="showWeek(4)">Неделя 4</button> <button class="week-btn" onclick="showWeek(5)">Неделя 5</button> <button class="week-btn" onclick="showWeek(6)">Неделя 6</button> <button class="week-btn" onclick="showWeek(7)">Неделя 7</button> <button class="week-btn" onclick="showWeek(8)">Неделя 8</button> <button class="week-btn" onclick="showWeek(9)">Неделя 9</button> <button class="week-btn" onclick="showWeek(10)">Неделя 10</button> <button class="week-btn" onclick="showWeek(11)">Неделя 11</button> <button class="week-btn" onclick="showWeek(12)">Неделя 12</button>
     </div>
     <div id="week-content"><!-- Week content will be generated here -->
     </div>
     <div class="daily-tasks" id="daily-tasks"><!-- Daily tasks will be shown here -->
     </div>
    </div>
   </div>
   <div id="loading" class="loading hidden">
    Загрузка... ⏳
   </div><!-- Creator Footer -->
   <div style="text-align: center; margin-top: 30px; padding: 16px; border-top: 2px solid rgba(255,255,255,0.2); background: rgba(255,255,255,0.1); backdrop-filter: blur(10px); border-radius: 0 0 20px 20px;">
    <div style="font-size: 12px; color: #64748b; margin-bottom: 6px;">
     Created with ❤️ by <strong style="color: #667eea;">RRN</strong> • 2025
    </div>
    <div style="font-size: 11px; color: #94a3b8;">
     Follow us: <a href="https://t.me/RRNCLUB" target="_blank" rel="noopener noreferrer" style="color: #667eea; text-decoration: none; font-weight: 600; transition: color 0.3s ease;" onmouseover="this.style.color='#764ba2'" onmouseout="this.style.color='#667eea'">@RRNCLUB</a>
    </div>
   </div>
  </div>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <script>
        // Telegram Web App initialization
        let tg = window.Telegram?.WebApp;
        if (tg) {
            tg.ready();
            tg.expand();
            tg.MainButton.hide();
            
            // Set theme colors
            if (tg.themeParams) {
                document.documentElement.style.setProperty('--tg-theme-bg-color', tg.themeParams.bg_color || '#ffffff');
                document.documentElement.style.setProperty('--tg-theme-text-color', tg.themeParams.text_color || '#000000');
            }
        }

        // Default configuration
        const defaultConfig = {
            app_title: "English Learning Adventure",
            welcome_message: "Давайте изучать английский вместе!",
            daily_goal: "Ежедневная цель: Изучить 5 новых слов",
            background_color: "#667eea",
            surface_color: "#ffffff",
            text_color: "#2C3E50",
            primary_action_color: "#4facfe",
            secondary_action_color: "#667eea",
            font_family: "Comic Sans MS",
            font_size: 16
        };

        // Enhanced word database with categories and pronunciations
        const wordCategories = {
            animals: {
                name: "Животные",
                emoji: "🐾",
                words: [
                    { emoji: "🐱", english: "Cat", russian: "Кот", pronunciation: "[kæt]", example: "The cat is sleeping on the sofa", translation: "Кот спит на диване" },
                    { emoji: "🐶", english: "Dog", russian: "Собака", pronunciation: "[dɔːɡ]", example: "My dog loves to play fetch", translation: "Моя собака любит играть в апорт" },
                    { emoji: "🐘", english: "Elephant", russian: "Слон", pronunciation: "[ˈeləfənt]", example: "The elephant is very big", translation: "Слон очень большой" },
                    { emoji: "🦁", english: "Lion", russian: "Лев", pronunciation: "[ˈlaɪən]", example: "The lion is the king of animals", translation: "Лев - король зверей" },
                    { emoji: "🐸", english: "Frog", russian: "Лягушка", pronunciation: "[frɔːɡ]", example: "The frog jumps in the pond", translation: "Лягушка прыгает в пруду" },
                    { emoji: "🐰", english: "Rabbit", russian: "Кролик", pronunciation: "[ˈræbɪt]", example: "The rabbit eats carrots", translation: "Кролик ест морковь" },
                    { emoji: "🐻", english: "Bear", russian: "Медведь", pronunciation: "[ber]", example: "The bear lives in the forest", translation: "Медведь живёт в лесу" },
                    { emoji: "🐺", english: "Wolf", russian: "Волк", pronunciation: "[wʊlf]", example: "The wolf howls at the moon", translation: "Волк воет на луну" },
                    { emoji: "🦊", english: "Fox", russian: "Лиса", pronunciation: "[fɑːks]", example: "The fox is very clever", translation: "Лиса очень умная" },
                    { emoji: "🐯", english: "Tiger", russian: "Тигр", pronunciation: "[ˈtaɪɡər]", example: "The tiger has orange stripes", translation: "У тигра оранжевые полосы" },
                    { emoji: "🐵", english: "Monkey", russian: "Обезьяна", pronunciation: "[ˈmʌŋki]", example: "The monkey swings on trees", translation: "Обезьяна качается на деревьях" },
                    { emoji: "🐧", english: "Penguin", russian: "Пингвин", pronunciation: "[ˈpeŋɡwɪn]", example: "Penguins live in Antarctica", translation: "Пингвины живут в Антарктиде" },
                    { emoji: "🦅", english: "Eagle", russian: "Орёл", pronunciation: "[ˈiːɡəl]", example: "The eagle flies high in the sky", translation: "Орёл летает высоко в небе" },
                    { emoji: "🐢", english: "Turtle", russian: "Черепаха", pronunciation: "[ˈtɜːrtəl]", example: "The turtle moves very slowly", translation: "Черепаха движется очень медленно" },
                    { emoji: "🦋", english: "Butterfly", russian: "Бабочка", pronunciation: "[ˈbʌtərflaɪ]", example: "The butterfly has colorful wings", translation: "У бабочки разноцветные крылья" }
                ]
            },
            food: {
                name: "Еда",
                emoji: "🍎",
                words: [
                    { emoji: "🍎", english: "Apple", russian: "Яблоко", pronunciation: "[ˈæpəl]", example: "I eat an apple every day", translation: "Я ем яблоко каждый день" },
                    { emoji: "🍌", english: "Banana", russian: "Банан", pronunciation: "[bəˈnænə]", example: "Bananas are yellow and sweet", translation: "Бананы жёлтые и сладкие" },
                    { emoji: "🍞", english: "Bread", russian: "Хлеб", pronunciation: "[bred]", example: "We buy fresh bread every morning", translation: "Мы покупаем свежий хлеб каждое утро" },
                    { emoji: "🥛", english: "Milk", russian: "Молоко", pronunciation: "[mɪlk]", example: "Children drink milk for breakfast", translation: "Дети пьют молоко на завтрак" },
                    { emoji: "🍪", english: "Cookie", russian: "Печенье", pronunciation: "[ˈkʊki]", example: "My grandmother bakes delicious cookies", translation: "Моя бабушка печёт вкусное печенье" },
                    { emoji: "🍕", english: "Pizza", russian: "Пицца", pronunciation: "[ˈpiːtsə]", example: "We order pizza for dinner", translation: "Мы заказываем пиццу на ужин" },
                    { emoji: "🍊", english: "Orange", russian: "Апельсин", pronunciation: "[ˈɔːrɪndʒ]", example: "Oranges are full of vitamin C", translation: "Апельсины богаты витамином C" },
                    { emoji: "🍇", english: "Grapes", russian: "Виноград", pronunciation: "[ɡreɪps]", example: "Purple grapes are very sweet", translation: "Фиолетовый виноград очень сладкий" },
                    { emoji: "🍓", english: "Strawberry", russian: "Клубника", pronunciation: "[ˈstrɔːberi]", example: "Strawberries taste best in summer", translation: "Клубника вкуснее всего летом" },
                    { emoji: "🥕", english: "Carrot", russian: "Морковь", pronunciation: "[ˈkærət]", example: "Carrots are good for your eyes", translation: "Морковь полезна для глаз" },
                    { emoji: "🍅", english: "Tomato", russian: "Помидор", pronunciation: "[təˈmeɪtoʊ]", example: "Tomatoes are red and juicy", translation: "Помидоры красные и сочные" },
                    { emoji: "🥔", english: "Potato", russian: "Картофель", pronunciation: "[pəˈteɪtoʊ]", example: "We cook potatoes for dinner", translation: "Мы готовим картофель на ужин" },
                    { emoji: "🧄", english: "Garlic", russian: "Чеснок", pronunciation: "[ˈɡɑːrlɪk]", example: "Garlic makes food taste better", translation: "Чеснок делает еду вкуснее" },
                    { emoji: "🧅", english: "Onion", russian: "Лук", pronunciation: "[ˈʌnjən]", example: "Cutting onions makes me cry", translation: "Резка лука заставляет меня плакать" },
                    { emoji: "🥒", english: "Cucumber", russian: "Огурец", pronunciation: "[ˈkjuːkʌmbər]", example: "Cucumbers are fresh and crunchy", translation: "Огурцы свежие и хрустящие" }
                ]
            },
            colors: {
                name: "Цвета",
                emoji: "🌈",
                words: [
                    { emoji: "🔴", english: "Red", russian: "Красный", pronunciation: "[red]" },
                    { emoji: "🔵", english: "Blue", russian: "Синий", pronunciation: "[bluː]" },
                    { emoji: "🟢", english: "Green", russian: "Зелёный", pronunciation: "[ɡriːn]" },
                    { emoji: "🟡", english: "Yellow", russian: "Жёлтый", pronunciation: "[ˈjeloʊ]" },
                    { emoji: "🟣", english: "Purple", russian: "Фиолетовый", pronunciation: "[ˈpɜːrpəl]" },
                    { emoji: "🟠", english: "Orange", russian: "Оранжевый", pronunciation: "[ˈɔːrɪndʒ]" },
                    { emoji: "⚫", english: "Black", russian: "Чёрный", pronunciation: "[blæk]" },
                    { emoji: "⚪", english: "White", russian: "Белый", pronunciation: "[waɪt]" },
                    { emoji: "🟤", english: "Brown", russian: "Коричневый", pronunciation: "[braʊn]" },
                    { emoji: "🩶", english: "Gray", russian: "Серый", pronunciation: "[ɡreɪ]" }
                ]
            },
            family: {
                name: "Семья",
                emoji: "👨‍👩‍👧‍👦",
                words: [
                    { emoji: "👨", english: "Father", russian: "Отец", pronunciation: "[ˈfɑːðər]" },
                    { emoji: "👩", english: "Mother", russian: "Мать", pronunciation: "[ˈmʌðər]" },
                    { emoji: "👦", english: "Son", russian: "Сын", pronunciation: "[sʌn]" },
                    { emoji: "👧", english: "Daughter", russian: "Дочь", pronunciation: "[ˈdɔːtər]" },
                    { emoji: "👴", english: "Grandfather", russian: "Дедушка", pronunciation: "[ˈɡrænfɑːðər]" },
                    { emoji: "👵", english: "Grandmother", russian: "Бабушка", pronunciation: "[ˈɡrænmʌðər]" },
                    { emoji: "👶", english: "Baby", russian: "Малыш", pronunciation: "[ˈbeɪbi]" },
                    { emoji: "👫", english: "Brother", russian: "Брат", pronunciation: "[ˈbrʌðər]" },
                    { emoji: "👭", english: "Sister", russian: "Сестра", pronunciation: "[ˈsɪstər]" },
                    { emoji: "👨‍👩‍👧‍👦", english: "Family", russian: "Семья", pronunciation: "[ˈfæməli]" }
                ]
            },
            body: {
                name: "Тело",
                emoji: "👤",
                words: [
                    { emoji: "👁️", english: "Eye", russian: "Глаз", pronunciation: "[aɪ]" },
                    { emoji: "👂", english: "Ear", russian: "Ухо", pronunciation: "[ɪr]" },
                    { emoji: "👃", english: "Nose", russian: "Нос", pronunciation: "[noʊz]" },
                    { emoji: "👄", english: "Mouth", russian: "Рот", pronunciation: "[maʊθ]" },
                    { emoji: "🦷", english: "Tooth", russian: "Зуб", pronunciation: "[tuːθ]" },
                    { emoji: "✋", english: "Hand", russian: "Рука", pronunciation: "[hænd]" },
                    { emoji: "🦵", english: "Leg", russian: "Нога", pronunciation: "[leɡ]" },
                    { emoji: "🦶", english: "Foot", russian: "Стопа", pronunciation: "[fʊt]" },
                    { emoji: "💪", english: "Arm", russian: "Рука", pronunciation: "[ɑːrm]" },
                    { emoji: "🧠", english: "Head", russian: "Голова", pronunciation: "[hed]" }
                ]
            },
            clothes: {
                name: "Одежда",
                emoji: "👕",
                words: [
                    { emoji: "👕", english: "Shirt", russian: "Рубашка", pronunciation: "[ʃɜːrt]" },
                    { emoji: "👖", english: "Pants", russian: "Штаны", pronunciation: "[pænts]" },
                    { emoji: "👗", english: "Dress", russian: "Платье", pronunciation: "[dres]" },
                    { emoji: "👟", english: "Shoes", russian: "Обувь", pronunciation: "[ʃuːz]" },
                    { emoji: "🧥", english: "Jacket", russian: "Куртка", pronunciation: "[ˈdʒækɪt]" },
                    { emoji: "👒", english: "Hat", russian: "Шляпа", pronunciation: "[hæt]" },
                    { emoji: "🧦", english: "Socks", russian: "Носки", pronunciation: "[sɑːks]" },
                    { emoji: "👔", english: "Tie", russian: "Галстук", pronunciation: "[taɪ]" },
                    { emoji: "👚", english: "Blouse", russian: "Блузка", pronunciation: "[blaʊs]" },
                    { emoji: "🩳", english: "Shorts", russian: "Шорты", pronunciation: "[ʃɔːrts]" }
                ]
            },
            transport: {
                name: "Транспорт",
                emoji: "🚗",
                words: [
                    { emoji: "🚗", english: "Car", russian: "Машина", pronunciation: "[kɑːr]" },
                    { emoji: "🚌", english: "Bus", russian: "Автобус", pronunciation: "[bʌs]" },
                    { emoji: "🚂", english: "Train", russian: "Поезд", pronunciation: "[treɪn]" },
                    { emoji: "✈️", english: "Plane", russian: "Самолёт", pronunciation: "[pleɪn]" },
                    { emoji: "🚲", english: "Bicycle", russian: "Велосипед", pronunciation: "[ˈbaɪsɪkəl]" },
                    { emoji: "🛵", english: "Motorcycle", russian: "Мотоцикл", pronunciation: "[ˈmoʊtərˌsaɪkəl]" },
                    { emoji: "🚁", english: "Helicopter", russian: "Вертолёт", pronunciation: "[ˈhelɪkɑːptər]" },
                    { emoji: "🚢", english: "Ship", russian: "Корабль", pronunciation: "[ʃɪp]" },
                    { emoji: "🚕", english: "Taxi", russian: "Такси", pronunciation: "[ˈtæksi]" },
                    { emoji: "🚐", english: "Van", russian: "Фургон", pronunciation: "[væn]" }
                ]
            },
            house: {
                name: "Дом",
                emoji: "🏠",
                words: [
                    { emoji: "🏠", english: "House", russian: "Дом", pronunciation: "[haʊs]" },
                    { emoji: "🚪", english: "Door", russian: "Дверь", pronunciation: "[dɔːr]" },
                    { emoji: "🪟", english: "Window", russian: "Окно", pronunciation: "[ˈwɪndoʊ]" },
                    { emoji: "🛏️", english: "Bed", russian: "Кровать", pronunciation: "[bed]" },
                    { emoji: "🪑", english: "Chair", russian: "Стул", pronunciation: "[tʃer]" },
                    { emoji: "🛋️", english: "Sofa", russian: "Диван", pronunciation: "[ˈsoʊfə]" },
                    { emoji: "📺", english: "TV", russian: "Телевизор", pronunciation: "[ˌtiːˈviː]" },
                    { emoji: "🍽️", english: "Table", russian: "Стол", pronunciation: "[ˈteɪbəl]" },
                    { emoji: "🚿", english: "Shower", russian: "Душ", pronunciation: "[ˈʃaʊər]" },
                    { emoji: "🔥", english: "Kitchen", russian: "Кухня", pronunciation: "[ˈkɪtʃən]" }
                ]
            },
            weather: {
                name: "Погода",
                emoji: "🌤️",
                words: [
                    { emoji: "☀️", english: "Sun", russian: "Солнце", pronunciation: "[sʌn]" },
                    { emoji: "🌧️", english: "Rain", russian: "Дождь", pronunciation: "[reɪn]" },
                    { emoji: "❄️", english: "Snow", russian: "Снег", pronunciation: "[snoʊ]" },
                    { emoji: "☁️", english: "Cloud", russian: "Облако", pronunciation: "[klaʊd]" },
                    { emoji: "💨", english: "Wind", russian: "Ветер", pronunciation: "[wɪnd]" },
                    { emoji: "⛈️", english: "Storm", russian: "Буря", pronunciation: "[stɔːrm]" },
                    { emoji: "🌈", english: "Rainbow", russian: "Радуга", pronunciation: "[ˈreɪnboʊ]" },
                    { emoji: "🌡️", english: "Hot", russian: "Жарко", pronunciation: "[hɑːt]" },
                    { emoji: "🧊", english: "Cold", russian: "Холодно", pronunciation: "[koʊld]" },
                    { emoji: "🌫️", english: "Fog", russian: "Туман", pronunciation: "[fɔːɡ]" }
                ]
            },
            school: {
                name: "Школа",
                emoji: "🎓",
                words: [
                    { emoji: "🏫", english: "School", russian: "Школа", pronunciation: "[skuːl]" },
                    { emoji: "👨‍🏫", english: "Teacher", russian: "Учитель", pronunciation: "[ˈtiːtʃər]" },
                    { emoji: "👨‍🎓", english: "Student", russian: "Ученик", pronunciation: "[ˈstuːdənt]" },
                    { emoji: "📚", english: "Book", russian: "Книга", pronunciation: "[bʊk]" },
                    { emoji: "✏️", english: "Pencil", russian: "Карандаш", pronunciation: "[ˈpensəl]" },
                    { emoji: "📝", english: "Paper", russian: "Бумага", pronunciation: "[ˈpeɪpər]" },
                    { emoji: "🖥️", english: "Computer", russian: "Компьютер", pronunciation: "[kəmˈpjuːtər]" },
                    { emoji: "📐", english: "Ruler", russian: "Линейка", pronunciation: "[ˈruːlər]" },
                    { emoji: "🎒", english: "Backpack", russian: "Рюкзак", pronunciation: "[ˈbækpæk]" },
                    { emoji: "🔔", english: "Bell", russian: "Звонок", pronunciation: "[bel]" }
                ]
            },
            sports: {
                name: "Спорт",
                emoji: "⚽",
                words: [
                    { emoji: "⚽", english: "Football", russian: "Футбол", pronunciation: "[ˈfʊtbɔːl]" },
                    { emoji: "🏀", english: "Basketball", russian: "Баскетбол", pronunciation: "[ˈbæskɪtbɔːl]" },
                    { emoji: "🎾", english: "Tennis", russian: "Теннис", pronunciation: "[ˈtenɪs]" },
                    { emoji: "🏐", english: "Volleyball", russian: "Волейбол", pronunciation: "[ˈvɑːlibɔːl]" },
                    { emoji: "🏊‍♂️", english: "Swimming", russian: "Плавание", pronunciation: "[ˈswɪmɪŋ]" },
                    { emoji: "🏃‍♂️", english: "Running", russian: "Бег", pronunciation: "[ˈrʌnɪŋ]" },
                    { emoji: "🚴‍♂️", english: "Cycling", russian: "Велоспорт", pronunciation: "[ˈsaɪklɪŋ]" },
                    { emoji: "🥊", english: "Boxing", russian: "Бокс", pronunciation: "[ˈbɑːksɪŋ]" },
                    { emoji: "⛷️", english: "Skiing", russian: "Лыжи", pronunciation: "[ˈskiːɪŋ]" },
                    { emoji: "🏋️‍♂️", english: "Gym", russian: "Спортзал", pronunciation: "[dʒɪm]" }
                ]
            },
            nature: {
                name: "Природа",
                emoji: "🌳",
                words: [
                    { emoji: "🌳", english: "Tree", russian: "Дерево", pronunciation: "[triː]", example: "The tree gives us shade", translation: "Дерево даёт нам тень" },
                    { emoji: "🌸", english: "Flower", russian: "Цветок", pronunciation: "[ˈflaʊər]", example: "The flower smells beautiful", translation: "Цветок красиво пахнет" },
                    { emoji: "🌿", english: "Grass", russian: "Трава", pronunciation: "[ɡræs]", example: "The grass is green and soft", translation: "Трава зелёная и мягкая" },
                    { emoji: "🏔️", english: "Mountain", russian: "Гора", pronunciation: "[ˈmaʊntən]", example: "The mountain is very high", translation: "Гора очень высокая" },
                    { emoji: "🌊", english: "Ocean", russian: "Океан", pronunciation: "[ˈoʊʃən]", example: "The ocean is deep and blue", translation: "Океан глубокий и синий" },
                    { emoji: "🏞️", english: "River", russian: "Река", pronunciation: "[ˈrɪvər]", example: "Fish swim in the river", translation: "Рыбы плавают в реке" },
                    { emoji: "🌙", english: "Moon", russian: "Луна", pronunciation: "[muːn]", example: "The moon shines at night", translation: "Луна светит ночью" },
                    { emoji: "⭐", english: "Star", russian: "Звезда", pronunciation: "[stɑːr]", example: "Stars twinkle in the sky", translation: "Звёзды мерцают в небе" },
                    { emoji: "🌍", english: "Earth", russian: "Земля", pronunciation: "[ɜːrθ]", example: "Earth is our home planet", translation: "Земля - наша родная планета" },
                    { emoji: "🔥", english: "Fire", russian: "Огонь", pronunciation: "[ˈfaɪər]", example: "Fire keeps us warm", translation: "Огонь согревает нас" }
                ]
            },
            numbers: {
                name: "Числа",
                emoji: "🔢",
                words: [
                    { emoji: "1️⃣", english: "One", russian: "Один", pronunciation: "[wʌn]", example: "I have one apple", translation: "У меня одно яблоко" },
                    { emoji: "2️⃣", english: "Two", russian: "Два", pronunciation: "[tuː]", example: "Two cats are playing", translation: "Две кошки играют" },
                    { emoji: "3️⃣", english: "Three", russian: "Три", pronunciation: "[θriː]", example: "Three birds in the tree", translation: "Три птицы на дереве" },
                    { emoji: "4️⃣", english: "Four", russian: "Четыре", pronunciation: "[fɔːr]", example: "Four wheels on a car", translation: "Четыре колеса у машины" },
                    { emoji: "5️⃣", english: "Five", russian: "Пять", pronunciation: "[faɪv]", example: "Five fingers on my hand", translation: "Пять пальцев на моей руке" },
                    { emoji: "6️⃣", english: "Six", russian: "Шесть", pronunciation: "[sɪks]", example: "Six eggs in the box", translation: "Шесть яиц в коробке" },
                    { emoji: "7️⃣", english: "Seven", russian: "Семь", pronunciation: "[ˈsevən]", example: "Seven days in a week", translation: "Семь дней в неделе" },
                    { emoji: "8️⃣", english: "Eight", russian: "Восемь", pronunciation: "[eɪt]", example: "Eight legs on a spider", translation: "Восемь ног у паука" },
                    { emoji: "9️⃣", english: "Nine", russian: "Девять", pronunciation: "[naɪn]", example: "Nine planets in our system", translation: "Девять планет в нашей системе" },
                    { emoji: "🔟", english: "Ten", russian: "Десять", pronunciation: "[ten]", example: "Ten toes on my feet", translation: "Десять пальцев на моих ногах" }
                ]
            },
            emotions: {
                name: "Эмоции",
                emoji: "😊",
                words: [
                    { emoji: "😊", english: "Happy", russian: "Счастливый", pronunciation: "[ˈhæpi]", example: "I am happy today", translation: "Я счастлив сегодня" },
                    { emoji: "😢", english: "Sad", russian: "Грустный", pronunciation: "[sæd]", example: "She feels sad", translation: "Она чувствует грусть" },
                    { emoji: "😠", english: "Angry", russian: "Злой", pronunciation: "[ˈæŋɡri]", example: "He is angry at me", translation: "Он злится на меня" },
                    { emoji: "😴", english: "Tired", russian: "Усталый", pronunciation: "[ˈtaɪərd]", example: "I am tired after work", translation: "Я устал после работы" },
                    { emoji: "😱", english: "Scared", russian: "Испуганный", pronunciation: "[skerd]", example: "The child is scared", translation: "Ребёнок испуган" },
                    { emoji: "😍", english: "Love", russian: "Любовь", pronunciation: "[lʌv]", example: "I love my family", translation: "Я люблю свою семью" },
                    { emoji: "😂", english: "Funny", russian: "Смешной", pronunciation: "[ˈfʌni]", example: "That joke is very funny", translation: "Эта шутка очень смешная" },
                    { emoji: "😌", english: "Calm", russian: "Спокойный", pronunciation: "[kɑːm]", example: "Stay calm and relax", translation: "Оставайся спокойным и расслабься" },
                    { emoji: "🤔", english: "Confused", russian: "Смущённый", pronunciation: "[kənˈfjuːzd]", example: "I am confused about this", translation: "Я смущён этим" },
                    { emoji: "😎", english: "Cool", russian: "Крутой", pronunciation: "[kuːl]", example: "That car looks cool", translation: "Эта машина выглядит круто" }
                ]
            },
            actions: {
                name: "Действия",
                emoji: "🏃‍♂️",
                words: [
                    { emoji: "🏃‍♂️", english: "Run", russian: "Бегать", pronunciation: "[rʌn]", example: "I run every morning", translation: "Я бегаю каждое утро" },
                    { emoji: "🚶‍♂️", english: "Walk", russian: "Идти", pronunciation: "[wɔːk]", example: "Let's walk to the park", translation: "Давайте пойдём в парк" },
                    { emoji: "🛌", english: "Sleep", russian: "Спать", pronunciation: "[sliːp]", example: "I sleep eight hours", translation: "Я сплю восемь часов" },
                    { emoji: "🍽️", english: "Eat", russian: "Есть", pronunciation: "[iːt]", example: "We eat dinner together", translation: "Мы ужинаем вместе" },
                    { emoji: "🥤", english: "Drink", russian: "Пить", pronunciation: "[drɪŋk]", example: "Drink more water", translation: "Пей больше воды" },
                    { emoji: "📖", english: "Read", russian: "Читать", pronunciation: "[riːd]", example: "I read books every day", translation: "Я читаю книги каждый день" },
                    { emoji: "✍️", english: "Write", russian: "Писать", pronunciation: "[raɪt]", example: "Write your name here", translation: "Напиши своё имя здесь" },
                    { emoji: "🎵", english: "Sing", russian: "Петь", pronunciation: "[sɪŋ]", example: "She sings beautifully", translation: "Она красиво поёт" },
                    { emoji: "💃", english: "Dance", russian: "Танцевать", pronunciation: "[dæns]", example: "They dance at the party", translation: "Они танцуют на вечеринке" },
                    { emoji: "🎨", english: "Draw", russian: "Рисовать", pronunciation: "[drɔː]", example: "I draw pictures", translation: "Я рисую картинки" }
                ]
            },
            time: {
                name: "Время",
                emoji: "⏰",
                words: [
                    { emoji: "🌅", english: "Morning", russian: "Утро", pronunciation: "[ˈmɔːrnɪŋ]", example: "Good morning everyone", translation: "Доброе утро всем" },
                    { emoji: "☀️", english: "Day", russian: "День", pronunciation: "[deɪ]", example: "Have a nice day", translation: "Хорошего дня" },
                    { emoji: "🌆", english: "Evening", russian: "Вечер", pronunciation: "[ˈiːvnɪŋ]", example: "Good evening friends", translation: "Добрый вечер друзья" },
                    { emoji: "🌙", english: "Night", russian: "Ночь", pronunciation: "[naɪt]", example: "Good night and sweet dreams", translation: "Спокойной ночи и сладких снов" },
                    { emoji: "📅", english: "Today", russian: "Сегодня", pronunciation: "[təˈdeɪ]", example: "Today is a beautiful day", translation: "Сегодня прекрасный день" },
                    { emoji: "📆", english: "Tomorrow", russian: "Завтра", pronunciation: "[təˈmɑːroʊ]", example: "See you tomorrow", translation: "Увидимся завтра" },
                    { emoji: "📋", english: "Yesterday", russian: "Вчера", pronunciation: "[ˈjestərdeɪ]", example: "Yesterday was fun", translation: "Вчера было весело" },
                    { emoji: "📊", english: "Week", russian: "Неделя", pronunciation: "[wiːk]", example: "This week is busy", translation: "Эта неделя занятая" },
                    { emoji: "📈", english: "Month", russian: "Месяц", pronunciation: "[mʌnθ]", example: "Next month is December", translation: "Следующий месяц - декабрь" },
                    { emoji: "🗓️", english: "Year", russian: "Год", pronunciation: "[jɪr]", example: "Happy New Year", translation: "С Новым годом" }
                ]
            }
        };

        // Game state
        let gameData = [];
        let currentSection = 'dashboard';
        let currentLearnWord = null;
        let currentQuizWord = null;
        let quizScore = 0;
        let quizTotal = 0;
        let selectedCategory = null;
        let currentSentenceWords = [];
        let userSentence = [];
        let currentWeek = 1;
        let currentDay = 1;

        // Study plan data
        const studyPlan = {
            1: {
                title: "Неделя 1: Основы - Семья и дом",
                description: "Изучаем базовые слова о семье и доме",
                words: ["family", "mother", "father", "house", "room", "kitchen", "bedroom"],
                tasks: [
                    "Изучить 5 слов о семье",
                    "Пройти викторину",
                    "Составить 3 предложения",
                    "Повторить вчерашние слова",
                    "Изучить слова о доме",
                    "Практика произношения",
                    "Итоговый тест недели"
                ]
            },
            2: {
                title: "Неделя 2: Еда и напитки",
                description: "Расширяем словарь едой и напитками",
                words: ["food", "water", "bread", "meat", "fruit", "vegetable", "drink"],
                tasks: [
                    "Изучить названия еды",
                    "Викторина о еде",
                    "Составить меню",
                    "Повторение",
                    "Напитки и десерты",
                    "Диалог в ресторане",
                    "Тест недели"
                ]
            },
            3: {
                title: "Неделя 3: Одежда и цвета",
                description: "Изучаем одежду и цвета",
                words: ["clothes", "shirt", "pants", "dress", "red", "blue", "green"],
                tasks: [
                    "Одежда: основы",
                    "Цвета радуги",
                    "Описание внешности",
                    "Повторение",
                    "Времена года и одежда",
                    "Покупки в магазине",
                    "Тест недели"
                ]
            },
            4: {
                title: "Неделя 4: Транспорт и путешествия",
                description: "Учим транспорт и путешествия",
                words: ["car", "bus", "train", "plane", "travel", "road", "station"],
                tasks: [
                    "Виды транспорта",
                    "В аэропорту",
                    "Направления",
                    "Повторение",
                    "Планирование поездки",
                    "Диалог с водителем",
                    "Тест недели"
                ]
            },
            5: {
                title: "Неделя 5: Работа и профессии",
                description: "Изучаем профессии и работу",
                words: ["work", "job", "teacher", "doctor", "office", "school", "hospital"],
                tasks: [
                    "Популярные профессии",
                    "Рабочий день",
                    "В офисе",
                    "Повторение",
                    "Собеседование",
                    "Рабочие инструменты",
                    "Тест недели"
                ]
            },
            6: {
                title: "Неделя 6: Хобби и спорт",
                description: "Говорим о хобби и спорте",
                words: ["sport", "football", "tennis", "music", "book", "game", "hobby"],
                tasks: [
                    "Виды спорта",
                    "Музыкальные инструменты",
                    "Свободное время",
                    "Повторение",
                    "Спортивные игры",
                    "Культурные мероприятия",
                    "Тест недели"
                ]
            },
            7: {
                title: "Неделя 7: Погода и природа",
                description: "Изучаем погоду и природу",
                words: ["weather", "sun", "rain", "snow", "tree", "flower", "mountain"],
                tasks: [
                    "Погодные явления",
                    "Времена года",
                    "Растения и животные",
                    "Повторение",
                    "Прогноз погоды",
                    "Экология",
                    "Тест недели"
                ]
            },
            8: {
                title: "Неделя 8: Здоровье и тело",
                description: "Учим части тела и здоровье",
                words: ["body", "head", "hand", "foot", "health", "doctor", "medicine"],
                tasks: [
                    "Части тела",
                    "У врача",
                    "Здоровый образ жизни",
                    "Повторение",
                    "Симптомы болезни",
                    "В аптеке",
                    "Тест недели"
                ]
            },
            9: {
                title: "Неделя 9: Время и календарь",
                description: "Изучаем время и даты",
                words: ["time", "hour", "minute", "day", "week", "month", "year"],
                tasks: [
                    "Который час?",
                    "Дни недели",
                    "Месяцы года",
                    "Повторение",
                    "Планирование времени",
                    "Исторические даты",
                    "Тест недели"
                ]
            },
            10: {
                title: "Неделя 10: Технологии",
                description: "Современные технологии",
                words: ["computer", "phone", "internet", "email", "website", "app", "digital"],
                tasks: [
                    "Компьютер и интернет",
                    "Мобильные технологии",
                    "Социальные сети",
                    "Повторение",
                    "Онлайн покупки",
                    "Цифровая безопасность",
                    "Тест недели"
                ]
            },
            11: {
                title: "Неделя 11: Культура и искусство",
                description: "Искусство и культура",
                words: ["art", "music", "movie", "book", "museum", "theater", "culture"],
                tasks: [
                    "Виды искусства",
                    "В музее",
                    "Литература",
                    "Повторение",
                    "Кинематограф",
                    "Культурные традиции",
                    "Тест недели"
                ]
            },
            12: {
                title: "Неделя 12: Итоговое повторение",
                description: "Закрепляем все изученное",
                words: ["review", "practice", "test", "knowledge", "skill", "progress", "success"],
                tasks: [
                    "Повторение 1-3 недель",
                    "Повторение 4-6 недель",
                    "Повторение 7-9 недель",
                    "Повторение 10-11 недель",
                    "Итоговая викторина",
                    "Финальный проект",
                    "Празднование успеха!"
                ]
            }
        };

        // Data SDK handler
        const dataHandler = {
            onDataChanged(data) {
                gameData = data;
                loadStudyProgress();
                updateDashboard();
                updateCategories();
                updateFavorites();
                updateStudyPlan();
            }
        };

        // Element SDK implementation
        async function onConfigChange(config) {
            const customFont = config.font_family || defaultConfig.font_family;
            const baseFontStack = 'cursive, sans-serif';
            const baseSize = config.font_size || defaultConfig.font_size;
            
            // Apply font family
            document.body.style.fontFamily = `${customFont}, ${baseFontStack}`;
            
            // Apply font sizes proportionally
            document.querySelector('.title').style.fontSize = `${baseSize * 2.25}px`;
            document.querySelector('.subtitle').style.fontSize = `${baseSize * 1}px`;
            
            // Apply colors
            document.body.style.background = `linear-gradient(135deg, ${config.background_color || defaultConfig.background_color}, #764ba2, #f093fb)`;
            
            // Apply text content
            document.getElementById('app-title').textContent = config.app_title || defaultConfig.app_title;
            document.getElementById('welcome-message').textContent = config.welcome_message || defaultConfig.welcome_message;
            document.getElementById('daily-goal').textContent = config.daily_goal || defaultConfig.daily_goal;
        }

        function mapToCapabilities(config) {
            return {
                recolorables: [
                    {
                        get: () => config.background_color || defaultConfig.background_color,
                        set: (value) => {
                            if (window.elementSdk) {
                                window.elementSdk.setConfig({ background_color: value });
                            }
                        }
                    }
                ],
                borderables: [],
                fontEditable: {
                    get: () => config.font_family || defaultConfig.font_family,
                    set: (value) => {
                        if (window.elementSdk) {
                            window.elementSdk.setConfig({ font_family: value });
                        }
                    }
                },
                fontSizeable: {
                    get: () => config.font_size || defaultConfig.font_size,
                    set: (value) => {
                        if (window.elementSdk) {
                            window.elementSdk.setConfig({ font_size: value });
                        }
                    }
                }
            };
        }

        function mapToEditPanelValues(config) {
            return new Map([
                ["app_title", config.app_title || defaultConfig.app_title],
                ["welcome_message", config.welcome_message || defaultConfig.welcome_message],
                ["daily_goal", config.daily_goal || defaultConfig.daily_goal]
            ]);
        }

        // Initialize SDKs
        async function initializeApp() {
            try {
                if (window.dataSdk) {
                    const initResult = await window.dataSdk.init(dataHandler);
                    if (!initResult.isOk) {
                        console.error("Failed to initialize data SDK");
                    }
                }

                if (window.elementSdk) {
                    window.elementSdk.init({
                        defaultConfig,
                        onConfigChange,
                        mapToCapabilities,
                        mapToEditPanelValues
                    });
                }

                updateCategories();
                loadRandomLearnWord();
                newSentenceExercise();
                updateStudyPlan();
            } catch (error) {
                console.error("Failed to initialize SDKs:", error);
            }
        }

        // Navigation
        function showSection(sectionName) {
            // Hide all sections
            document.querySelectorAll('.content-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all tabs
            document.querySelectorAll('.nav-tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionName).classList.add('active');
            
            // Add active class to clicked tab
            event.target.classList.add('active');
            
            currentSection = sectionName;
        }

        // Dashboard functions
        function updateDashboard() {
            const totalWords = getAllWords().length;
            const learnedWords = new Set(gameData.map(item => item.word)).size;
            const totalAttempts = gameData.reduce((sum, item) => sum + item.total_attempts, 0);
            const correctAnswers = gameData.reduce((sum, item) => sum + item.correct_answers, 0);
            const accuracy = totalAttempts > 0 ? Math.round((correctAnswers / totalAttempts) * 100) : 0;

            document.getElementById('total-words').textContent = totalWords;
            document.getElementById('learned-words').textContent = learnedWords;
            document.getElementById('accuracy-rate').textContent = accuracy + '%';
            document.getElementById('streak-days').textContent = '1'; // Simplified for demo
        }

        // Categories functions
        function updateCategories() {
            const grid = document.getElementById('categories-grid');
            grid.innerHTML = '';

            Object.entries(wordCategories).forEach(([key, category]) => {
                const categoryWords = category.words.map(w => w.english);
                const learnedInCategory = gameData.filter(item => 
                    categoryWords.includes(item.word)
                ).length;
                const progressPercent = Math.round((learnedInCategory / category.words.length) * 100);

                const card = document.createElement('div');
                card.className = 'category-card';
                card.onclick = () => selectCategory(key);
                
                // Add progress bar styling
                const progressBarHtml = `
                    <div style="width: 100%; background: rgba(236, 240, 241, 0.8); border-radius: 8px; height: 6px; margin: 12px 0; overflow: hidden;">
                        <div style="width: ${progressPercent}%; height: 100%; background: linear-gradient(90deg, #11998e, #38ef7d); border-radius: 8px; transition: width 0.8s ease;"></div>
                    </div>
                `;
                
                card.innerHTML = `
                    <div class="category-emoji">${category.emoji}</div>
                    <div class="category-name">${category.name}</div>
                    ${progressBarHtml}
                    <div class="category-progress">${learnedInCategory}/${category.words.length} слов (${progressPercent}%)</div>
                    <div style="margin-top: 8px; font-size: 12px; color: #64748b;">
                        Кликните для изучения
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        function selectCategory(categoryKey) {
            selectedCategory = categoryKey;
            showSection('learn');
            loadCategoryWord();
        }

        // Learning functions
        function getAllWords() {
            return Object.values(wordCategories).flatMap(category => category.words);
        }

        function loadRandomLearnWord() {
            const allWords = getAllWords();
            currentLearnWord = allWords[Math.floor(Math.random() * allWords.length)];
            displayLearnWord();
        }

        function loadCategoryWord() {
            if (!selectedCategory) return;
            const categoryWords = wordCategories[selectedCategory].words;
            currentLearnWord = categoryWords[Math.floor(Math.random() * categoryWords.length)];
            displayLearnWord();
        }

        async function displayLearnWord() {
            if (!currentLearnWord) return;

            document.getElementById('learn-emoji').textContent = currentLearnWord.emoji;
            document.getElementById('learn-english').textContent = currentLearnWord.english;
            document.getElementById('learn-russian').textContent = currentLearnWord.russian;
            document.getElementById('learn-pronunciation').textContent = currentLearnWord.pronunciation;

            // Add example sentence if available
            const wordCard = document.getElementById('learn-word-card');
            let exampleHtml = '';
            if (currentLearnWord.example && currentLearnWord.translation) {
                exampleHtml = `
                    <div style="background: rgba(102, 126, 234, 0.1); border-radius: 12px; padding: 16px; margin: 16px 0; border-left: 4px solid #667eea;">
                        <div style="font-size: 16px; color: #2C3E50; margin-bottom: 6px; font-weight: 600;">
                            📝 Пример использования:
                        </div>
                        <div style="font-size: 14px; color: #667eea; font-style: italic; margin-bottom: 6px;">
                            "${currentLearnWord.example}"
                        </div>
                        <div style="font-size: 13px; color: #64748b;">
                            "${currentLearnWord.translation}"
                        </div>
                    </div>
                `;
            }

            // Update the word card with example
            const actionButtons = wordCard.querySelector('.action-buttons');
            const existingExample = wordCard.querySelector('.example-section');
            if (existingExample) {
                existingExample.remove();
            }
            
            if (exampleHtml) {
                const exampleDiv = document.createElement('div');
                exampleDiv.className = 'example-section';
                exampleDiv.innerHTML = exampleHtml;
                wordCard.insertBefore(exampleDiv, actionButtons);
            }

            // Автоматически отмечаем слово как просмотренное
            await saveWordView(currentLearnWord.english);

            // Update progress
            const allWords = getAllWords();
            const learnedWords = new Set(gameData.map(item => item.word)).size;
            const progress = (learnedWords / allWords.length) * 100;
            document.getElementById('learn-progress').style.width = progress + '%';
            document.getElementById('learn-progress-text').textContent = 
                `${learnedWords} из ${allWords.length} слов изучено`;
        }

        async function nextLearnWord() {
            // Автоматически сохраняем прогресс изучения
            if (currentLearnWord) {
                await saveWordProgress(currentLearnWord.english, true);
            }
            
            if (selectedCategory) {
                loadCategoryWord();
            } else {
                loadRandomLearnWord();
            }
        }

        function speakWord() {
            if (!currentLearnWord || !window.speechSynthesis) return;
            
            const utterance = new SpeechSynthesisUtterance(currentLearnWord.english);
            utterance.lang = 'en-US';
            utterance.rate = 0.8;
            window.speechSynthesis.speak(utterance);
        }

        async function toggleFavorite() {
            if (!currentLearnWord || !window.dataSdk) return;

            showLoading(true);
            
            try {
                const existingRecord = gameData.find(item => item.word === currentLearnWord.english);
                
                if (existingRecord) {
                    existingRecord.is_favorite = !existingRecord.is_favorite;
                    await window.dataSdk.update(existingRecord);
                } else {
                    if (gameData.length >= 999) {
                        showMessage("Достигнут лимит в 999 слов!");
                        return;
                    }
                    
                    const newRecord = {
                        word: currentLearnWord.english,
                        category: getCategoryForWord(currentLearnWord.english),
                        correct_answers: 0,
                        total_attempts: 0,
                        learned_date: new Date().toISOString(),
                        difficulty_level: 1,
                        is_favorite: true,
                        last_practiced: new Date().toISOString()
                    };
                    
                    await window.dataSdk.create(newRecord);
                }
            } catch (error) {
                console.error("Error toggling favorite:", error);
            } finally {
                showLoading(false);
            }
        }

        // Quiz functions
        function startQuiz() {
            quizScore = 0;
            quizTotal = 0;
            nextQuizWord();
        }

        function nextQuizWord() {
            const allWords = getAllWords();
            currentQuizWord = allWords[Math.floor(Math.random() * allWords.length)];
            
            document.getElementById('quiz-emoji').textContent = currentQuizWord.emoji;
            document.getElementById('quiz-english').textContent = currentQuizWord.english;
            
            generateQuizOptions();
            updateQuizProgress();
            
            // Hide next button and enable options
            document.getElementById('next-quiz-btn').style.display = 'none';
        }

        function generateQuizOptions() {
            const options = [currentQuizWord.russian];
            const allWords = getAllWords();
            
            while (options.length < 4) {
                const randomWord = allWords[Math.floor(Math.random() * allWords.length)];
                if (!options.includes(randomWord.russian)) {
                    options.push(randomWord.russian);
                }
            }
            
            // Shuffle options
            for (let i = options.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [options[i], options[j]] = [options[j], options[i]];
            }
            
            const container = document.getElementById('quiz-options');
            container.innerHTML = '';
            
            options.forEach(option => {
                const button = document.createElement('button');
                button.className = 'quiz-option';
                button.textContent = option;
                button.onclick = () => checkQuizAnswer(option, button);
                container.appendChild(button);
            });
        }

        async function checkQuizAnswer(selectedAnswer, button) {
            quizTotal++;
            const isCorrect = selectedAnswer === currentQuizWord.russian;
            
            if (isCorrect) {
                quizScore++;
                button.classList.add('correct');
            } else {
                button.classList.add('incorrect');
                
                // Highlight correct answer
                const buttons = document.querySelectorAll('.quiz-option');
                buttons.forEach(btn => {
                    if (btn.textContent === currentQuizWord.russian) {
                        btn.classList.add('correct');
                    }
                });
            }

            // Save progress
            await saveWordProgress(currentQuizWord.english, isCorrect);
            
            // Disable all buttons
            const buttons = document.querySelectorAll('.quiz-option');
            buttons.forEach(btn => btn.disabled = true);
            
            document.getElementById('next-quiz-btn').style.display = 'block';
            updateQuizScore();
        }

        function updateQuizScore() {
            document.getElementById('quiz-score').textContent = `Счёт: ${quizScore}/${quizTotal}`;
        }

        function updateQuizProgress() {
            const progress = Math.min((quizTotal / 10) * 100, 100);
            document.getElementById('quiz-progress').style.width = progress + '%';
        }

        // Sentence building functions
        function newSentenceExercise() {
            const sentences = [
                { words: ['I', 'like', 'apples'], translation: 'Мне нравятся яблоки' },
                { words: ['The', 'cat', 'is', 'big'], translation: 'Кот большой' },
                { words: ['My', 'dog', 'runs', 'fast'], translation: 'Моя собака бегает быстро' },
                { words: ['We', 'eat', 'pizza'], translation: 'Мы едим пиццу' },
                { words: ['She', 'has', 'a', 'car'], translation: 'У неё есть машина' },
                { words: ['This', 'is', 'my', 'house'], translation: 'Это мой дом' },
                { words: ['I', 'can', 'swim'], translation: 'Я умею плавать' },
                { words: ['The', 'sun', 'is', 'bright'], translation: 'Солнце яркое' },
                { words: ['Birds', 'can', 'fly'], translation: 'Птицы умеют летать' },
                { words: ['Water', 'is', 'cold'], translation: 'Вода холодная' }
            ];
            
            const randomSentence = sentences[Math.floor(Math.random() * sentences.length)];
            currentSentenceWords = [...randomSentence.words];
            userSentence = [];
            
            // Show translation hint
            const sentenceBuilder = document.querySelector('.sentence-builder h3');
            sentenceBuilder.innerHTML = `Составьте предложение<br><small style="color: #64748b; font-weight: normal;">"${randomSentence.translation}"</small>`;
            
            // Shuffle words for word bank
            const shuffledWords = [...currentSentenceWords].sort(() => Math.random() - 0.5);
            
            const wordBank = document.getElementById('word-bank');
            wordBank.innerHTML = '';
            
            shuffledWords.forEach(word => {
                const token = document.createElement('div');
                token.className = 'word-token';
                token.textContent = word;
                token.onclick = () => addWordToSentence(word, token);
                wordBank.appendChild(token);
            });
            
            clearSentence();
        }

        function addWordToSentence(word, token) {
            userSentence.push(word);
            token.style.display = 'none';
            updateSentenceDisplay();
        }

        function updateSentenceDisplay() {
            const sentenceArea = document.getElementById('sentence-area');
            
            if (userSentence.length === 0) {
                sentenceArea.innerHTML = '<p style="color: #95a5a6;">Кликайте на слова для составления предложения</p>';
            } else {
                sentenceArea.innerHTML = userSentence.map((word, index) => 
                    `<span class="word-token" onclick="removeWordFromSentence(${index})">${word}</span>`
                ).join(' ');
            }
        }

        function removeWordFromSentence(index) {
            const removedWord = userSentence.splice(index, 1)[0];
            
            // Show the word back in word bank
            const wordTokens = document.querySelectorAll('#word-bank .word-token');
            wordTokens.forEach(token => {
                if (token.textContent === removedWord && token.style.display === 'none') {
                    token.style.display = 'block';
                    return;
                }
            });
            
            updateSentenceDisplay();
        }

        async function checkSentence() {
            const isCorrect = JSON.stringify(userSentence) === JSON.stringify(currentSentenceWords);
            
            // Автоматически сохраняем прогресс для всех слов в предложении
            for (const word of currentSentenceWords) {
                await saveWordProgress(word, isCorrect);
            }
            
            if (isCorrect) {
                showMessage('Отлично! Предложение составлено правильно! 🎉');
                // Автоматически переходим к новому упражнению через 2 секунды
                setTimeout(() => {
                    newSentenceExercise();
                }, 2000);
            } else {
                showMessage(`Попробуйте ещё раз. Правильный порядок: ${currentSentenceWords.join(' ')}`);
            }
        }

        function clearSentence() {
            userSentence = [];
            
            // Show all words in word bank
            const wordTokens = document.querySelectorAll('#word-bank .word-token');
            wordTokens.forEach(token => {
                token.style.display = 'block';
            });
            
            updateSentenceDisplay();
        }

        // Favorites functions
        function updateFavorites() {
            const favoriteWords = gameData.filter(item => item.is_favorite);
            const container = document.getElementById('favorites-list');
            
            if (favoriteWords.length === 0) {
                container.innerHTML = `
                    <div style="text-align: center; padding: 40px 20px;">
                        <div style="font-size: 60px; margin-bottom: 16px;">⭐</div>
                        <h3 style="color: #64748b; margin-bottom: 12px;">Пока нет избранных слов</h3>
                        <p style="color: #94a3b8; font-size: 14px;">Добавляйте слова в избранное во время изучения!</p>
                        <button class="action-btn" onclick="showSection('learn')" style="margin-top: 16px;">
                            🎓 Перейти к изучению
                        </button>
                    </div>
                `;
                return;
            }
            
            container.innerHTML = `
                <div style="text-align: center; margin-bottom: 24px;">
                    <h3 style="color: #2C3E50;">⭐ Ваши избранные слова (${favoriteWords.length})</h3>
                </div>
            `;
            
            favoriteWords.forEach(wordData => {
                const wordInfo = findWordByEnglish(wordData.word);
                if (!wordInfo) return;
                
                const card = document.createElement('div');
                card.className = 'word-card';
                card.style.margin = '16px 0';
                card.innerHTML = `
                    <div class="word-emoji">${wordInfo.emoji}</div>
                    <div class="word-english">${wordInfo.english}</div>
                    <div class="word-russian">${wordInfo.russian}</div>
                    <div class="word-pronunciation">${wordInfo.pronunciation}</div>
                    <div class="action-buttons">
                        <button class="action-btn speak" onclick="speakFavoriteWord('${wordInfo.english}')">
                            🔊 Произнести
                        </button>
                        <button class="action-btn" onclick="removeFavorite('${wordData.word}')">
                            ❌ Удалить из избранного
                        </button>
                        <button class="action-btn" onclick="practiceFavorite('${wordInfo.english}')">
                            🎯 Практиковать
                        </button>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function practiceFavorite(english) {
            const wordInfo = findWordByEnglish(english);
            if (wordInfo) {
                currentLearnWord = wordInfo;
                showSection('learn');
                displayLearnWord();
            }
        }

        function speakFavoriteWord(english) {
            if (!window.speechSynthesis) return;
            
            const utterance = new SpeechSynthesisUtterance(english);
            utterance.lang = 'en-US';
            utterance.rate = 0.8;
            window.speechSynthesis.speak(utterance);
        }

        async function removeFavorite(word) {
            if (!window.dataSdk) return;

            showLoading(true);
            
            try {
                const record = gameData.find(item => item.word === word);
                if (record) {
                    record.is_favorite = false;
                    await window.dataSdk.update(record);
                }
            } catch (error) {
                console.error("Error removing favorite:", error);
            } finally {
                showLoading(false);
            }
        }

        // Utility functions
        function findWordByEnglish(english) {
            return getAllWords().find(word => word.english === english);
        }

        function getCategoryForWord(english) {
            for (const [key, category] of Object.entries(wordCategories)) {
                if (category.words.some(word => word.english === english)) {
                    return key;
                }
            }
            return 'general';
        }

        async function saveWordView(word) {
            if (!window.dataSdk) return;
            
            try {
                const existingRecord = gameData.find(item => item.word === word);
                
                if (!existingRecord) {
                    if (gameData.length >= 999) {
                        return; // Тихо игнорируем, если лимит достигнут
                    }
                    
                    const newRecord = {
                        word: word,
                        category: getCategoryForWord(word),
                        correct_answers: 0,
                        total_attempts: 0,
                        learned_date: new Date().toISOString(),
                        difficulty_level: 1,
                        is_favorite: false,
                        last_practiced: new Date().toISOString()
                    };
                    
                    await window.dataSdk.create(newRecord);
                    showSaveIndicator();
                }
            } catch (error) {
                console.error("Error saving word view:", error);
            }
        }

        async function saveWordProgress(word, isCorrect) {
            if (!window.dataSdk) return;

            showLoading(true);
            
            try {
                const existingRecord = gameData.find(item => item.word === word);
                
                if (existingRecord) {
                    existingRecord.total_attempts++;
                    if (isCorrect) {
                        existingRecord.correct_answers++;
                    }
                    existingRecord.last_practiced = new Date().toISOString();
                    
                    await window.dataSdk.update(existingRecord);
                    showSaveIndicator();
                } else {
                    if (gameData.length >= 999) {
                        showMessage("Достигнут лимит в 999 слов!");
                        return;
                    }
                    
                    const newRecord = {
                        word: word,
                        category: getCategoryForWord(word),
                        correct_answers: isCorrect ? 1 : 0,
                        total_attempts: 1,
                        learned_date: new Date().toISOString(),
                        difficulty_level: 1,
                        is_favorite: false,
                        last_practiced: new Date().toISOString()
                    };
                    
                    await window.dataSdk.create(newRecord);
                    showSaveIndicator();
                }
            } catch (error) {
                console.error("Error saving progress:", error);
            } finally {
                showLoading(false);
            }
        }

        function showLoading(show) {
            document.getElementById('loading').classList.toggle('hidden', !show);
        }

        function showSaveIndicator() {
            const indicator = document.getElementById('save-indicator');
            if (indicator) {
                indicator.style.display = 'inline';
                indicator.style.animation = 'fadeIn 0.3s ease';
                
                setTimeout(() => {
                    indicator.style.display = 'none';
                }, 2000);
            }
        }

        function showMessage(message) {
            // Create an enhanced toast message with animations
            const toast = document.createElement('div');
            toast.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                background: linear-gradient(135deg, #11998e, #38ef7d);
                color: white;
                padding: 16px 24px;
                border-radius: 12px;
                font-weight: 700;
                font-size: 14px;
                z-index: 10000;
                box-shadow: 
                    0 20px 40px rgba(0,0,0,0.15),
                    0 0 0 1px rgba(255,255,255,0.2),
                    inset 0 1px 0 rgba(255,255,255,0.3);
                backdrop-filter: blur(10px);
                transform: translateX(400px) scale(0.8);
                opacity: 0;
                transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
                border: 2px solid rgba(255,255,255,0.2);
                display: flex;
                align-items: center;
                gap: 10px;
                max-width: 300px;
            `;
            
            // Add icon based on message type
            const icon = document.createElement('span');
            icon.style.fontSize = '18px';
            if (message.includes('Отлично') || message.includes('🎉')) {
                icon.textContent = '🎉';
            } else if (message.includes('Сохранено')) {
                icon.textContent = '💾';
            } else {
                icon.textContent = '💫';
            }
            
            const text = document.createElement('span');
            text.textContent = message;
            
            toast.appendChild(icon);
            toast.appendChild(text);
            document.body.appendChild(toast);
            
            // Animate in
            setTimeout(() => {
                toast.style.transform = 'translateX(0) scale(1)';
                toast.style.opacity = '1';
            }, 10);
            
            // Animate out and remove
            setTimeout(() => {
                toast.style.transform = 'translateX(400px) scale(0.8)';
                toast.style.opacity = '0';
                setTimeout(() => {
                    if (toast.parentNode) {
                        toast.remove();
                    }
                }, 500);
            }, 3500);
        }

        // Study plan functions
        function showWeek(weekNumber) {
            currentWeek = weekNumber;
            
            // Update week buttons
            document.querySelectorAll('.week-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            updateStudyPlan();
        }

        function updateStudyPlan() {
            const weekData = studyPlan[currentWeek];
            if (!weekData) return;

            // Update week content
            const weekContent = document.getElementById('week-content');
            weekContent.innerHTML = `
                <div class="week-content">
                    <h3>${weekData.title}</h3>
                    <p style="color: #64748b; font-size: 14px; margin: 12px 0;">${weekData.description}</p>
                    <div style="background: rgba(102, 126, 234, 0.1); padding: 12px; border-radius: 10px; margin: 12px 0;">
                        <strong>Ключевые слова недели:</strong> ${weekData.words.join(', ')}
                    </div>
                </div>
            `;

            // Update daily tasks
            const dailyTasks = document.getElementById('daily-tasks');
            dailyTasks.innerHTML = '';

            weekData.tasks.forEach((task, index) => {
                const dayNumber = index + 1;
                const dayCard = document.createElement('div');
                dayCard.className = 'day-card';
                
                // Determine day status
                const totalDays = (currentWeek - 1) * 7 + dayNumber;
                if (totalDays < currentDay) {
                    dayCard.classList.add('completed');
                } else if (totalDays === currentDay) {
                    dayCard.classList.add('current');
                }
                
                dayCard.onclick = () => selectDay(dayNumber);
                dayCard.innerHTML = `
                    <div class="day-number">День ${dayNumber}</div>
                    <div class="day-task">${task}</div>
                `;
                
                dailyTasks.appendChild(dayCard);
            });

            // Update progress
            const totalDays = 84; // 12 weeks * 7 days
            const completedDays = Math.max(0, currentDay - 1);
            const progress = (completedDays / totalDays) * 100;
            
            document.getElementById('month-progress').style.width = progress + '%';
            document.getElementById('month-progress-text').textContent = 
                `День ${currentDay} из ${totalDays} (${Math.round(progress)}% завершено)`;
        }

        async function selectDay(dayNumber) {
            const weekData = studyPlan[currentWeek];
            const task = weekData.tasks[dayNumber - 1];
            
            // Mark day as completed and save progress
            const totalDay = (currentWeek - 1) * 7 + dayNumber;
            if (totalDay >= currentDay) {
                currentDay = totalDay + 1;
                await saveStudyProgress();
                updateStudyPlan();
                
                showMessage(`✅ День ${dayNumber} завершён: ${task}`);
            }
        }

        async function saveStudyProgress() {
            if (!window.dataSdk) return;

            try {
                const progressRecord = gameData.find(item => item.type === 'study_progress');
                
                if (progressRecord) {
                    progressRecord.current_week = currentWeek;
                    progressRecord.current_day = currentDay;
                    progressRecord.last_updated = new Date().toISOString();
                    await window.dataSdk.update(progressRecord);
                } else {
                    if (gameData.length >= 999) {
                        return;
                    }
                    
                    const newProgress = {
                        type: 'study_progress',
                        current_week: currentWeek,
                        current_day: currentDay,
                        started_date: new Date().toISOString(),
                        last_updated: new Date().toISOString()
                    };
                    
                    await window.dataSdk.create(newProgress);
                }
                
                showSaveIndicator();
            } catch (error) {
                console.error("Error saving study progress:", error);
            }
        }

        function loadStudyProgress() {
            const progressRecord = gameData.find(item => item.type === 'study_progress');
            if (progressRecord) {
                currentWeek = progressRecord.current_week || 1;
                currentDay = progressRecord.current_day || 1;
            }
        }

        // Listening functions
        let currentListeningWord = null;
        let listeningScore = 0;
        let listeningTotal = 0;

        function startListening() {
            listeningScore = 0;
            listeningTotal = 0;
            nextListeningWord();
        }

        function nextListeningWord() {
            const allWords = getAllWords();
            currentListeningWord = allWords[Math.floor(Math.random() * allWords.length)];
            
            document.getElementById('listening-emoji').textContent = currentListeningWord.emoji;
            document.getElementById('play-listening-btn').style.display = 'block';
            document.getElementById('repeat-listening-btn').style.display = 'none';
            document.getElementById('listening-options').style.display = 'none';
            document.getElementById('next-listening-btn').style.display = 'none';
            
            generateListeningOptions();
        }

        function playListeningWord() {
            if (!currentListeningWord || !window.speechSynthesis) return;
            
            const utterance = new SpeechSynthesisUtterance(currentListeningWord.english);
            utterance.lang = 'en-US';
            utterance.rate = 0.7;
            window.speechSynthesis.speak(utterance);
            
            // Show options after playing
            setTimeout(() => {
                document.getElementById('listening-options').style.display = 'grid';
                document.getElementById('repeat-listening-btn').style.display = 'inline-flex';
            }, 1000);
        }

        function repeatListening() {
            playListeningWord();
        }

        function generateListeningOptions() {
            const options = [currentListeningWord.russian];
            const allWords = getAllWords();
            
            while (options.length < 4) {
                const randomWord = allWords[Math.floor(Math.random() * allWords.length)];
                if (!options.includes(randomWord.russian)) {
                    options.push(randomWord.russian);
                }
            }
            
            // Shuffle options
            for (let i = options.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [options[i], options[j]] = [options[j], options[i]];
            }
            
            const container = document.getElementById('listening-options');
            container.innerHTML = '';
            
            options.forEach(option => {
                const button = document.createElement('button');
                button.className = 'quiz-option';
                button.textContent = option;
                button.onclick = () => checkListeningAnswer(option, button);
                container.appendChild(button);
            });
        }

        async function checkListeningAnswer(selectedAnswer, button) {
            listeningTotal++;
            const isCorrect = selectedAnswer === currentListeningWord.russian;
            
            if (isCorrect) {
                listeningScore++;
                button.classList.add('correct');
                showMessage('🎉 Отлично! Правильный ответ!');
            } else {
                button.classList.add('incorrect');
                showMessage(`❌ Неправильно. Правильный ответ: ${currentListeningWord.russian}`);
                
                // Highlight correct answer
                const buttons = document.querySelectorAll('#listening-options .quiz-option');
                buttons.forEach(btn => {
                    if (btn.textContent === currentListeningWord.russian) {
                        btn.classList.add('correct');
                    }
                });
            }

            // Save progress
            await saveWordProgress(currentListeningWord.english, isCorrect);
            
            // Disable all buttons
            const buttons = document.querySelectorAll('#listening-options .quiz-option');
            buttons.forEach(btn => btn.disabled = true);
            
            document.getElementById('next-listening-btn').style.display = 'block';
            document.getElementById('listening-score').textContent = `Счёт: ${listeningScore}/${listeningTotal}`;
        }

        // Speaking functions
        let currentSpeakingWord = null;
        let speakingCount = 0;
        let isRecording = false;

        function loadRandomSpeakingWord() {
            const allWords = getAllWords();
            currentSpeakingWord = allWords[Math.floor(Math.random() * allWords.length)];
            displaySpeakingWord();
        }

        function displaySpeakingWord() {
            if (!currentSpeakingWord) return;

            document.getElementById('speaking-emoji').textContent = currentSpeakingWord.emoji;
            document.getElementById('speaking-english').textContent = currentSpeakingWord.english;
            document.getElementById('speaking-russian').textContent = currentSpeakingWord.russian;
            document.getElementById('speaking-pronunciation').textContent = currentSpeakingWord.pronunciation;
        }

        function playOriginalWord() {
            if (!currentSpeakingWord || !window.speechSynthesis) return;
            
            const utterance = new SpeechSynthesisUtterance(currentSpeakingWord.english);
            utterance.lang = 'en-US';
            utterance.rate = 0.6;
            window.speechSynthesis.speak(utterance);
        }

        async function startRecording() {
            if (isRecording) return;
            
            const recordBtn = document.getElementById('record-btn');
            const recordingStatus = document.getElementById('recording-status');
            
            try {
                isRecording = true;
                recordBtn.textContent = '⏹️ Остановить запись';
                recordingStatus.style.display = 'block';
                
                // Simulate recording for 3 seconds
                setTimeout(async () => {
                    isRecording = false;
                    recordBtn.textContent = '🎤 Записать произношение';
                    recordingStatus.style.display = 'none';
                    
                    speakingCount++;
                    document.getElementById('speaking-score').textContent = `Слов произнесено: ${speakingCount}`;
                    
                    // Save progress
                    await saveWordProgress(currentSpeakingWord.english, true);
                    
                    showMessage('✅ Запись завершена! Отличная работа!');
                }, 3000);
                
            } catch (error) {
                isRecording = false;
                recordBtn.textContent = '🎤 Записать произношение';
                recordingStatus.style.display = 'none';
                showMessage('❌ Ошибка записи. Попробуйте ещё раз.');
            }
        }

        function nextSpeakingWord() {
            loadRandomSpeakingWord();
        }

        // Initialize app when page loads
        document.addEventListener('DOMContentLoaded', () => {
            initializeApp();
            loadRandomSpeakingWord();
        });
    </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'99e96ac5521d9a64',t:'MTc2MzE1MzgwMy4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
