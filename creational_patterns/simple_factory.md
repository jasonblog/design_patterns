#簡單工廠模式( Simple Factory Pattern )


#模式動機
考慮一個簡單的軟件應用場景，一個軟件系統可以提供多個外觀不同的按鈕（如圓形按鈕、矩形按鈕、菱形按鈕等），
這些按鈕都源自同一個基類，不過在繼承基類後不同的子類修改了部分屬性從而使得它們可以呈現不同的外觀，如果我們希望在使用這些按鈕時，不需要知道這些具體按鈕類的名字，只需要知道表示該按鈕類的一個參數，並提供一個調用方便的方法，把該參數傳入方法即可返回一個相應的按鈕對象，此時，就可以使用簡單工廠模式。

#模式定義
簡單工廠模式(Simple Factory Pattern)：又稱為靜態工廠方法(Static Factory Method)模式，它屬於類創建型模式。在簡單工廠模式中，可以根據參數的不同返回不同類的實例。簡單工廠模式專門定義一個類來負責創建其他類的實例，被創建的實例通常都具有共同的父類。


#模式結構
簡單工廠模式包含如下角色：

- Factory：工廠角色
    工廠角色負責實現創建所有實例的內部邏輯
- Product：抽象產品角色
    抽象產品角色是所創建的所有對象的父類，負責描述所有實例所共有的公共接口
- ConcreteProduct：具體產品角色
    具體產品角色是創建目標，所有創建的對象都充當這個角色的某個具體類的實例。

![](../_static/SimpleFactory.jpg)

#時序圖

![](../_static/seq_SimpleFactory.jpg)

#代碼分析
```cpp
///////////////////////////////////////////////////////////
//  Factory.cpp
//  Implementation of the Class Factory
//  Created on:      01-十月-2014 18:41:33
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Factory.h"
#include "ConcreteProductA.h"
#include "ConcreteProductB.h"

Factory::Factory()
{

}


Factory::~Factory()
{

}

Product* Factory::createProduct(string proname)
{
    if ("A" == proname) {
        return new ConcreteProductA();
    } else if ("B" == proname) {
        return new ConcreteProductB();
    }

    return  NULL;
}
```


#模式分析

- 將對象的創建和對象本身業務處理分離可以降低系統的耦合度，使得兩者修改起來都相對容易。
- 在調用工廠類的工廠方法時，由於工廠方法是靜態方法，使用起來很方便，可通過類名直接調用，而且只需要傳入一個簡單的參數即可，在實際開發中，還可以在調用時將所傳入的參數保存在XML等格式的配置文件中，修改參數時無須修改任何源代碼。
- 簡單工廠模式最大的問題在於工廠類的職責相對過重，增加新的產品需要修改工廠類的判斷邏輯，這一點與開閉原則是相違背的。
- 簡單工廠模式的要點在於：當你需要什麼，只需要傳入一個正確的參數，就可以獲取你所需要的對象，而無須知道其創建細節。

#實例
(略）


#簡單工廠模式的優點
--------------------

- 工廠類含有必要的判斷邏輯，可以決定在什麼時候創建哪一個產品類的實例，客戶端可以免除直接創建產品對象的責任，而僅僅“消費”產品；簡單工廠模式通過這種做法實現了對責任的分割，它提供了專門的工廠類用於創建對象。
- 客戶端無須知道所創建的具體產品類的類名，只需要知道具體產品類所對應的參數即可，對於一些複雜的類名，通過簡單工廠模式可以減少使用者的記憶量。
- 通過引入配置文件，可以在不修改任何客戶端代碼的情況下更換和增加新的具體產品類，在一定程度上提高了系統的靈活性。

#簡單工廠模式的缺點
--------------------

- 由於工廠類集中了所有產品創建邏輯，一旦不能正常工作，整個系統都要受到影響。
- 使用簡單工廠模式將會增加系統中類的個數，在一定程序上增加了系統的複雜度和理解難度。
- 系統擴展困難，一旦添加新產品就不得不修改工廠邏輯，在產品類型較多時，有可能造成工廠邏輯過於複雜，不利於系統的擴展和維護。
- 簡單工廠模式由於使用了靜態工廠方法，造成工廠角色無法形成基於繼承的等級結構。

#適用環境
在以下情況下可以使用簡單工廠模式：

- 工廠類負責創建的對象比較少：由於創建的對象較少，不會造成工廠方法中的業務邏輯太過複雜。
- 客戶端只知道傳入工廠類的參數，對於如何創建對象不關心：客戶端既不需要關心創建細節，甚至連類名都不需要記住，只需要知道類型所對應的參數。

#模式應用
1. JDK類庫中廣泛使用了簡單工廠模式，如工具類java.text.DateFormat，它用於格式化一個本地日期或者時間。


```
public final static DateFormat getDateInstance();
public final static DateFormat getDateInstance(int style);
public final static DateFormat getDateInstance(int style,Locale
locale);
```
2. Java加密技術

獲取不同加密算法的密鑰生成器::

```
KeyGenerator keyGen=KeyGenerator.getInstance("DESede");
```

創建密碼器::

```
Cipher cp=Cipher.getInstance("DESede");
```
#總結
--------------------

- 創建型模式對類的實例化過程進行了抽象，能夠將對象的創建與對象的使用過程分離。
- 簡單工廠模式又稱為靜態工廠方法模式，它屬於類創建型模式。在簡單工廠模式中，可以根據參數的不同返回不同類的實例。簡單工廠模式專門定義一個類來負責創建其他類的實例，被創建的實例通常都具有共同的父類。
- 簡單工廠模式包含三個角色：工廠角色負責實現創建所有實例的內部邏輯；抽象產品角色是所創建的所有對象的父類，負責描述所有實例所共有的公共接口；具體產品角色是創建目標，所有創建的對象都充當這個角色的某個具體類的實例。
- 簡單工廠模式的要點在於：當你需要什麼，只需要傳入一個正確的參數，就可以獲取你所需要的對象，而無須知道其創建細節。
- 簡單工廠模式最大的優點在於實現對象的創建和對象的使用分離，將對象的創建交給專門的工廠類負責，但是其最大的缺點在於工廠類不夠靈活，增加新的具體產品需要修改工廠類的判斷邏輯代碼，而且產品較多時，工廠方法代碼將會非常複雜。
- 簡單工廠模式適用情況包括：工廠類負責創建的對象比較少；客戶端只知道傳入工廠類的參數，對於如何創建對象不關心。



