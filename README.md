<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>军警指挥 100词 - 斯瓦希里语</title>
  <meta name="theme-color" content="#1e3a8a">
  <style>
    :root { --primary: #1e3a8a; --card: #1e2937; }
    body { 
      font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; 
      margin:0; background:#0f172a; color:#e2e8f0; line-height:1.5;
    }
    header { 
      background: linear-gradient(135deg, #1e3a8a, #3b82f6); 
      padding:1.2rem; text-align:center; position:sticky; top:0; z-index:100; box-shadow:0 4px 12px rgba(0,0,0,0.3);
    }
    .tab-bar { 
      display:flex; overflow-x:auto; background:#1e2937; padding:10px 8px; gap:8px; 
      scrollbar-width: none;
    }
    .tab { 
      padding:10px 18px; white-space:nowrap; border-radius:9999px; background:#334155; 
      font-size:15px; transition:all 0.2s;
    }
    .tab.active { background:var(--primary); color:white; font-weight:600; }
    
    .container { padding:16px; }
    .search { 
      width:100%; padding:15px 18px; border-radius:9999px; border:none; 
      background:#1e2937; color:white; font-size:17px; margin-bottom:16px;
    }
    .card { 
      background:var(--card); border-radius:16px; padding:18px; margin-bottom:12px; 
      box-shadow:0 4px 12px rgba(0,0,0,0.4); transition:all 0.2s;
    }
    .card:active { transform:scale(0.97); }
    .chinese { font-size:1.35rem; font-weight:700; margin:0 0 8px 0; }
    .trans { font-size:1.15rem; color:#60a5fa; margin:4px 0; }
    .swahili { font-size:0.98rem; color:#94a3b8; }
    
    .modal { 
      position:fixed; inset:0; background:rgba(0,0,0,0.95); display:none; 
      align-items:center; justify-content:center; z-index:1000;
    }
    .modal-content { 
      background:#1e2937; padding:28px; border-radius:20px; width:92%; max-width:420px; 
      text-align:center;
    }
    .btn { 
      padding:14px 24px; border-radius:9999px; border:none; font-size:16px; 
      margin:6px; cursor:pointer;
    }
    .btn-primary { background:var(--primary); color:white; font-weight:600; }
  </style>
</head>
<body>
  <header>
    <h1>🛡️ 军警指挥 100词</h1>
    <p>斯瓦希里语现场快速指挥工具</p>
  </header>

  <div class="tab-bar" id="tabs"></div>

  <div class="container">
    <input type="text" id="search" class="search" placeholder="🔍 搜索指令（中文 / 音译 / 斯瓦希里语）" onkeyup="filterCards()">
    <div id="cards"></div>
  </div>

  <!-- 闪卡模式 -->
  <div class="modal" id="flashModal">
    <div class="modal-content" onclick="flipCard()">
      <h2 id="flashChinese" style="font-size:2.1rem;margin:20px 0 12px;"></h2>
      <p id="flashTrans" style="font-size:1.5rem;color:#60a5fa;margin:8px 0;"></p>
      <p id="flashSwahili" style="font-size:1.15rem;color:#94a3b8;"></p>
      <div style="margin-top:24px;">
        <button class="btn btn-primary" onclick="event.stopImmediatePropagation();speakCurrent()">🔊 朗读</button>
        <button class="btn" onclick="event.stopImmediatePropagation();closeModal()">关闭</button>
      </div>
    </div>
  </div>

  <script>
    const vocabulary = [
      {id:1, cat:"指挥与身份", cn:"领导/首长", sw:"Boss", trans:"波丝"},
      {id:2, cat:"指挥与身份", cn:"警察", sw:"Polisi", trans:"波利斯"},
      {id:3, cat:"指挥与身份", cn:"军人", sw:"Soldat", trans:"索哒"},
      {id:4, cat:"指挥与身份", cn:"保安", sw:"Shei", trans:"谢义"},
      {id:5, cat:"指挥与身份", cn:"我", sw:"Miye", trans:"米也"},
      {id:6, cat:"指挥与身份", cn:"你", sw:"Wei", trans:"微"},
      {id:7, cat:"指挥与身份", cn:"他", sw:"Wuyou", trans:"无有"},
      {id:8, cat:"指挥与身份", cn:"刚果人", sw:"Congoli", trans:"刚果里"},
      {id:9, cat:"指挥与身份", cn:"外国人", sw:"Bazungu", trans:"巴容古"},
      {id:10, cat:"指挥与身份", cn:"姓名", sw:"Nom", trans:"农"},
      {id:11, cat:"指挥与身份", cn:"身份证", sw:"ID", trans:"埃迪"},
      {id:12, cat:"指挥与身份", cn:"护照", sw:"Passeport", trans:"帕斯报特"},
      {id:13, cat:"指挥与身份", cn:"签字", sw:"Signer", trans:"新艳"},
      {id:14, cat:"指挥与身份", cn:"盖章", sw:"Cachet", trans:"嘎谢"},
      {id:15, cat:"指挥与身份", cn:"组织/团伙", sw:"Group", trans:"古鲁普"},
      {id:16, cat:"行动指令", cn:"过来", sw:"Guya", trans:"古雅"},
      {id:17, cat:"行动指令", cn:"走/离开", sw:"Dembia", trans:"点必亚"},
      {id:18, cat:"行动指令", cn:"停止/站住", sw:"Simama", trans:"西马马"},
      {id:19, cat:"行动指令", cn:"别动", sw:"Guigala", trans:"规嘎拉"},
      {id:20, cat:"行动指令", cn:"坐下", sw:"Igara", trans:"以嘎拉"},
      {id:21, cat:"行动指令", cn:"起来", sw:"Selewei", trans:"塞来为"},
      {id:22, cat:"行动指令", cn:"进去", sw:"Gunjia", trans:"滚几亚"},
      {id:23, cat:"行动指令", cn:"出来", sw:"Toka", trans:"到嘎"},
      {id:24, cat:"行动指令", cn:"放下", sw:"Lebo/Wega", trans:"来博"},
      {id:25, cat:"行动指令", cn:"拿起/提着", sw:"Beba", trans:"贝巴"},
      {id:26, cat:"行动指令", cn:"快点", sw:"Fast", trans:"法斯特"},
      {id:27, cat:"行动指令", cn:"慢点", sw:"Bolebole", trans:"波累波累"},
      {id:28, cat:"行动指令", cn:"等一下", sw:"Adang", trans:"阿当"},
      {id:29, cat:"行动指令", cn:"集合", sw:"Pode", trans:"抱得"},
      {id:30, cat:"行动指令", cn:"跟着", sw:"Gogoda", trans