<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"
  />

  <title>军警指挥 100词 - 斯瓦希里语</title>

  <meta name="theme-color" content="#1e3a8a" />

  <style>
    :root {
      --primary: #1e3a8a;
      --card: #1e2937;
    }

    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      margin: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont,
        "Segoe UI", Roboto, sans-serif;
      background: #0f172a;
      color: #e2e8f0;
      line-height: 1.5;
      overflow-x: hidden;
    }

    header {
      background: linear-gradient(135deg, #1e3a8a, #3b82f6);
      padding: 1.2rem;
      text-align: center;
      position: sticky;
      top: 0;
      z-index: 100;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
    }

    header h1 {
      margin: 0;
      font-size: 1.7rem;
    }

    header p {
      margin: 6px 0 0;
      opacity: 0.9;
    }

    .tab-bar {
      display: flex;
      overflow-x: auto;
      background: #1e2937;
      padding: 10px 8px;
      gap: 8px;
      scrollbar-width: none;
    }

    .tab-bar::-webkit-scrollbar {
      display: none;
    }

    .tab {
      padding: 10px 18px;
      white-space: nowrap;
      border-radius: 9999px;
      background: #334155;
      font-size: 15px;
      transition: all 0.2s;
      cursor: pointer;
      user-select: none;
    }

    .tab.active {
      background: var(--primary);
      color: white;
      font-weight: 600;
    }

    .container {
      padding: 16px;
    }

    .search {
      width: 100%;
      padding: 15px 18px;
      border-radius: 9999px;
      border: none;
      background: #1e2937;
      color: white;
      font-size: 17px;
      margin-bottom: 16px;
      outline: none;
    }

    .card {
      background: var(--card);
      border-radius: 16px;
      padding: 18px;
      margin-bottom: 12px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
      transition: all 0.2s;
      cursor: pointer;
    }

    .card:hover {
      transform: translateY(-2px);
    }

    .card:active {
      transform: scale(0.97);
    }

    .chinese {
      font-size: 1.35rem;
      font-weight: 700;
      margin: 0 0 8px 0;
    }

    .trans {
      font-size: 1.15rem;
      color: #60a5fa;
      margin: 4px 0;
    }

    .swahili {
      font-size: 0.98rem;
      color: #94a3b8;
    }

    .modal {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.95);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 1000;
      padding: 20px;
    }

    .modal-content {
      background: #1e2937;
      padding: 28px;
      border-radius: 20px;
      width: 100%;
      max-width: 420px;
      text-align: center;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
    }

    .btn {
      padding: 14px 24px;
      border-radius: 9999px;
      border: none;
      font-size: 16px;
      margin: 6px;
      cursor: pointer;
      transition: all 0.2s;
    }

    .btn:active {
      transform: scale(0.95);
    }

    .btn-primary {
      background: var(--primary);
      color: white;
      font-weight: 600;
    }

    .empty {
      text-align: center;
      color: #94a3b8;
      padding: 40px 0;
    }
  </style>
</head>

<body>

  <header>
    <h1>🛡️ 军警指挥 100词</h1>
    <p>斯瓦希里语现场快速指挥工具</p>
  </header>

  <div class="tab-bar" id="tabs"></div>

  <div class="container">

    <input
      type="text"
      id="search"
      class="search"
      placeholder="🔍 搜索：中文 / 音译 / 斯瓦希里语"
      onkeyup="filterCards()"
    />

    <div id="cards"></div>

  </div>

  <!-- 闪卡 -->
  <div class="modal" id="flashModal">

    <div class="modal-content" onclick="flipCard()">

      <h2
        id="flashChinese"
        style="font-size:2.1rem;margin:20px 0 12px;"
      ></h2>

      <p
        id="flashTrans"
        style="font-size:1.5rem;color:#60a5fa;margin:8px 0;"
      ></p>

      <p
        id="flashSwahili"
        style="font-size:1.15rem;color:#94a3b8;"
      ></p>

      <div style="margin-top:24px;">

        <button
          class="btn btn-primary"
          onclick="event.stopPropagation();speakCurrent()"
        >
          🔊 朗读
        </button>

        <button
          class="btn"
          onclick="event.stopPropagation();closeModal()"
        >
          关闭
        </button>

      </div>

    </div>

  </div>

  <script>

    const vocabulary = [

      {id:1,cat:"指挥与身份",cn:"领导/首长",sw:"Boss",trans:"波丝"},
      {id:2,cat:"指挥与身份",cn:"警察",sw:"Polisi",trans:"波利斯"},
      {id:3,cat:"指挥与身份",cn:"军人",sw:"Soldat",trans:"索哒"},
      {id:4,cat:"指挥与身份",cn:"保安",sw:"Shei",trans:"谢义"},
      {id:5,cat:"指挥与身份",cn:"我",sw:"Miye",trans:"米也"},
      {id:6,cat:"指挥与身份",cn:"你",sw:"Wei",trans:"微"},
      {id:7,cat:"指挥与身份",cn:"他",sw:"Wuyou",trans:"无有"},
      {id:8,cat:"指挥与身份",cn:"刚果人",sw:"Congoli",trans:"刚果里"},
      {id:9,cat