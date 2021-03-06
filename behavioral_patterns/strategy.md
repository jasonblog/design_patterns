#策略模式


#模式動機
- 完成一項任務，往往可以有多種不同的方式，每一種方式稱為一個策略，我們可以根據環境或者條件的不同選擇不同的策略來完成該項任務。
- 在軟件開發中也常常遇到類似的情況，實現某一個功能有多個途徑，此時可以使用一種設計模式來使得系統可以靈活地選擇解決途徑，也能夠方便地增加新的解決途徑。
- 在軟件系統中，有許多算法可以實現某一功能，如查找、排序等，一種常用的方法是硬編碼(Hard Coding)在一個類中，如需要提供多種查找算法，可以將這些算法寫到一個類中，在該類中提供多個方法，每一個方法對應一個具體的查找算法；當然也可以將這些查找算法封裝在一個統一的方法中，通過if…else…等條件判斷語句來進行選擇。這兩種實現方法我們都可以稱之為硬編碼，如果需要增加一種新的查找算法，需要修改封裝算法類的源代碼；更換查找算法，也需要修改客戶端調用代碼。在這個算法類中封裝了大量查找算法，該類代碼將較複雜，維護較為困難。
- 除了提供專門的查找算法類之外，還可以在客戶端程序中直接包含算法代碼，這種做法更不可取，將導致客戶端程序龐大而且難以維護，如果存在大量可供選擇的算法時問題將變得更加嚴重。
- 為瞭解決這些問題，可以定義一些獨立的類來封裝不同的算法，每一個類封裝一個具體的算法，在這裡，每一個封裝算法的類我們都可以稱之為策略(Strategy)，為了保證這些策略的一致性，一般會用一個抽象的策略類來做算法的定義，而具體每種算法則對應於一個具體策略類。


#模式定義
策略模式(Strategy Pattern)：定義一系列算法，將每一個算法封裝起來，並讓它們可以相互替換。策略模式讓算法獨立於使用它的客戶而變化，也稱為政策模式(Policy)。

策略模式是一種對象行為型模式。


#模式結構
策略模式包含如下角色：

- Context: 環境類
- Strategy: 抽象策略類
- ConcreteStrategy: 具體策略類


![](../_static/Strategy.jpg)


#時序圖
![](../_static/seq_Strategy.jpg)

#代碼分析

```cpp
#include <iostream>
#include "Context.h"
#include "ConcreteStrategyA.h"
#include "ConcreteStrategyB.h"
#include "Strategy.h"
#include <vector>
using namespace std;

int main(int argc, char* argv[])
{
    Strategy* s1 = new ConcreteStrategyA();
    Context* cxt = new Context();
    cxt->setStrategy(s1);
    cxt->algorithm();

    Strategy* s2 = new ConcreteStrategyB();
    cxt->setStrategy(s2);
    cxt->algorithm();

    delete s1;
    delete s2;

    int rac1 = 0x1;
    int rac2 = 0x2;
    int rac3 = 0x4;
    int rac4 = 0x8;

    int i = 0xe;
    int j = 0x5;

    int r1 = i & rac1;
    int r2 = i & rac2;
    int r3 = i & rac3;
    int r4 = i & rac4;

    cout << "res:" << r1 << "/" << r2 << "/" << r3 << "/" << r4 << endl;

    return 0;
}
```

```cpp
///////////////////////////////////////////////////////////
//  Context.h
//  Implementation of the Class Context
//  Created on:      09-十月-2014 22:21:07
//  Original author: colin
///////////////////////////////////////////////////////////

#if !defined(EA_0DA87730_4DEE_4392_9BAF_4AC64A8A07A4__INCLUDED_)
#define EA_0DA87730_4DEE_4392_9BAF_4AC64A8A07A4__INCLUDED_

#include "Strategy.h"

class Context
{

public:
    Context();
    virtual ~Context();


    void algorithm();
    void setStrategy(Strategy* st);

private:
    Strategy* m_pStrategy;

};
#endif // !defined(EA_0DA87730_4DEE_4392_9BAF_4AC64A8A07A4__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  Context.cpp
//  Implementation of the Class Context
//  Created on:      09-十月-2014 22:21:07
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Context.h"

Context::Context()
{
}

Context::~Context()
{
}

void Context::algorithm()
{
    m_pStrategy->algorithm();
}

void Context::setStrategy(Strategy* st)
{
    m_pStrategy = st;
}
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteStrategyA.h
//  Implementation of the Class ConcreteStrategyA
//  Created on:      09-十月-2014 22:21:06
//  Original author: colin
///////////////////////////////////////////////////////////

#if !defined(EA_9B180F12_677B_4e9b_A243_1F5DAD93FE1D__INCLUDED_)
#define EA_9B180F12_677B_4e9b_A243_1F5DAD93FE1D__INCLUDED_

#include "Strategy.h"

class ConcreteStrategyA : public Strategy
{

public:
    ConcreteStrategyA();
    virtual ~ConcreteStrategyA();

    virtual void algorithm();

};
#endif // !defined(EA_9B180F12_677B_4e9b_A243_1F5DAD93FE1D__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteStrategyA.cpp
//  Implementation of the Class ConcreteStrategyA
//  Created on:      09-十月-2014 22:21:07
//  Original author: colin
///////////////////////////////////////////////////////////

#include "ConcreteStrategyA.h"
#include <iostream>
using namespace std;

ConcreteStrategyA::ConcreteStrategyA()
{

}

ConcreteStrategyA::~ConcreteStrategyA()
{

}

void ConcreteStrategyA::algorithm()
{
    cout << "use algorithm A" << endl;
}
```

