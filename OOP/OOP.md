### 物件導向
具有物件概念的程式設計，物件是類別的實體，也是程式的基本單元，三大概念為封裝、繼承、多型，提高軟體的資料隱蔽性、重複利用性、擴充性，類別及函式之間可彼此相關，並將程式間模組化，提升可維護性

### 封裝
將函式內部實際運作情形隱藏起來，防止外界呼叫端去存取內部實作細節的手段

* 外部使用者可以透過公開函式內的方法或是成員存取封裝內的方法，但是使用者無法得知內部的實際運作情形，提高資料隱蔽性


### 繼承
類別間可建立垂直的親屬關係，父類別把私有函式以外的方法及屬性傳承給建立的子類別，提高重複利用性

### 多型
多型如同平行的親屬關係，在函式之間或是類別之間的擴充性，主要兩個概念為多載(overloading)及覆寫(override)

* 多載適合用於相同函式名稱但有不同的方法，函式可以使用統一的介面，根據不同類型、不同數量的傳參數來給予不同的回應

* 覆寫在於不同類別之間，相同函式名稱但需要不同方法，即子類別可以覆寫掉父類別原有的方法

### 物件
物件導向程式設計的基本單位，類別的實體

### 類別
包含物件的屬性以及方法，為物件的藍圖

### 建構子
類別裡用於建立物件的特殊子程式

### 解構子
物件的生命周期結束時會自動呼叫執行，清空或釋放物件占用的記憶體資源

### 方法
物件的行為
[ C++ ]
### 抽象類別（Abstract class）
1. 一個類別之中至少有一個虛擬函式，此類別就稱為抽象類別
2. 抽象類別無法產生物件(無法實體化)
3. 只能用來被繼承

### 虛擬函式（Virtual）
此函式及方法可以被子類別繼承或是覆蓋，虛擬函式可以給出目標的定義，但不具體實作(只規範但不實作)

### 介面（Interface）
把實作與呼叫方法分開，封裝特定功能的集合稱為介面

### Abstract class vs interface
* Abstract class具有自己的屬性，同時擁有不具備詳細定義的抽象方法，抽象方法在繼承類別中實作
* interface不具有自己的屬性，只擁有不具備詳細定義的抽象方法，抽象方法在實作類別中實作

### 模板（Template）
當函數內容一樣，參數資料型態不同時，可利用模板實現通用函數，由編譯器透過類似巨集代換的方式，根據樣板內容產生實際的程式碼

[ Python ]

### lambda
匿名函式，只能一行，自動回傳結果，執行運算式時將會產生函式物件

```Python
mul = lambda x,y : x * y
print(4,2)
### output:8
```

### super
呼叫父類別的方法，用於多重繼承
