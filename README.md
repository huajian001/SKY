```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
  <title>指挥速查系统</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>
    body { background-color: #0b0f19; color: #f1f5f9; -webkit-tap-highlight-color: transparent; }
    ::-webkit-scrollbar { display: none; }
    .tactical-bg { background: linear-gradient(135deg, #0f172a 0%, #020617 100%); }
  </style>
</head>
<body class="tactical-bg min-h-screen">
  <div id="app" class="p-4">
    <div class="text-center mb-6">
      <h1 class="text-xl font-black text-cyan-400 tracking-widest">指挥 100 词速查</h1>
      <p class="text-[10px] text-slate-500 mt-1">战术离线指挥系统</p>
    </div>
    
    <div class="overflow-x-auto bg-slate-900/50 rounded-xl border border-slate-800">
      <table class="w-full text-left text-xs border-collapse">
        <thead class="bg-slate-950 border-b border-slate-800">
          <tr>
            <th class="p-3 text-slate-400">序号</th>
            <th class="p-3 text-slate-400">中文</th>
            <th class="p-3 text-slate-400">参考词</th>
            <th class="p-3 text-slate-400">音译</th>
          </tr>
        </thead>
        <tbody id="data-body" class="divide-y divide-slate-800"></tbody>
      </table>
    </div>
  </div>

  <script>
    const data = [
      { id: 1, cn: "领导", sw: "Boss", py: "波丝" },
      { id: 2, cn: "警察", sw: "Polisi", py: "波利斯" },
      { id: 3, cn: "军人", sw: "Soldat", py: "索哒" },
      { id: 4, cn: "站住", sw: "Simama", py: "西马马" },
      { id: 5, cn: "别动", sw: "Guigala", py: "规嘎拉" },
      { id: 6, cn: "走", sw: "Dembia", py: "点必亚" },
      { id: 7, cn: "快点", sw: "Fast", py: "法斯特" },
      { id: 8, cn: "危险", sw: "Danger", py: "单杰" },
      { id: 9, cn: "注意", sw: "Attention", py: "阿当雄" },
      { id: 10, cn: "明白吗", sw: "Elewa", py: "艾类瓦" }
      // 此处可补全 100 词数据
    ];

    const body = document.getElementById('data-body');
    data.forEach(item => {
      body.innerHTML += `
        <tr class="hover:bg-slate-800/30">
          <td class="p-3 text-slate-600">${item.id}</td>
          <td class="p-3 font-bold text-white">${item.cn}</td>
          <td class="p-3 font-mono text-cyan-400">${item.sw}</td>
          <td class="p-3 text-amber-400">${item.py}</td>
        </tr>
      `;
    });
    lucide.createIcons();
  </script>
</body>
</html>

```
