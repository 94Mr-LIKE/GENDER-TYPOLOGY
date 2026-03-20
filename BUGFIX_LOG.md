# Gender Typology v5.2 — Bug 修復紀錄

> 修復日期：2026-03-20

## 修復清單

### BUG-01：SSR falsy 值問題（嚴重）
```javascript
// 修改前
var SSR = (ans.Q39 || 2) * 2 + 1.5;

// 修改後
var SSR = (ans.Q39 != null ? ans.Q39 : 2) * 2 + 1.5;
```
選「1-2 分」時 Q39=0 被 JS 當 falsy，SSR 被錯算為 5.5 而非 1.5。

### BUG-02：內耗指數分母錯誤
```javascript
// 修改前
var stressPct = Math.round(stress / 24 * 100);

// 修改後
var stressPct = Math.round(stress / 22 * 100);
```
Q35/Q37 最大值為 3，實際總分上限 22，分母 24 導致永遠到不了 100%。

### BUG-03：Q29 轉換表非線性跳躍
```javascript
// 修改前
L3.RS_R = [0, 50, 75, 100][parseInt(ans.Q29) || 0];

// 修改後
L3.RS_R = [0, 33, 67, 100][parseInt(ans.Q29) || 0];
```
與其他三軸的均勻分布一致。

### BUG-04：numb 診斷攔截 shift 案例
```javascript
// 修改前
else if (alpha >= 60 && stressPct <= 33 && SSR >= 6) diag = 'numb';

// 修改後
else if (mm === 0 && alpha >= 60 && stressPct <= 33 && SSR >= 6) diag = 'numb';
```
加入 `mm === 0` 條件，確保有軸線偏移的人能看到偏移診斷。
