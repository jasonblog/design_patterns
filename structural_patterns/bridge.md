#橋接模式

#模式動機
設想如果要繪製矩形、圓形、橢圓、正方形，我們至少需要4個形狀類，但是如果繪製的圖形需要具有不同的顏色，如紅色、綠色、藍色等，此時至少有如下兩種設計方案：

- 第一種設計方案是為每一種形狀都提供一套各種顏色的版本。
- 第二種設計方案是根據實際需要對形狀和顏色進行組合

對於有兩個變化維度（即兩個變化的原因）的系統，採用方案二來進行設計系統中類的個數更少，且系統擴展更為方便。設計方案二即是橋接模式的應用。橋接模式將繼承關係轉換為關聯關係，從而降低了類與類之間的耦合，減少了代碼編寫量。


#模式定義
橋接模式(Bridge Pattern)：將抽象部分與它的實現部分分離，使它們都可以獨立地變化。它是一種對象結構型模式，又稱為柄體(Handle and Body)模式或接口(Interface)模式。


#模式結構
橋接模式包含如下角色：

- Abstraction：抽象類
- RefinedAbstraction：擴充抽象類
- Implementor：實現類接口
- ConcreteImplementor：具體實現類


![](../_static/Bridge.jpg)


#時序圖
![](../_static/seq_Bridge.jpg)

#代碼分析
```cpp
// main.cpp
#include <iostream>
#include "ConcreteImplementorA.h"
#include "ConcreteImplementorB.h"
#include "RefinedAbstraction.h"
#include "Abstraction.h"

using namespace std;

int main(int argc, char* argv[])
{

    Implementor* pImp = new ConcreteImplementorA();
    Abstraction* pa = new RefinedAbstraction(pImp);
    pa->operation();

    Abstraction* pb = new RefinedAbstraction(new ConcreteImplementorB());
    pb->operation();

    delete pa;
    delete pb;

    return 0;
}
```

```cpp
///////////////////////////////////////////////////////////
//  RefinedAbstraction.h
//  Implementation of the Class RefinedAbstraction
//  Created on:      03-十月-2014 18:12:43
//  Original author: colin
///////////////////////////////////////////////////////////

#if !defined(EA_4BA5BE7C_DED5_4236_8362_F2988921CFA7__INCLUDED_)
#define EA_4BA5BE7C_DED5_4236_8362_F2988921CFA7__INCLUDED_

#include "Abstraction.h"

class RefinedAbstraction : public Abstraction
{

public:
    RefinedAbstraction();
    RefinedAbstraction(Implementor* imp);
    virtual ~RefinedAbstraction();

    virtual void operation();

};
#endif // !defined(EA_4BA5BE7C_DED5_4236_8362_F2988921CFA7__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  RefinedAbstraction.cpp
//  Implementation of the Class RefinedAbstraction
//  Created on:      03-十月-2014 18:12:43
//  Original author: colin
///////////////////////////////////////////////////////////

#include "RefinedAbstraction.h"
#include <iostream>
using namespace std;


RefinedAbstraction::RefinedAbstraction()
{

}

RefinedAbstraction::RefinedAbstraction(Implementor* imp)
    : Abstraction(imp)
{
}

RefinedAbstraction::~RefinedAbstraction()
{

}

void RefinedAbstraction::operation()
{
    cout << "do something else ,and then " << endl;
    m_pImp->operationImp();
}
```

#運行結果：

![](../_static/Bridge_run.jpg)


#模式分析
理解橋接模式，重點需要理解如何將抽象化(Abstraction)與實現化(Implementation)脫耦，使得二者可以獨立地變化。

- 抽象化：抽象化就是忽略一些信息，把不同的實體當作同樣的實體對待。在面向對象中，將對象的共同性質抽取出來形成類的過程即為抽象化的過程。
- 實現化：針對抽象化給出的具體實現，就是實現化，抽象化與實現化是一對互逆的概念，實現化產生的對象比抽象化更具體，是對抽象化事物的進一步具體化的產物。
- 脫耦：脫耦就是將抽象化和實現化之間的耦合解脫開，或者說是將它們之間的強關聯改換成弱關聯，將兩個角色之間的繼承關係改為關聯關係。橋接模式中的所謂脫耦，就是指在一個軟件系統的抽象化和實現化之間使用關聯關係（組合或者聚合關係）而不是繼承關係，從而使兩者可以相對獨立地變化，這就是橋接模式的用意。


#實例
如果需要開發一個跨平臺視頻播放器，可以在不同操作系統平臺（如Windows、Linux、Unix等）上播放多種格式的視頻文件，常見的視頻格式包括MPEG、RMVB、AVI、WMV等。現使用橋接模式設計該播放器。


