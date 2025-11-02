# 🏛️ NPMC 國政模擬中心
## 完整開發計劃與技術文檔

---

## 📋 目錄
1. [專案概述](#專案概述)
2. [Phase 2: 核心功能開發](#phase-2)
3. [Phase 3: 技術實現](#phase-3)
4. [系統架構設計](#系統架構)
5. [安全與穩定性](#安全與穩定性)
6. [官方網站規劃](#官方網站)
7. [開發時程表](#時程表)

---

## 🎯 專案概述 {#專案概述}

### 產品定位
NPMC是一個**遊戲化學習生態系統**,結合:
- 📚 監控式陪讀系統
- 🎮 多元遊戲世界
- 👥 社交互動平台
- 🏆 積分獎勵機制
- 💰 付費增值服務

### 核心價值
> "讓學習不再孤單,讓成長充滿樂趣"

---

## ✅ Phase 2: 核心功能開發 {#phase-2}

### 2.1 戰爭遊戲3D介面

#### 基礎版 (1-2個月)
```
功能清單:
✓ 國家選擇系統 (6個國家)
✓ 回合制戰略玩法
✓ 資源管理 (金錢、軍事、科技、人口)
✓ 簡易地圖系統 (2D平面)
✓ 戰鬥結算動畫
```

#### 進階版 (3-6個月)
```
升級功能:
✓ 3D地球儀視角 (Three.js)
✓ 即時戰略模式
✓ 外交系統 (結盟、談判)
✓ 多人對戰模式
✓ 排行榜系統
✓ 錄像回放功能
```

**技術需求:**
- 前端: React + Three.js / Unity WebGL
- 後端: Node.js + Socket.io (即時對戰)
- 數據庫: MongoDB (玩家數據、戰績)

---

### 2.2 寵物世界系統

#### 基礎功能
```
核心機制:
✓ 4種寵物選擇 (兔子、狗、貓、熊貓)
✓ 屬性系統:
  - 飽食度 (每小時-10)
  - 快樂度 (互動+20)
  - 等級 (經驗值升級)
  - 體力 (影響活動力)
  
✓ 基礎互動:
  - 餵食 (消耗積分/道具)
  - 玩耍 (提升快樂度)
  - 清潔 (維持衛生)
  - 睡覺 (恢復體力)
```

#### 進階功能
```
社交系統:
✓ 棲息地探索 (大草原、動物園、竹林等)
✓ 遇見其他玩家寵物
✓ 寵物社交互動:
  - 打招呼
  - 一起玩耍
  - 交換禮物
  - 寵物對戰 (友誼賽)
  
✓ 繁殖系統:
  - 配對機制
  - 遺傳系統 (顏色、特質)
  - 稀有品種培育
  
✓ 寵物成就:
  - 參加競賽
  - 解鎖稱號
  - 專屬裝扮
```

**技術需求:**
- 動畫: CSS/Canvas動畫
- 多人互動: WebSocket即時通訊
- AI行為: 簡易AI決策樹

---

### 2.3 好友與通訊系統

#### 私人通話
```
功能:
✓ 一對一語音通話
✓ 一對一視訊通話
✓ 通話中文字訊息
✓ 通話記錄
✓ 封鎖/靜音功能
```

#### 多人通話
```
功能:
✓ 支援2-50人同時通話
✓ 視訊網格布局
✓ 舉手發言機制
✓ 主持人控制 (靜音全員)
✓ 螢幕分享功能
```

#### 組隊讀書
```
特色功能:
✓ 讀書房間創建
✓ 番茄鐘同步倒數
✓ 組隊積分加成 (+10%)
✓ 集體目標挑戰
✓ 隊伍排行榜
```

**技術需求:**
- WebRTC (P2P通話)
- Agora/Twilio API (多人通話)
- Socket.io (即時狀態同步)

---

### 2.4 積分商城系統

#### 商品分類
```
1. 遊戲裝備 (戰爭模式)
   - 進階武器包 (500積分)
   - 科技研發加速 (300積分)
   - 資源禮包 (200積分)

2. 寵物道具
   - 超級飼料 (200積分)
   - 寵物玩具 (300積分)
   - 稀有裝扮 (500積分)

3. 讀書工具
   - 專注藥水 (2倍積分/1小時) (400積分)
   - 時間延長卡 (100積分)
   - 干擾屏蔽器 (600積分)

4. 虛擬獎勵
   - 頭像框 (150積分)
   - 特殊稱號 (250積分)
   - 動態背景 (350積分)

5. 實體獎勵
   - 7-11禮券$50 (500積分)
   - 手搖飲料券 (300積分)
   - 文具組 (400積分)
```

#### 積分不足處理
```javascript
function checkPurchase(itemPrice, userPoints) {
  if (userPoints < itemPrice) {
    const shortage = itemPrice - userPoints;
    const hoursNeeded = Math.ceil(shortage / 10); // 每小時10積分
    
    showAlert({
      title: "積分不足!",
      message: `
        需要: ${itemPrice}積分
        當前: ${userPoints}積分
        差額: ${shortage}積分
        
        建議: 前往讀書模式讀書約${hoursNeeded}小時即可獲得足夠積分!
      `,
      buttons: ["前往讀書", "取消"]
    });
  }
}
```

---

### 2.5 監控與反作弊系統

#### 真實監控實現

##### 錄音監控
```javascript
// 麥克風權限請求
async function requestAudioPermission() {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ 
      audio: {
        echoCancellation: true,
        noiseSuppression: true,
        autoGainControl: true
      } 
    });
    
    // 音量偵測
    const audioContext = new AudioContext();
    const analyser = audioContext.createAnalyser();
    const microphone = audioContext.createMediaStreamSource(stream);
    microphone.connect(analyser);
    
    // 檢測是否有環境音(說話、移動等)
    checkAudioActivity(analyser);
    
    // 上傳音頻片段到服務器(每5分鐘)
    uploadAudioSample(stream);
    
  } catch (error) {
    console.error('麥克風權限被拒絕');
  }
}
```

##### 視訊監控
```javascript
// 視訊監控實現
async function startVideoMonitoring() {
  const stream = await navigator.mediaDevices.getUserMedia({ 
    video: {
      width: { ideal: 640 },
      height: { ideal: 480 },
      facingMode: 'user'
    } 
  });
  
  const video = document.getElementById('monitorVideo');
  video.srcObject = stream;
  
  // AI人臉偵測
  setInterval(() => {
    detectFace(video); // 確認使用者在鏡頭前
    detectEyeGaze(video); // 檢測視線方向
    captureSnapshot(video); // 每10分鐘截圖一次
  }, 10000);
}
```

##### 作弊偵測機制
```
自動偵測:
✓ 離開視窗超過30秒 → 暫停計時
✓ 視訊畫面無人臉 → 警告
✓ 環境音過於安靜(可能睡著) → 檢測
✓ 視線長時間偏離 → 提示
✓ 多次切換應用程式 → 記錄

人工審核:
✓ 隨機抽查錄像片段
✓ 舉報系統處理
✓ 異常積分增長調查
```

---

## 🔧 Phase 3: 技術實現 {#phase-3}

### 3.1 免責聲明與法律文件

#### 使用者條款
```markdown
# NPMC平台使用條款

## 1. 服務說明
本平台提供遊戲化學習服務,包括但不限於監控式讀書、
遊戲功能、社交互動等。

## 2. 監控聲明
⚠️ 使用讀書模式時,系統將:
- 錄製視訊畫面
- 錄製環境音頻
- 截取螢幕畫面
- 追蹤應用程式使用

所有錄製內容僅用於驗證學習真實性,
保存期限7天後自動刪除。

## 3. 隱私權政策
我們承諾:
✓ 不公開個人錄像
✓ 不販售使用者數據
✓ 僅在必要時(違規調查)人工審核
✓ 可隨時要求刪除個人資料

## 4. 付費服務
付費功能為自願購買,不影響基本使用。
付費後恕不退款,詳見退款政策。

## 5. 違規處理
嚴禁:
❌ 使用外掛程式
❌ 作弊刷積分
❌ 騷擾其他使用者
❌ 散布不當內容

違規將:
第1次 → 警告
第2次 → 停權7天
第3次 → 永久封禁

## 6. 免責聲明
✓ 平台不保證學習成效
✓ 網路斷線不負賠償責任
✓ 第三方服務問題不負責
✓ 使用者自行負責資訊安全

## 7. 年齡限制
✓ 12歲以下需家長同意
✓ 18歲以下付費需監護人授權

最後更新: 2025年11月
```

---

### 3.2 權限管理系統

#### Android權限請求
```xml
<!-- AndroidManifest.xml -->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

#### iOS權限描述
```xml
<!-- Info.plist -->
<key>NSCameraUsageDescription</key>
<string>需要相機權限以進行讀書監控</string>

<key>NSMicrophoneUsageDescription</key>
<string>需要麥克風權限以偵測學習狀態</string>

<key>NSPhotoLibraryUsageDescription</key>
<string>需要相簿權限以上傳學習記錄</string>
```

#### 權限管理流程
```
首次開啟APP:
1. 顯示歡迎畫面
2. 說明需要權限原因
3. 逐步請求權限
4. 允許用戶跳過(限制功能)

使用特定功能時:
1. 檢查權限狀態
2. 若未授權 → 彈出說明視窗
3. 導向系統設定
4. 返回APP自動重試
```

---

### 3.3 多平台APP開發

#### 技術棧選擇

**方案A: React Native (推薦)**
```
優點:
✓ 一套代碼支援iOS + Android
✓ 熱更新能力
✓ 龐大生態系
✓ 接近原生性能

缺點:
✗ 複雜功能需原生模組
✗ 包大小較大

適用: 快速開發,成本有限
```

**方案B: Flutter**
```
優點:
✓ 極致性能
✓ 精美UI
✓ 一套代碼多平台

缺點:
✗ 學習曲線陡峭
✗ 生態較React Native小

適用: 追求性能與體驗
```

**方案C: 原生開發**
```
優點:
✓ 最佳性能
✓ 完整平台功能
✓ 無相容性問題

缺點:
✗ 開發成本高(需兩套代碼)
✗ 維護困難

適用: 大型團隊,長期專案
```

#### 平台支援規劃
```
Phase 1: 網頁版 (PWA)
- 支援所有瀏覽器
- 可安裝到桌面
- 離線基本功能

Phase 2: 移動端
- Android 8.0+ 
- iOS 13.0+
- 平板電腦優化

Phase 3: 桌面端
- Windows 10/11
- macOS 11+
- Linux (Ubuntu 20.04+)
- 使用Electron框架
```

---

### 3.4 後端數據儲存設計

#### 數據庫選擇

**主數據庫: PostgreSQL**
```sql
-- 使用者表
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(100) UNIQUE,
  points INTEGER DEFAULT 0,
  is_premium BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT NOW()
);

-- 學習記錄表
CREATE TABLE study_sessions (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id),
  start_time TIMESTAMP NOT NULL,
  end_time TIMESTAMP,
  duration INTEGER, -- 秒數
  points_earned INTEGER,
  is_verified BOOLEAN DEFAULT FALSE,
  video_url VARCHAR(255),
  created_at TIMESTAMP DEFAULT NOW()
);

-- 寵物表
CREATE TABLE pets (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id),
  pet_type VARCHAR(20),
  name VARCHAR(50),
  level INTEGER DEFAULT 1,
  hunger INTEGER DEFAULT 100,
  happiness INTEGER DEFAULT 100,
  created_at TIMESTAMP DEFAULT NOW()
);

-- 交易記錄表
CREATE TABLE transactions (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id),
  type VARCHAR(20), -- 'earn', 'spend', 'purchase'
  amount INTEGER,
  description TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

**快取層: Redis**
```
用途:
✓ Session管理
✓ 即時排行榜
✓ 在線用戶列表
✓ 積分暫存(每5分鐘同步DB)
```

**文件儲存: AWS S3 / Cloudinary**
```
儲存內容:
✓ 使用者頭像
✓ 寵物截圖
✓ 學習監控錄像(7天後刪除)
✓ 遊戲資源(圖片、音效)
```

---

### 3.5 離線功能實現

#### Service Worker策略
```javascript
// 離線優先策略
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      // 有快取就用快取
      if (response) return response;
      
      // 沒有就從網路獲取
      return fetch(event.request).then((response) => {
        // 快取新資源
        const responseClone = response.clone();
        caches.open('npmc-v1').then((cache) => {
          cache.put(event.request, responseClone);
        });
        return response;
      });
    })
  );
});
```

#### 離線功能清單
```
可離線使用:
✓ 查看個人資料
✓ 寵物互動(本地計算)
✓ 單人遊戲模式
✓ 讀書計時(本地記錄)
✓ 查看已快取內容

需要網路:
✗ 即時對戰
✗ 好友聊天
✗ 積分同步
✗ 排行榜
✗ 購買商品
```

#### 資料同步機制
```javascript
// 網路恢復時自動同步
window.addEventListener('online', () => {
  syncOfflineData();
});

async function syncOfflineData() {
  const offlineData = await getLocalStorage('pendingSync');
  
  for (const record of offlineData) {
    try {
      await api.post('/sync', record);
      removeLocalRecord(record.id);
    } catch (error) {
      console.error('同步失敗:', record);
    }
  }
}
```

---

## 🛡️ 安全與穩定性 {#安全與穩定性}

### 穩定性保證

#### 高並發處理
```
架構設計:
✓ 負載均衡器 (Nginx)
✓ 水平擴展 (Docker + Kubernetes)
✓ CDN加速 (CloudFlare)
✓ 數據庫讀寫分離

目標:
→ 支援10萬人同時在線
→ API響應時間 < 200ms
→ 99.9%可用時間
```

#### 監控系統
```
工具:
✓ Sentry (錯誤追蹤)
✓ DataDog (性能監控)
✓ Grafana (視覺化儀表板)

指標:
- CPU使用率
- 記憶體使用率
- API請求數/秒
- 錯誤率
- 使用者在線數
```

---

### 反外掛與安全

#### 外掛偵測
```javascript
// 前端檢測
function detectCheating() {
  // 檢測開發者工具
  if (isDevToolsOpen()) {
    reportSuspiciousActivity('devtools_detected');
  }
  
  // 檢測自動化工具
  if (navigator.webdriver) {
    reportSuspiciousActivity('automation_detected');
  }
  
  // 檢測異常積分增長
  if (pointsGainedPerHour > 100) {
    flagAccount('unusual_points_gain');
  }
}

// 後端驗證
function verifyStudySession(session) {
  // 檢查時間戳是否合理
  if (session.duration > 14400) { // 4小時
    return false;
  }
  
  // 檢查是否有視訊紀錄
  if (!session.video_url) {
    return false;
  }
  
  // AI分析視訊內容
  const aiResult = analyzeVideo(session.video_url);
  if (aiResult.faceDetected < 0.8) {
    return false;
  }
  
  return true;
}
```

#### 付費外掛功能(反制違規者)
```
反制工具套件 (VIP專屬):
1. 超級舉報 
   - 檢舉後優先審核
   - 提供證據截圖/錄影
   
2. 黑名單功能
   - 自動過濾黑名單用戶
   - 不會匹配到違規者
   
3. 保護模式
   - 優先服務器資源
   - 防止惡意攻擊影響
   
價格: 包含在VIP套餐中
```

#### 違規處理流程
```
自動處理:
第1次檢測到作弊 → 系統警告
第2次 → 停權24小時
第3次 → 停權7天
第4次 → 永久封禁

人工審核:
✓ 嚴重違規 → 立即永ban
✓ 惡意程式碼攻擊 → IP封鎖
✓ 付費詐騙 → 法律追訴
```

---

### 資料加密
```
傳輸加密:
✓ HTTPS/TLS 1.3
✓ WebSocket安全連接(WSS)
✓ API密鑰加密

儲存加密:
✓ 密碼 bcrypt加密
✓ 敏感資料AES-256加密
✓ 付款資訊不儲存(金流代理)
```

---

## 🌐 官方網站規劃 {#官方網站}

### 網站架構
首頁 (Landing Page)
├── 品牌介紹
├── 核心功能展示
├── 下載區
└── 最新消息

功能介紹頁
├── 讀書模式
├── 遊戲世界
├── 社交系統
└── 獎勵機制

支援中心
├── 使用教學
├── 常見問題
├── 聯繫我們
└── 問題回報

社群
├── 官方部落格
├── 排行榜
├── 成就展示
└── 活動公告

關於我們
├── 團隊介紹
├── 理念願景
├── 合作夥伴
└── 媒體報導

法律文件
├── 使用條款
├── 隱私政策
├── 免責聲明
└── 退款政策

**版本:** v1.0
**最後更新:** 2025-11-02
**文檔作者:** Claude for NPMC Team