#運行結果：

![](../_static/Strategy_run.jpg)


#模式分析
- 策略模式是一個比較容易理解和使用的設計模式，策略模式是對算法的封裝，它把算法的責任和算法本身分割開，委派給不同的對象管理。策略模式通常把一個系列的算法封裝到一系列的策略類裡面，作為一個抽象策略類的子類。用一句話來說，就是“準備一組算法，並將每一個算法封裝起來，使得它們可以互換”。

- 在策略模式中，應當由客戶端自己決定在什麼情況下使用什麼具體策略角色。

- 策略模式僅僅封裝算法，提供新算法插入到已有系統中，以及老算法從系統中“退休”的方便，策略模式並不決定在何時使用何種算法，算法的選擇由客戶端來決定。這在一定程度上提高了系統的靈活性，但是客戶端需要理解所有具體策略類之間的區別，以便選擇合適的算法，這也是策略模式的缺點之一，在一定程度上增加了客戶端的使用難度。

#實例

#優點
策略模式的優點

- 策略模式提供了對“開閉原則”的完美支持，用戶可以在不修改原有系統的基礎上選擇算法或行為，也可以靈活地增加新的算法或行為。
- 策略模式提供了管理相關的算法族的辦法。
- 策略模式提供了可以替換繼承關係的辦法。
- 使用策略模式可以避免使用多重條件轉移語句。


#缺點
策略模式的缺點

- 客戶端必須知道所有的策略類，並自行決定使用哪一個策略類。
- 策略模式將造成產生很多策略類，可以通過使用享元模式在一定程度上減少對象的數量。


#適用環境
在以下情況下可以使用策略模式：

- 如果在一個系統裡面有許多類，它們之間的區別僅在於它們的行為，那麼使用策略模式可以動態地讓一個對象在許多行為中選擇一種行為。
- 一個系統需要動態地在幾種算法中選擇一種。
- 如果一個對象有很多的行為，如果不用恰當的模式，這些行為就只好使用多重的條件選擇語句來實現。
- 不希望客戶端知道複雜的、與算法相關的數據結構，在具體策略類中封裝算法和相關的數據結構，提高算法的保密性與安全性。


#模式應用

#模式擴展
策略模式與狀態模式

- 可以通過環境類狀態的個數來決定是使用策略模式還是狀態模式。
- 策略模式的環境類自己選擇一個具體策略類，具體策略類無須關心環境類；而狀態模式的環境類由於外在因素需要放進一個具體狀態中，以便通過其方法實現狀態的切換，因此環境類和狀態類之間存在一種雙向的關聯關係。
- 使用策略模式時，客戶端需要知道所選的具體策略是哪一個，而使用狀態模式時，客戶端無須關心具體狀態，環境類的狀態會根據用戶的操作自動轉換。
- 如果系統中某個類的對象存在多種狀態，不同狀態下行為有差異，而且這些狀態之間可以發生轉換時使用狀態模式；如果系統中某個類的某一行為存在多種實現方式，而且這些實現方式可以互換時使用策略模式。


#總結
- 在策略模式中定義了一系列算法，將每一個算法封裝起來，並讓它們可以相互替換。策略模式讓算法獨立於使用它的客戶而變化，也稱為政策模式。策略模式是一種對象行為型模式。
- 策略模式包含三個角色：環境類在解決某個問題時可以採用多種策略，在環境類中維護一個對抽象策略類的引用實例；抽象策略類為所支持的算法聲明瞭抽象方法，是所有策略類的父類；具體策略類實現了在抽象策略類中定義的算法。
- 策略模式是對算法的封裝，它把算法的責任和算法本身分割開，委派給不同的對象管理。策略模式通常把一個系列的算法封裝到一系列的策略類裡面，作為一個抽象策略類的子類。
- 策略模式主要優點在於對“開閉原則”的完美支持，在不修改原有系統的基礎上可以更換算法或者增加新的算法，它很好地管理算法族，提高了代碼的複用性，是一種替換繼承，避免多重條件轉移語句的實現方式；其缺點在於客戶端必須知道所有的策略類，並理解其區別，同時在一定程度上增加了系統中類的個數，可能會存在很多策略類。
- 策略模式適用情況包括：在一個系統裡面有許多類，它們之間的區別僅在於它們的行為，使用策略模式可以動態地讓一個對象在許多行為中選擇一種行為；一個系統需要動態地在幾種算法中選擇一種；避免使用難以維護的多重條件選擇語句；希望在具體策略類中封裝算法和與相關的數據結構。

