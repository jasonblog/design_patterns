#狀態模式

#模式動機
- 在很多情況下，一個對象的行為取決於一個或多個動態變化的屬性，這樣的屬性叫做狀態，這樣的對象叫做有狀態的(stateful)對象，這樣的對象狀態是從事先定義好的一系列值中取出的。當一個這樣的對象與外部事件產生互動時，其內部狀態就會改變，從而使得系統的行為也隨之發生變化。

- 在UML中可以使用狀態圖來描述對象狀態的變化。


#模式定義
狀態模式(State Pattern) ：允許一個對象在其內部狀態改變時改變它的行為，對象看起來似乎修改了它的類。其別名為狀態對象(Objects for States)，狀態模式是一種對象行為型模式。


#模式結構
狀態模式包含如下角色：

- Context: 環境類
- State: 抽象狀態類
- ConcreteState: 具體狀態類


![](../_static/State.jpg)


#時序圖
![](../_static/seq_State.jpg)


既然是狀態模式，加上狀態圖，對狀態轉換間的理解會清晰很多：

![](../_static/State1.png)

#代碼分析
```cpp
#include <iostream>
#include "Context.h"
#include "ConcreteStateA.h"
#include "ConcreteStateB.h"

using namespace std;

int main(int argc, char* argv[])
{
    char a = '0';

    if ('0' == a) {
        cout << "yes" << endl;
    } else {
        cout << "no" << endl;
    }

    Context* c = new Context();
    c->request();
    c->request();
    c->request();

    delete c;
    return 0;
}
```


```cpp
///////////////////////////////////////////////////////////
//  Context.h
//  Implementation of the Class Context
//  Created on:      09-十月-2014 17:20:59
//  Original author: colin
///////////////////////////////////////////////////////////

#if !defined(EA_F245CF81_2A68_4461_B039_2B901BD5A126__INCLUDED_)
#define EA_F245CF81_2A68_4461_B039_2B901BD5A126__INCLUDED_

#include "State.h"

class Context
{

public:
    Context();
    virtual ~Context();

    void changeState(State* st);
    void request();

private:
    State* m_pState;
};
#endif // !defined(EA_F245CF81_2A68_4461_B039_2B901BD5A126__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  Context.cpp
//  Implementation of the Class Context
//  Created on:      09-十月-2014 17:20:59
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Context.h"
#include "ConcreteStateA.h"

Context::Context()
{
    //default is a
    m_pState = ConcreteStateA::Instance();
}

Context::~Context()
{
}

void Context::changeState(State* st)
{
    m_pState = st;
}

void Context::request()
{
    m_pState->handle(this);
}
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteStateA.h
//  Implementation of the Class ConcreteStateA
//  Created on:      09-十月-2014 17:20:58
//  Original author: colin
///////////////////////////////////////////////////////////

#if !defined(EA_84158F08_E96A_4bdb_89A1_4BE2E633C3EE__INCLUDED_)
#define EA_84158F08_E96A_4bdb_89A1_4BE2E633C3EE__INCLUDED_

#include "State.h"

class ConcreteStateA : public State
{

public:
    virtual ~ConcreteStateA();

    static State* Instance();

    virtual void handle(Context* c);

private:
    ConcreteStateA();
    static State* m_pState;
};
#endif // !defined(EA_84158F08_E96A_4bdb_89A1_4BE2E633C3EE__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteStateA.cpp
//  Implementation of the Class ConcreteStateA
//  Created on:      09-十月-2014 17:20:58
//  Original author: colin
///////////////////////////////////////////////////////////

#include "ConcreteStateA.h"
#include "ConcreteStateB.h"
#include "Context.h"
#include <iostream>
using namespace std;

State* ConcreteStateA::m_pState = NULL;
ConcreteStateA::ConcreteStateA()
{
}

ConcreteStateA::~ConcreteStateA()
{
}

State* ConcreteStateA::Instance()
{
    if (NULL == m_pState) {
        m_pState = new ConcreteStateA();
    }

    return m_pState;
}

void ConcreteStateA::handle(Context* c)
{
    cout << "doing something in State A.\n done,change state to B" << endl;
    c->changeState(ConcreteStateB::Instance());
}
```

#運行結果：

![](../_static/State_run.jpg)


#模式分析
- 狀態模式描述了對象狀態的變化以及對象如何在每一種狀態下表現出不同的行為。
- 狀態模式的關鍵是引入了一個抽象類來專門表示對象的狀態，這個類我們叫做抽象狀態類，而對象的每一種具體狀態類都繼承了該類，並在不同具體狀態類中實現了不同狀態的行為，包括各種狀態之間的轉換。

在狀態模式結構中需要理解環境類與抽象狀態類的作用：

- 環境類實際上就是擁有狀態的對象，環境類有時候可以充當狀態管理器(State Manager)的角色，可以在環境類中對狀態進行切換操作。
- 抽象狀態類可以是抽象類，也可以是接口，不同狀態類就是繼承這個父類的不同子類，狀態類的產生是由於環境類存在多個狀態，同時還滿足兩個條件： 這些狀態經常需要切換，在不同的狀態下對象的行為不同。因此可以將不同對象下的行為單獨提取出來封裝在具體的狀態類中，使得環境類對象在其內部狀態改變時可以改變它的行為，對象看起來似乎修改了它的類，而實際上是由於切換到不同的具體狀態類實現的。由於環境類可以設置為任一具體狀態類，因此它針對抽象狀態類進行編程，在程序運行時可以將任一具體狀態類的對象設置到環境類中，從而使得環境類可以改變內部狀態，並且改變行為。

