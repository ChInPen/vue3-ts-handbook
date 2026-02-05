# Vue 3 + TypeScript 實戰技術筆記庫

這是我在開發過程中針對 **Vue 3 (Composition API)** 與 **TypeScript** 的深度總結。旨在透過系統化的歸檔，提升開發效率並確保代碼品質。

---

## 技術棧 (Tech Stack)

![Vue3](https://img.shields.io/badge/vue-%2335495e.svg?style=for-the-badge&logo=vuedotjs&logoColor=%234FC08D)
![TypeScript](https://img.shields.io/badge/typescript-%23007acc.svg?style=for-the-badge&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/vite-%23646CFF.svg?style=for-the-badge&logo=vite&logoColor=white)
![Pinia](https://img.shields.io/badge/Pinia-35495e?style=for-the-badge&logo=vue.js&logoColor=4FC08D)

---

## 核心技術對照表 (Key API & Logic)

| 功能            | 類型       | 說明                   | 關鍵邏輯                           |
| :-------------- | :--------- | :--------------------- | :--------------------------------- |
| **ref**         | 響應式數據 | 基本型別與物件包裝     | 需透過 .value 存取，TS 推導直覺    |
| **watch**       | 監聽器     | 觀察數據變化執行副作用 | 支援 oldValue 對比，適合非同步操作 |
| **computed**    | 計算屬性   | 衍生數據計算           | 具備緩存機制，效能優化首選         |
| **defineModel** | 編譯器宏   | 父子組件雙向綁定       | Vue 3.4+ 推薦，大幅減少 emit 代碼  |
| **onMounted**   | 生命週期   | 組件掛載後執行         | 常用於 DOM 操作與初始化 API 請求   |

---

## 筆記分類 (Directory)

### 1. [Reactivity 基礎](./Basic/Reactivity.md)

- ref vs reactive 的決策權衡。
- 為什麼我傾向於在 TS 專案中「全 ref 化」。

### 2. [TypeScript 實戰型別](./TypeScript/TypeDefinition.md)

- 如何定義 API 回傳資料的 Interface。
- defineProps 與 defineEmits 的型別安全實踐。

### 3. [進階開發技巧](./Advanced/BestPractices.md)

- 使用 defineModel 簡化組件溝通。
- 封裝高複用性的 Custom Composables (useHooks)。

---

## 我的開發哲學

- **類型優先**：先定義 Interface，再撰寫邏輯，利用 TS 的強制力減少 Bug。
- **邏輯抽離**：善用 Composition API 將 UI 與業務邏輯分離，提高可測試性。
- **持續迭代**：關注 Vue 3.4+ 新特性（如 `defineModel`），保持技術棧的先進性。

---

## 聯絡我

- **Email**: [azse0225@gmail.com]
- **LinkedIn**: [https://www.linkedin.com/in/金瀅-lin-jin-ying-林-9b8b05362]