#優點
橋接模式的優點:

- 分離抽象接口及其實現部分。
- 橋接模式有時類似於多繼承方案，但是多繼承方案違背了類的單一職責原則（即一個類只有一個變化的原因），複用性比較差，而且多繼承結構中類的個數非常龐大，橋接模式是比多繼承方案更好的解決方法。
- 橋接模式提高了系統的可擴充性，在兩個變化維度中任意擴展一個維度，都不需要修改原有系統。
- 實現細節對客戶透明，可以對用戶隱藏實現細節。


#缺點
橋接模式的缺點:

- 橋接模式的引入會增加系統的理解與設計難度，由於聚合關聯關係建立在抽象層，要求開發者針對抽象進
行設計與編程。
- 橋接模式要求正確識別出系統中兩個獨立變化的維度，因此其使用範圍具有一定的侷限性。


#適用環境
在以下情況下可以使用橋接模式：

- 如果一個系統需要在構件的抽象化角色和具體化角色之間增加更多的靈活性，避免在兩個層次之間建立靜態的繼承聯繫，通過橋接模式可以使它們在抽象層建立一個關聯關係。
- 抽象化角色和實現化角色可以以繼承的方式獨立擴展而互不影響，在程序運行時可以動態將一個抽象化子類的對象和一個實現化子類的對象進行組合，即系統需要對抽象化角色和實現化角色進行動態耦合。
- 一個類存在兩個獨立變化的維度，且這兩個維度都需要進行擴展。
- 雖然在系統中使用繼承是沒有問題的，但是由於抽象化角色和具體化角色需要獨立變化，設計要求需要獨立管理這兩者。
- 對於那些不希望使用繼承或因為多層次繼承導致系統類的個數急劇增加的系統，橋接模式尤為適用。


模式應用
--------------------
一個Java桌面軟件總是帶有所在操作系統的視感(LookAndFeel)，如果一個Java軟件是在Unix系統上開發的，那麼開發人員看到的是Motif用戶界面的視感；在Windows上面使用這個系統的用戶看到的是Windows用戶界面的視感；而一個在Macintosh上面使用的用戶看到的則是Macintosh用戶界面的視感，Java語言是通過所謂的Peer架構做到這一點的。Java為AWT中的每一個GUI構件都提供了一個Peer構件，在AWT中的Peer架構就使用了橋接模式


模式擴展
--------------------
適配器模式與橋接模式的聯用:

- 橋接模式和適配器模式用於設計的不同階段，橋接模式用於系統的初步設計，對於存在兩個獨立變化維度的類可以將其分為抽象化和實現化兩個角色，使它們可以分別進行變化；而在初步設計完成之後，當發現系統與已有類無法協同工作時，可以採用適配器模式。但有時候在設計初期也需要考慮適配器模式，特別是那些涉及到大量第三方應用接口的情況。


總結
--------------------
- 橋接模式將抽象部分與它的實現部分分離，使它們都可以獨立地變化。它是一種對象結構型模式，又稱為柄體(Handle and Body)模式或接口(Interface)模式。
- 橋接模式包含如下四個角色：抽象類中定義了一個實現類接口類型的對象並可以維護該對象；擴充抽象類擴充由抽象類定義的接口，它實現了在抽象類中定義的抽象業務方法，在擴充抽象類中可以調用在實現類接口中定義的業務方法；實現類接口定義了實現類的接口，實現類接口僅提供基本操作，而抽象類定義的接口可能會做更多更復雜的操作；具體實現類實現了實現類接口並且具體實現它，在不同的具體實現類中提供基本操作的不同實現，在程序運行時，具體實現類對象將替換其父類對象，提供給客戶端具體的業務操作方法。
- 在橋接模式中，抽象化(Abstraction)與實現化(Implementation)脫耦，它們可以沿著各自的維度獨立變化。
- 橋接模式的主要優點是分離抽象接口及其實現部分，是比多繼承方案更好的解決方法，橋接模式還提高了系統的可擴充性，在兩個變化維度中任意擴展一個維度，都不需要修改原有系統，實現細節對客戶透明，可以對用戶隱藏實現細節；其主要缺點是增加系統的理解與設計難度，且識別出系統中兩個獨立變化的維度並不是一件容易的事情。
- 橋接模式適用情況包括：需要在構件的抽象化角色和具體化角色之間增加更多的靈活性，避免在兩個層次之間建立靜態的繼承聯繫；抽象化角色和實現化角色可以以繼承的方式獨立擴展而互不影響；一個類存在兩個獨立變化的維度，且這兩個維度都需要進行擴展；設計要求需要獨立管理抽象化角色和具體化角色；不希望使用繼承或因為多層次繼承導致系統類的個數急劇增加的系統。

