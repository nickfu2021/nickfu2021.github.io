---
layout: post
title:  "[LeetCode]學習心得-運用Dictionary"
date:   2025-05-15 12:00:00 +0800
---

最近剛剛開始練習LeetCode，做的時候就會一直想之前維護的程式哪些效能可以再改進，下面是跟 chatgpt 聊了一下得出的結論，這邊單純做個記錄有興趣的人也可參考看看


# 空間換時間：Dictionary 優化筆記

## 問題背景
原始程式每次使用 DataTable.Select 來搜尋資料，  
會造成 O(n * m) 的時間複雜度，  
尤其當輸入資料量大時效能明顯變差。

---

## 優化原則
- **空間換時間**：預先把 DataTable 資料做成 Dictionary 快速查找表。
- **時間複雜度**：
  - 原始：O(n * m)
  - 優化：O(n + m)

---

## 原始邏輯 (雙迴圈)

```vbnet
For jCnt = 0 To DataList1.length - 1
    vmtno1 = DataList1(jCnt).childnodes(0).text
    vqty1 = DataList1(jCnt).childnodes(1).text

    For Each dr In QUT0112.Select("$QUMTNO='" & vmtno1 & "'")
        ' 處理邏輯
    Next
Next
```

---

## 優化後邏輯 (預處理索引 + 單層查找)

```vbnet
' 建立 QUT0112 資料字典
Dim qut0112Map As New Dictionary(Of String, List(Of DataRow))
For Each dr In QUT0112.Rows
    Dim key As String = dr("$QUMTNO").ToString().Trim()
    If Not qut0112Map.ContainsKey(key) Then
        qut0112Map(key) = New List(Of DataRow)()
    End If
    qut0112Map(key).Add(dr)
Next

' 依序處理輸入資料
For jCnt = 0 To DataList1.length - 1
    vmtno1 = DataList1(jCnt).childnodes(0).text
    vqty1 = DataList1(jCnt).childnodes(1).text

    If qut0112Map.ContainsKey(vmtno1) Then
        For Each dr In qut0112Map(vmtno1)
            ' 處理邏輯
        Next
    End If
Next
```

---

## 成效分析
- 原始運算次數：約 158 * DataList1.length
- 優化運算次數：約 158 + DataList1.length
- 適合資料量大或需要多次查找的情境

---

## 工程實務建議
1. 當效能成為瓶頸時，適當犧牲空間提升查找效率。
2. 使用 Dictionary 提高查找速度。
3. 可進一步封裝成共用方法，提高可維護性。

---

## 成本與效益取捨

### 成本
- **程式碼變多**：需要額外建立 Dictionary 與額外邏輯，程式看起來稍微變複雜。
- **佔用更多記憶體**：將 DataTable 資料額外存放在 Dictionary 結構中。

### 效益
- **效能大幅提升**：從 O(n * m) 降低為 O(n + m)，處理大量資料時速度明顯提升。
- **程式邏輯更清晰**：先建索引，再查找，符合「先準備、後查詢」的工程習慣。
- **可維護性良好**：只要命名清晰、封裝得當，未來可重用、易讀性佳。

![test](https://i.ibb.co/FkV4ZGY7/annotated-merge-sorted-lists.jpg)

### 工程最佳實踐
> 當**效能比程式碼簡潔更重要時**，這樣的取捨是值得的。  
> 記憶體在現代環境相對便宜，但 CPU 運算與使用者體驗更為關鍵。

---