#實例
TCPConnection

這個示例來自《設計模式》,展示了一個簡化版的TCP協議實現;
TCP連接的狀態有多種可能，狀態之間的轉換有相應的邏輯前提；
這是使用狀態模式的場合；


#狀態圖：

![](../_static/State2.png)

#結構圖：

![](../_static/State_eg.jpg)


#時序圖：

![](../_static/seq_State_eg.jpg)


#優點
狀態模式的優點

- 封裝了轉換規則。
- 枚舉可能的狀態，在枚舉狀態之前需要確定狀態種類。
- 將所有與某個狀態有關的行為放到一個類中，並且可以方便地增加新的狀態，只需要改變對象狀態即可改變對象的行為。
- 允許狀態轉換邏輯與狀態對象合成一體，而不是某一個巨大的條件語句塊。
- 可以讓多個環境對象共享一個狀態對象，從而減少系統中對象的個數。



#缺點
狀態模式的缺點

- 狀態模式的使用必然會增加系統類和對象的個數。
- 狀態模式的結構與實現都較為複雜，如果使用不當將導致程序結構和代碼的混亂。
- 狀態模式對“開閉原則”的支持並不太好，對於可以切換狀態的狀態模式，增加新的狀態類需要修改那些負責狀態轉換的源代碼，否則無法切換到新增狀態；而且修改某個狀態類的行為也需修改對應類的源代碼。


#適用環境
在以下情況下可以使用狀態模式：

- 對象的行為依賴於它的狀態（屬性）並且可以根據它的狀態改變而改變它的相關行為。
- 代碼中包含大量與對象狀態有關的條件語句，這些條件語句的出現，會導致代碼的可維護性和靈活性變差，不能方便地增加和刪除狀態，使客戶類與類庫之間的耦合增強。在這些條件語句中包含了對象的行為，而且這些條件對應於對象的各種狀態。


#模式應用
狀態模式在工作流或遊戲等類型的軟件中得以廣泛使用，甚至可以用於這些系統的核心功能設計，如在政府OA辦公系統中，一個批文的狀態有多種：尚未辦理；正在辦理；正在批示；正在審核；已經完成等各種狀態，而且批文狀態不同時對批文的操作也有所差異。使用狀態模式可以描述工作流對象（如批文）的狀態轉換以及不同狀態下它所具有的行為。

#模式擴展
共享狀態

- 在有些情況下多個環境對象需要共享同一個狀態，如果希望在系統中實現多個環境對象實例共享一個或多個狀態對象，那麼需要將這些狀態對象定義為環境的靜態成員對象。


簡單狀態模式與可切換狀態的狀態模式
    1) 簡單狀態模式：簡單狀態模式是指狀態都相互獨立，狀態之間無須進行轉換的狀態模式，這是最簡單的一種狀態模式。對於這種狀態模式，每個狀態類都封裝與狀態相關的操作，而無須關心狀態的切換，可以在客戶端直接實例化狀態類，然後將狀態對象設置到環境類中。如果是這種簡單的狀態模式，它遵循“開閉原則”，在客戶端可以針對抽象狀態類進行編程，而將具體狀態類寫到配置文件中，同時增加新的狀態類對原有系統也不造成任何影響。
    2) 可切換狀態的狀態模式：大多數的狀態模式都是可以切換狀態的狀態模式，在實現狀態切換時，在具體狀態類內部需要調用環境類Context的setState()方法進行狀態的轉換操作，在具體狀態類中可以調用到環境類的方法，因此狀態類與環境類之間通常還存在關聯關係或者依賴關係。通過在狀態類中引用環境類的對象來回調環境類的setState()方法實現狀態的切換。在這種可以切換狀態的狀態模式中，增加新的狀態類可能需要修改其他某些狀態類甚至環境類的源代碼，否則系統無法切換到新增狀態。

#總結
- 狀態模式允許一個對象在其內部狀態改變時改變它的行為，對象看起來似乎修改了它的類。其別名為狀態對象，狀態模式是一種對象行為型模式。
- 狀態模式包含三個角色：環境類又稱為上下文類，它是擁有狀態的對象，在環境類中維護一個抽象狀態類State的實例，這個實例定義當前狀態，在具體實現時，它是一個State子類的對象，可以定義初始狀態；抽象狀態類用於定義一個接口以封裝與環境類的一個特定狀態相關的行為；具體狀態類是抽象狀態類的子類，每一個子類實現一個與環境類的一個狀態相關的行為，每一個具體狀態類對應環境的一個具體狀態，不同的具體狀態類其行為有所不同。
- 狀態模式描述了對象狀態的變化以及對象如何在每一種狀態下表現出不同的行為。
- 狀態模式的主要優點在於封裝了轉換規則，並枚舉可能的狀態，它將所有與某個狀態有關的行為放到一個類中，並且可以方便地增加新的狀態，只需要改變對象狀態即可改變對象的行為，還可以讓多個環境對象共享一個狀態對象，從而減少系統中對象的個數；其缺點在於使用狀態模式會增加系統類和對象的個數，且狀態模式的結構與實現都較為複雜，如果使用不當將導致程序結構和代碼的混亂，對於可以切換狀態的狀態模式不滿足“開閉原則”的要求。
- 狀態模式適用情況包括：對象的行為依賴於它的狀態（屬性）並且可以根據它的狀態改變而改變它的相關行為；代碼中包含大量與對象狀態有關的條件語句，這些條件語句的出現，會導致代碼的可維護性和靈活性變差，不能方便地增加和刪除狀態，使客戶類與類庫之間的耦合增強。

