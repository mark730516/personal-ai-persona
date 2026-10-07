# 個人 AI 分身（簡單版）

這是一個非常簡單、適合新手的個人 AI 分身原型版本。

目標：
- 讓 AI 了解你的背景
- 讓 AI 知道你的偏好與說話風格
- 讓 AI 回答時更像你，而不是一個冷冰冰的通用助手

這個版本不需要你會太多程式，不需要高級工具，
只需要你準備兩份資料：

1. 你的自我介紹（txt）
2. 你的聊天紀錄／筆記／文章（txt）

之後用 Google Colab 點擊執行即可。

---

## 你需要準備的資料

### 1) self_intro.txt
這是你的自我介紹，內容可以這樣寫：

```text
我是一名熱愛技術與 AI 的人。
我喜歡實作，重視效率和清晰的邏輯。
我通常會先理解問題，再找落地方案。
我比較偏好簡潔、有用、有成果的回答。
我興趣在 AI、產品、程式開發、創新與自我成長。
```

### 2) memory.txt
這是你的個人記憶來源，可以放：
- WhatsApp 聊天紀錄
- Telegram 歷史
- 筆記
- 日記
- 文章
- 任何你想讓 AI 知道的內容

例如：

```text
我喜歡解決實際問題，討厭空泛建議。
我習慣先找資料，再做決策。
我不喜歡浪費時間在無效流程上。
我偏好用簡單步驟來落地事情。
我喜歡 AI 但不希望它只會說大話。
```

---

## Google Colab 使用方式

1. 打開 Google Colab
2. 建立一個新的 Notebook
3. 把下面的程式碼貼到第一個 Cell
4. 按執行
5. 接著上傳 `self_intro.txt` 和 `memory.txt`
6. 之後就可以聊天測試

---

## Colab 程式碼

```python
# ===== 1. 安裝套件 =====
!pip install -q google-generativeai pandas

# ===== 2. 設定 API Key =====
# 請先到 https://ai.google.dev/ 建立 API Key
# 然後貼到下面
API_KEY = "YOUR_GEMINI_API_KEY"

# ===== 3. 匯入函式 =====
import os
import re
from google import generativeai as genai
from google.colab import files

# 設定 Gemini
genai.configure(api_key=API_KEY)
model = genai.GenerativeModel("gemini-1.5-flash")

# ===== 4. 上傳資料 =====
print("請上傳 self_intro.txt")
uploaded = files.upload()

for fn in uploaded.keys():
    if "self_intro" in fn.lower():
        with open(fn, "r", encoding="utf-8") as f:
            self_intro = f.read()
    else:
        pass

print("請上傳 memory.txt")
uploaded2 = files.upload()

for fn in uploaded2.keys():
    if "memory" in fn.lower():
        with open(fn, "r", encoding="utf-8") as f:
            memory = f.read()
    else:
        pass

# ===== 5. 生成角色指令 =====
role_prompt = f"""
你現在是一個個人 AI 分身，請依照以下資料模仿這個人的風格。

自我介紹：
{self_intro}

個人記憶：
{memory}

請遵守以下規則：
1. 回答時要像這個人說話，而不是通用 AI
2. 保持語氣自然、真實、和這個人一致
3. 你要知道這個人的偏好、價值觀與思考方式
4. 如果你不確定，就先依照這個人的風格做合理回答
5. 回答時可以簡潔，但要有實際內容
6. 不要說你是機器人

你的目標是：
- 幫這個人處理問題
- 給出貼近這個人觀點的建議
- 讓對話感覺像是真正的他/她在說話
"""

# ===== 6. 對話函式 =====
def ask_ai(question):
    full_prompt = role_prompt + "\n\n使用者問題：\n" + question
    response = model.generate_content(full_prompt)
    return response.text

# ===== 7. 測試對話 =====
print("AI 分身已啟動。請開始問問題：")
while True:
    q = input("你：")
    if q.strip() == "exit":
        break
    ans = ask_ai(q)
    print("AI：")
    print(ans)
    print("\n---\n")
```

---

## 這個版本的優點

- 非常簡單
- 不需要高深程式知識
- 直接在 Google Colab 運行
- 能把你的歷史資料轉成個人化對話風格
- 很適合最初的原型驗證

---

## 這個版本的限制

這個版本沒有做：
- 真正的 vector database
- 長期記憶系統
- 自動讀取多份資料
- 圖像/語音分身
- 真正的神經網路微調

但它足夠你先做出一個可運行的「個人 AI 分身雛形」。

---

## 下一步建議

當這個版本跑起來後，我們可以再進階：

1. 加入多份資料（聊天紀錄、筆記、文章）
2. 加入向量資料庫（更像真實記憶系統）
3. 加入自我介紹與風格分析
4. 加入語音與圖像分身

---

## 你可以怎麼做

在本地端 IDE 或 Google Colab 中：

- 建立 `self_intro.txt`
- 建立 `memory.txt`
- 複製上面的 Colab 程式碼
- 按執行

就可以開始測試你的 AI 分身。

---

如果你願意，我下一步可以直接幫你做：

1. `self_intro.txt` 範本
2. `memory.txt` 範本
3. 讓你更像「你」的語氣模板
4. 一個更自然的 Gemini 版本

你可以直接回我：
- 「幫我寫 self_intro.txt 範本」
- 或「幫我做更像我說話的版本」

我會接著補上。