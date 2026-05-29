```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>军警指挥词汇系统</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { background-color: #0f172a; color: #f1f5f9; }
        .card { background-color: #1e293b; border-radius: 12px; transition: transform 0.1s; }
        .card:active { transform: scale(0.98); }
    </style>
</head>
<body class="p-4">

<div class="max-w-md mx-auto">
    <h1 class="text-2xl font-bold mb-4 text-center text-blue-400">指挥词汇系统</h1>
    
    <!-- 搜索框 -->
    <input type="text" id="search" placeholder="搜索中文、拼音或音译..." 
           class="w-full p-3 mb-4 rounded-lg bg-slate-800 border border-slate-700 text-white focus:outline-none focus:ring-2 focus:ring-blue-500">
    
    <!-- 分类筛选 -->
    <div id="categories" class="flex gap-2 overflow-x-auto pb-4 mb-2">
        <button onclick="filter('全部')" class="px-4 py-1 bg-blue-600 rounded-full text-sm shrink-0">全部</button>
        <button onclick="filter('指挥')" class="px-4 py-1 bg-slate-700 rounded-full text-sm shrink-0">指挥</button>
        <button onclick="filter('行动')" class="px-4 py-1 bg-slate-700 rounded-full text-sm shrink-0">行动</button>
        <button onclick="filter('方位')" class="px-4 py-1 bg-slate-700 rounded-full text-sm shrink-0">方位</button>
        <button onclick="filter('装备')" class="px-4 py-1 bg-slate-700 rounded-full text-sm shrink-0">装备</button>
        <button onclick="filter('状态')" class="px-4 py-1 bg-slate-700 rounded-full text-sm shrink-0">状态</button>
    </div>

    <div id="list" class="space-y-3">
        <!-- 列表由 JS 注入 -->
    </div>
</div>

<script>
const data = [
    { id: 1, cat: '指挥', cn: '领导/首长', ref: 'Boss', trans: '波丝' },
    { id: 2, cat: '指挥', cn: '警察', ref: 'Polisi', trans: '波利斯' },
    { id: 16, cat: '行动', cn: '过来', ref: 'Guya', trans: '古雅' },
    { id: 36, cat: '方位', cn: '前面', ref: 'Mbele', trans: '工贝里' },
    { id: 56, cat: '装备', cn: '步枪', ref: 'Munonge', trans: '布顿给' },
    { id: 76, cat: '状态', cn: '注意/小心', ref: 'Attention', trans: '阿当雄' },
    // 此处仅示例部分，实际应用应填入完整100条
    { id: 99, cat: '指挥', cn: '你好', ref: 'Jambo', trans: '江波' }
];

function render(arr) {
    const list = document.getElementById('list');
    list.innerHTML = arr.map(item => `
        <div class="card p-4 border border-slate-700" onclick="speak('${item.ref}')">
            <div class="flex justify-between items-center">
                <span class="font-bold text-blue-300">${item.cn}</span>
                <span class="text-xs text-slate-400">${item.ref}</span>
            </div>
            <div class="text-lg mt-1 text-white">${item.trans}</div>
        </div>
    `).join('');
}

function speak(text) {
    const utterance = new SpeechSynthesisUtterance(text);
    utterance.lang = 'en-US';
    window.speechSynthesis.speak(utterance);
}

document.getElementById('search').oninput = (e) => {
    const val = e.target.value.toLowerCase();
    render(data.filter(i => i.cn.includes(val) || i.trans.includes(val) || i.ref.toLowerCase().includes(val)));
};

function filter(cat) {
    render(cat === '全部' ? data : data.filter(i => i.cat === cat));
}

render(data);
</script>
</body>
</html>

```
