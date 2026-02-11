# 邏輯抽離實戰：從 Options API 到 Composables

在 Vue 3 開發中，透過 **Composition API** 實現邏輯抽離是專業開發者的標配。這不僅解決了代碼冗長問題，更透過 **TypeScript 泛型** 實現了強大的型別安全性。

---

## 為什麼需要邏輯抽離？

1. **關注點分離**：UI 組件只負責顯示，業務邏輯交給 Composable。
2. **狀態複用**：Loading、Error、Data 賦值邏輯不需要在每個組件重寫。
3. **主控權交接**：將 API 執行的權力交給 Hook，實現自動化的狀態切換與重新請求功能。

---

## 一體化全流程範例：從型別協議到組件實踐

以下展示如何定義一個萬用的資料讀取器（useDataLoader），並將 API 實作與邏輯封裝完美結合。

```typescript
/**
 * [1. 型別協議定義]
 * 作為前後端的通訊協議。
 * 若後端格式不符，應在 API 實作層 (Service Layer) 進行資料轉換 (Mapping)，
 * 確保 Composable 與組件層接收到的數據始終保持一致，降低維護成本。
 */
export interface ApiResponse<T> {
  data: T;
  success: boolean;
  message: string;
  [K: string]: any; //允許後端有其他欄位回傳
}

// 具體的資料型別（例如：用戶資訊）
export interface UserProfile {
  id: number;
  username: string;
  email: string;
}

/**
 * [2. 萬用讀取器封裝 (The Composable)]
 * 接收一個「動作(apiCall)」，負責管理其載入狀態與資料容器
 */
import { ref, type Ref } from "vue";

export function useDataLoader<T>(apiCall: () => Promise<ApiResponse<T>>) {
  // data 裝載結果，loading 是轉圈圈狀態，error 是故障訊息
  const data = ref<T | null>(null) as Ref<T | null>;
  const loading = ref(false);
  const error = ref<string | null>(null);

  // 定義執行動作，讓組件可以隨時主動觸發（或重複觸發）
  const execute = async () => {
    loading.value = true; // 啟動轉圈圈
    error.value = null; // 清空舊錯誤
    try {
      const res = await apiCall(); // 執行傳入的規範函式
      if (res.success) {
        data.value = res.data; // 成功後將資料存入響應式容器
      } else {
        error.value = res.message;
      }
    } catch (err) {
      error.value = "系統連線異常";
    } finally {
      loading.value = false; // 關閉轉圈圈
    }
  };

  return { data, loading, error, execute };
}
/**
 * [3. API實作 ]
 * 使用 Axios 進行網路請求，並將結果「裝進」ApiResponse 規範中
 */

export const createUser = async (
  newUser: Partial<UserProfile>,
): Promise<ApiResponse<UserProfile>> => {
  try {
    // 這裡我們直接對應後端的 ApiResponse 格式
    const response = await axios.post<ApiResponse<UserProfile>>(
      "這裡填入URL",
      newUser,
    );
    // 直接回傳後端給我們的整包資料
    // response.data 裡面就包含了後端的 { data, success, message }
    return response.data;
  } catch (error: any) {
    return {
      data: null as any,
      success: false,
      message: error.response?.data?.message || "網路連線異常",
    };
  }
};

/**
 * [4. 實際應用]
 * 在組件層，我們透過一個「閉包 (Closure)」將資料封裝進 API 動作中
 */

<script setup lang="ts">
import {ref} from "vue"
import {useDataLoader} from "./composables/useDataLoader"
import {createUser type UserProfile} from "./api"

const form = ref<Partial<UserProfile>>({
  username: '',
  email: ''
});

const apiData = useDataLoader( () => createUser(form.value) )
const { data, loading, error, execute } = apiData
const handleRegister = async () => {
  await execute();
  if (data.value) {
    alert("註冊成功!");
  }
};
</script>
```
