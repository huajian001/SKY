<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>指挥 100 词智能战术 App v3.0</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        tactical: {
                            bg: '#0B0F17',
                            card: '#121826',
                            border: '#1E293B',
                            accent: '#10B981', // 极光绿
                            amber: '#F59E0B'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body { background-color: #0B0F17; color: #E2E8F0; font-family: 'Inter', system-ui, sans-serif; }
        .glass-header { background: rgba(18, 24, 38, 0.95); backdrop-filter: blur(10px); border-bottom: 1px solid #1E293B; }
        .glow-green { text-shadow: 0 0 10px rgba(16, 185, 129, 0.5); }
        .btn-tactical { transition: all 0.2s; border: 1px solid #1E293B; background: #161E2E; }
        .btn-tactical:active { transform: scale(0.95); background: #1E293B; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .tab-active { background: #0891B2; color: white; border: none; box-shadow: 0 4px 12px rgba(8, 145, 178, 0.4); }
        input::placeholder { color: #475569; }
    </style>
</head>
<body class="pb-24">

    <!-- 顶部导航栏 (参考 1000017999.jpg) -->
    <header class="sticky top-0 z-50 glass-header px-4 pt-4 pb-2">
        <div class="flex items-center justify-between mb-4">
            <div class="flex items-center space-x-3">
                <div class="w-10 h-10 rounded-xl bg-slate-800 border border-cyan-500/30 flex items-center justify-center text-cyan-400 shadow-[0_0_15px_rgba(6,182,212,0.2)]">
                    <i class="fa-solid fa-shield-halved text-xl"></i>
                </div>
                <div>
                    <h1 class="text-lg font-bold tracking-tight text-slate-100">指挥 100 词智能战术 App</h1>
                    <p class="text-[10px] text-slate-500 font-mono tracking-widest uppercase">Tactical Command Dictionary v3.0</p>
                </div>
            </div>
            <button onclick="syncData()" class="w-10 h-10 rounded-full bg-emerald-950/30 border border-emerald-500/30 flex items-center justify-center text-emerald-500">
                <i class="fa-solid fa-cloud-arrow-down"></i>
            </button>
        </div>

        <!-- 顶部功能按钮 -->
        <div class="grid grid-cols-2 gap-3 mb-4">
            <button onclick="exportExcel()" class="btn-tactical py-2 rounded-lg text-xs font-semibold text-emerald-400 flex items-center justify-center space-x-2">
                <i class="fa-solid fa-file-export"></i>
                <span>导出 Excel</span>
            </button>
            <button onclick="showAddModal()" class="btn-tactical py-2 rounded-lg text-xs font-semibold text-cyan-400 flex items-center justify-center space-x-2">
                <i class="fa-solid fa-plus-circle"></i>
                <span>录入新指令</span>
            </button>
        </div>

        <!-- 分类选项卡 -->
        <div class="flex space-x-2 overflow-x-auto no-scrollbar pb-2" id="categoryTabs">
            <!-- 动态渲染 -->
        </div>
    </header>

    <!-- 搜索与统计 -->
    <div class="px-4 py-3">
        <div class="relative group">
            <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 text-slate-500 text-xs"></i>
            <input type="text" id="searchInput" oninput="filterWords()" placeholder="快速检索战术意义或拟音..." 
                   class="w-full bg-slate-900/50 border border-slate-800 rounded-xl py-2.5 pl-10 pr-4 text-sm focus:outline-none focus:border-cyan-500/50 transition-all">
        </div>
    </div>

    <!-- 核心列表区 -->
    <main class="px-4 space-y-3" id="wordContainer">
        <!-- 动态生成战术卡片 -->
    </main>

    <!-- 底部状态栏 (参考 1000017999.jpg) -->
    <footer class="fixed bottom-0 left-0 right-0 bg-slate-950/90 backdrop-blur-md border-t border-slate-800 px-4 py-3 z-40">
        <div class="flex items-center justify-between text-[11px]">
            <div class="flex items-center space-x-2">
                <div class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse shadow-[0_0_8px_#10B981]"></div>
                <span class="text-slate-400">已启用离线持久保存技术</span>
            </div>
            <div class="text-slate-500">
                筛选结果: <span class="text-cyan-400 font-bold font-mono" id="resultCount">100</span> 项
            </div>
        </div>
        <div class="mt-4 text-center">
            <p class="text-[10px] text-slate-600 font-medium">维和安保航战术精简版 App 系统</p>
            <p class="text-[9px] text-slate-700 font-mono">APP ID: military_swahili_pwa_system</p>
  