```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>军警指挥 100 词手持辅助系统</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        tactical: {
                            gold: '#D4AF37',
                            olive: '#556B2F',
                            dark: '#121820',
                            card: '#1E2631',
                            red: '#8B0000',
                            redlight: '#FF4D4D'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        /* 战术红光夜视模式特定样式 */
        .night-vision {
            background-color: #0d0202 !important;
            color: #ff3333 !important;
        }
        .night-vision .tactical-card {
            background-color: #1a0505 !important;
            border-color: #550000 !important;
            color: #ff5555 !important;
        }
        .night-vision button {
            color: #ff3333 !important;
            border-color: #880000 !important;
        }
        .night-vision input, .night-vision select {
            background-color: #150202 !important;
            color: #ff3333 !important;
            border-color: #660000 !important;
        }
        .night-vision .highlight-text {
            color: #ff9999 !important;
        }
        /* 隐藏滚动条但保留滚动功能 */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        /* 针对手机端触摸反馈的优化 */
        .active-scale:active {
            transform: scale(0.95);
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen flex flex-col font-sans transition-colors duration-200 select-none pb-16">

    <!-- 顶部导航栏 -->
    <header class="sticky top-0 z-50 bg-slate-950 border-b border-slate-800 px-4 py-3 flex items-center justify-between shadow-md transition-colors duration-200" id="headerNav">
        <div class="flex items-center space-x-2">
            <div class="w-8 h-8 rounded-lg bg-emerald-600 flex items-center justify-center text-white font-bold shadow-lg shadow-emerald-900/40">
                <i class="fa-solid fa-shield-halved"></i>
            </div>
            <div>
                <h1 class="text-sm font-bold tracking-wider text-emerald-400">军警东非指挥系统</h1>
                <p class="text-[10px] text-slate-400">斯瓦希里语/战术100词</p>
            </div>
        </div>
        
        <div class="flex items-center space-x-2">
            <!-- 战术红光夜视切换 -->
            <button onclick="toggleNightVision()" class="w-10 h-10 rounded-full border border-slate-700 bg-slate-800/80 flex items-center justify-center active-scale text-slate-300" title="夜视模式">
                <i class="fa-solid fa-eye-low-beam text-sm text-red-500"></i>
            </button>
            <!-- 听写测试按钮 -->
            <button onclick="openQuiz()" class="px-3 py-2 rounded-lg bg-emerald-700 hover:bg-emerald-600 text-xs font-semibold flex items-center space-x-1 active-scale shadow-md shadow-emerald-950/50">
                <i class="fa-solid fa-graduation-cap"></i>
                <span>测验</span>
            </button>
        </div>
    </header>

    <!-- 主容器 -->
    <main class="flex-1 p-3 max-w-md mx-auto w-full space-y-4" id="mainContent">
        
        <!-- 便捷搜索与分类过滤 -->
        <section class="bg-slate-850 p-3 rounded-xl border border-slate-800 shadow-sm space-y-3" id="filterArea">
            <div class="relative">
                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400">
                    <i class="fa-solid fa-magnifying-glass text-xs"></i>
                </span>
                <input type="text" id="searchInput" oninput="handleSearch()" placeholder="搜索中文/外文/音译/拼音..." class="w-full bg-slate-900 border border-slate-700 rounded-lg py-2 pl-9 pr-8 text-xs focus:outline-none focus:ring-2 focus:ring-emerald-500 text-slate-100 placeholder-slate-500">
                <button onclick="clearSearch()" class="absolute inset-y-0 right-0 pr-3 flex items-center text-slate-400 hover:text-white">
                    <i class="fa-solid fa-circle-xmark text-xs"></i>
                </button>
            </div>

            <!-- 分类滑动轴（针对手机横向滑动优化） -->
            <div class="flex space-x-1.5 overflow-x-auto no-scrollbar py-1">
                <button onclick="filterCategory('all')" id="cat-all" class="category-btn px-3 py-1.5 rounded-full text-[11px] font-medium bg-emerald-600 text-white whitespace-nowrap sh