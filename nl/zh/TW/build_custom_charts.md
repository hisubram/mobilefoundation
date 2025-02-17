---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-22"

keywords: mobile analytics, charts, app sessions, crashes, graph

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# 建置自訂圖表
{: #build_custom_charts}

Mobile Analytics 主控台中的「自訂圖表」視圖可讓您針對所擷取和儲存的分析資料，彈性建置自己的視覺化內容。自訂圖表有助於從現成的內容進一步深化見解，甚至可以將其無縫擴展至針對自訂資料制定的商業分析。

在此視圖中，您可以選擇任何一個支援的分析資料集，然後選取其中一種支援的圖表類型來繪製資料集。您甚至可以定義要套用至所繪製資料的過濾器，進一步刪改視覺化內容。  

支援的資料集：
 * 應用程式日誌
 * 應用程式階段作業
 * 自訂資料
 * 網路交易

支援的圖表類型：
 * 長條圖
 * 流程圖
 * 折線圖
 * 度量群組
 * 圓餅圖
 * 表格

所選取的資料集、所要繪製的圖表類型、圖表特徵的定義，以及所要套用的資料過濾器，都可以一起拉入「自訂圖表定義」中並儲存。您可以依需要建立並儲存任意數量的「自訂圖表」定義。所儲存的「自訂圖表」會顯示在「自訂圖表」視圖上，其中會繪製相關的分析資料。

## 建立自訂圖表
{: #creating_custom_chart}

請使用下列步驟來建立自訂圖表：

1.  從 Mobile Analytics 儀表板，按一下**自訂圖表**標籤中的**建立圖表**按鈕。
2.  在**一般設定**標籤中，選取**圖表標題**、**事件類型**及**圖表類型**。
3.  選取*事件類型* 和*圖表類型* 時，會出現**圖表定義**標籤。請使用*圖表定義*標籤，針對您先前選取的指定圖表類型來定義圖表。定義圖表之後，您可以設定圖表過濾器和圖表內容。
4.  **圖表過濾器**可用來細部調整自訂圖表。一個圖表可以定義多個過濾器。例如，定義圖表以將應用程式階段作業平均持續時間視覺化之後，如果您想要只針對特定應用程式視覺化此圖表，則可以依下列方式建立過濾器：
    * 在**內容**選取**應用程式名稱**。
    * 在**運算子**選取**等於**。
    * 在**值**選取應用程式的名稱。
    * 按一下**新增過濾器**。應用程式名稱過濾器會新增至圖表的過濾器表格中。
5.  **表格**、**長條圖**和**折線圖**圖表類型有圖表內容可用。圖表內容的目標是要加強資料呈現的方式，讓視覺效果更好。如果您建立了**表格**圖表，可設定圖表內容來定義表格頁面大小、要排序的欄位，以及欄位的排序順序。如果您建立了**長條圖**或**折線圖**圖表，可設定圖表內容來標示臨界線，為監視圖表的任何人增添參照訊框。

## 從資料日誌取得自訂見解
{: #creating_custom_chart_for_client_logs}    

如果您想要獲得更深入的自訂見解，例如瀏覽過應用程式的使用者，首先，您必須擷取相關使用者的瀏覽資訊（例如所選擇的頁面、所選取的選項或所按過的按鈕）作為自訂資料，並加以記載。如需如何記載自訂資料的相關資訊，請參閱[檢測應用程式](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app)的主題。

接著，建立自訂圖表定義，以「自訂資料」作為「事件類型」，然後選擇「圖表類型」。當您進行到定義**圖表內容**或**圖表過濾器**時，您會發現「自訂資料」的類型和值出現在下拉方框中。請針對您要尋找的見解類型選取相關的選項。  

自訂見解的深度和實用度，完全取決於您在應用程式中定義及擷取自訂資料的有效性或相關性。
{: note}

## 圖表的類型
{: #types_of_charts}

### 長條圖
{:  #bar_graph}

長條圖可透過 X 軸將數值資料視覺化。當您定義長條圖時，必須先選擇「X 軸」的值。您可以從下列可能的值中選擇。

* **時間表** - 如果您想要查看資料的趨勢（例如，應用程式階段作業平均持續時間隨著時間的變化），請選擇*時間表* 作為「X 軸」。
* **內容** - 如果您想要查看特定內容的計數分解，請選擇「內容」。如果您選擇「內容」做為「X 軸」，則代表選擇「總計」做為「Y 軸」。例如，選擇*內容* 作為「X 軸」，並且在*內容* 選擇*應用程式名稱*，可查看指定事件類型的計數，且其會依應用程式名稱分析。

定義「X 軸」的值之後，您可以定義「Y 軸」的值。如果您選擇*時間表*做為「X 軸」，可以為「Y 軸」選擇下列可能的值。

* **平均值** - 將所提供之事件類型中的數值內容平均。
* **總計** - 所提供之事件類型中的內容總計。
* **唯一** - 所提供之事件類型中的內容唯一計數。定義圖表的座標軸之後，您必須選擇「內容」的值。

### 折線圖
{:  #line_graph}

折線圖可將某些隨著時間變化的度量值視覺化。如果您想要將顯示一段時間內之趨勢的資料視覺化，這種類型的圖表會很有價值。當您建立折線圖時，所要定義的第一個值是**測量**，其可能的值如下。

* **平均值** - 將所提供之事件類型中的數值內容平均。
* **總計** - 所提供之事件類型中的內容總計。
* **唯一** - 所提供之事件類型中的內容唯一計數。定義測量方式之後，您必須選擇**內容**的值。

### 流程圖
{:  #flow_chart}

流程圖可將一個內容到另一個內容的流程分解視覺化。針對流程圖，必須設定下列內容。

* **來源** - 圖表中來源節點的值。
* **目的地** - 圖表中目的地節點的值。
* **內容** - 來源節點或目的地節點的內容值。在流程圖中，您可以看到流向目的地之各種來源的密度分解，或者相反。例如，如果您想要查看應用程式日誌的嚴重性分解，您可以定義下列值。

* 在*來源* 選取**應用程式名稱**。
* 在*目的地* 選取**記載層次**。
* 在**內容**選取應用程式的名稱。

### 度量群組
{:  #metric_group}

度量群組可用來將測量為平均值、總計數或唯一計數的單一度量值視覺化。若要定義度量群組，您必須為**測量**定義下列其中一個可能值。

* **平均值** - 將所提供之事件類型中的數值內容平均。
* **總計** - 所提供之事件類型中的內容總計。
* **唯一** - 所提供之事件類型中的內容唯一計數。定義測量方式之後，您必須選擇*內容* 的值。此度量值會顯示在度量群組中。

### 圓餅圖
{:  #pie_chart}

圓餅圖可用來將特定內容值的計數分解視覺化。例如，如果您想要查看損毀分解，請定義下列值。

* 在**事件類型**選取*應用程式階段作業*。
* 在**圖表類型**選取*圓餅圖*。
* 在*內容* 選取**結束者**。結果圓餅圖會顯示由使用者結束之應用程式階段作業的分解，而非由損毀結束的應用程式階段作業。

### 表格
{:  #table}

如果您想要查看原始資料，表格會很好用。建置表格非常簡單，只要為您要查看的原始資料新增資料欄即可。由於特定事件類型並非所有內容都需要，所以您的表格中可能會出現空值。如果您想要防止這些資料列出現在您的表格中，請在**圖表過濾器**標籤中，為特定內容新增*存在* 過濾器。
