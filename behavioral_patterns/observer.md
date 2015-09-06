#觀察者模式

#模式動機
建立一種對象與對象之間的依賴關係，一個對象發生改變時將自動通知其他對象，其他對象將相應做出反應。在此，發生改變的對象稱為觀察目標，而被通知的對象稱為觀察者，一個觀察目標可以對應多個觀察者，而且這些觀察者之間沒有相互聯繫，可以根據需要增加和刪除觀察者，使得系統更易於擴展，這就是觀察者模式的模式動機。


#模式定義
觀察者模式(Observer Pattern)：定義對象間的一種一對多依賴關係，使得每當一個對象狀態發生改變時，其相關依賴對象皆得到通知並被自動更新。觀察者模式又叫做發佈-訂閱（Publish/Subscribe）模式、模型-視圖（Model/View）模式、源-監聽器（Source/Listener）模式或從屬者（Dependents）模式。

觀察者模式是一種對象行為型模式。


#模式結構
觀察者模式包含如下角色：

- Subject: 目標
- ConcreteSubject: 具體目標
- Observer: 觀察者
- ConcreteObserver: 具體觀察者


![](../_static/Obeserver.jpg)


#時序圖
![](../_static/seq_Obeserver.jpg)

#代碼分析

```cpp
#include <iostream>
#include "Subject.h"
#include "Obeserver.h"
#include "ConcreteObeserver.h"
#include "ConcreteSubject.h"

using namespace std;

int main(int argc, char* argv[])
{
    Subject* subject = new ConcreteSubject();
    Obeserver* objA = new ConcreteObeserver("A");
    Obeserver* objB = new ConcreteObeserver("B");
    subject->attach(objA);
    subject->attach(objB);

    subject->setState(1);
    subject->notify();

    cout << "--------------------" << endl;
    subject->detach(objB);
    subject->setState(2);
    subject->notify();

    delete subject;
    delete objA;
    delete objB;

    return 0;
}
```

```cpp
///////////////////////////////////////////////////////////
//  Subject.h
//  Implementation of the Class Subject
//  Created on:      07-十月-2014 23:00:10
//  Original author: cl
///////////////////////////////////////////////////////////

#if !defined(EA_61998456_1B61_49f4_B3EA_9D28EEBC9649__INCLUDED_)
#define EA_61998456_1B61_49f4_B3EA_9D28EEBC9649__INCLUDED_

#include "Obeserver.h"
#include <vector>
using namespace std;

class Subject
{

public:
    Subject();
    virtual ~Subject();
    Obeserver* m_Obeserver;

    void attach(Obeserver* pObeserver);
    void detach(Obeserver* pObeserver);
    void notify();

    virtual int getState() = 0;
    virtual void setState(int i) = 0;

private:
    vector<Obeserver*> m_vtObj;

};
#endif // !defined(EA_61998456_1B61_49f4_B3EA_9D28EEBC9649__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  Subject.cpp
//  Implementation of the Class Subject
//  Created on:      07-十月-2014 23:00:10
//  Original author: cl
///////////////////////////////////////////////////////////

#include "Subject.h"

Subject::Subject()
{

}

Subject::~Subject()
{

}

void Subject::attach(Obeserver* pObeserver)
{
    m_vtObj.push_back(pObeserver);
}

void Subject::detach(Obeserver* pObeserver)
{
    for (vector<Obeserver*>::iterator itr = m_vtObj.begin();
         itr != m_vtObj.end(); itr++) {
        if (*itr == pObeserver) {
            m_vtObj.erase(itr);
            return;
        }
    }
}

void Subject::notify()
{
    for (vector<Obeserver*>::iterator itr = m_vtObj.begin();
         itr != m_vtObj.end();
         itr++) {
        (*itr)->update(this);
    }
}
```

```cpp
///////////////////////////////////////////////////////////
//  Obeserver.h
//  Implementation of the Class Obeserver
//  Created on:      07-十月-2014 23:00:10
//  Original author: cl
///////////////////////////////////////////////////////////

#if !defined(EA_2C7362B2_0B22_4168_8690_F9C7B76C343F__INCLUDED_)
#define EA_2C7362B2_0B22_4168_8690_F9C7B76C343F__INCLUDED_

class Subject;

class Obeserver
{

public:
    Obeserver();
    virtual ~Obeserver();
    virtual void update(Subject* sub) = 0;
};
#endif // !defined(EA_2C7362B2_0B22_4168_8690_F9C7B76C343F__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteObeserver.h
//  Implementation of the Class ConcreteObeserver
//  Created on:      07-十月-2014 23:00:09
//  Original author: cl
///////////////////////////////////////////////////////////

#if !defined(EA_7B020534_BFEA_4c9e_8E4C_34DCE001E9B1__INCLUDED_)
#define EA_7B020534_BFEA_4c9e_8E4C_34DCE001E9B1__INCLUDED_
#include "Obeserver.h"
#include <string>
using namespace std;

class ConcreteObeserver : public Obeserver
{

public:
    ConcreteObeserver(string name);
    virtual ~ConcreteObeserver();
    virtual void update(Subject* sub);

private:
    string m_objName;
    int m_obeserverState;
};
#endif // !defined(EA_7B020534_BFEA_4c9e_8E4C_34DCE001E9B1__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteObeserver.cpp
//  Implementation of the Class ConcreteObeserver
//  Created on:      07-十月-2014 23:00:09
//  Original author: cl
///////////////////////////////////////////////////////////

#include "ConcreteObeserver.h"
#include <iostream>
#include <vector>
#include "Subject.h"
using namespace std;

ConcreteObeserver::ConcreteObeserver(string name)
{
    m_objName = name;
}

ConcreteObeserver::~ConcreteObeserver()
{

}

void ConcreteObeserver::update(Subject* sub)
{
    m_obeserverState = sub->getState();
    cout << "update oberserver[" << m_objName << "] state:" << m_obeserverState <<
         endl;
}
```

#運行結果：

![](../_static/Obeserver_run.jpg)


#模式分析
- 觀察者模式描述瞭如何建立對象與對象之間的依賴關係，如何構造滿足這種需求的系統。
- 這一模式中的關鍵對象是觀察目標和觀察者，一個目標可以有任意數目的與之相依賴的觀察者，一旦目標的狀態發生改變，所有的觀察者都將得到通知。
- 作為對這個通知的響應，每個觀察者都將即時更新自己的狀態，以與目標狀態同步，這種交互也稱為發佈-訂閱(publishsubscribe)。目標是通知的發佈者，它發出通知時並不需要知道誰是它的觀察者，可以有任意數目的觀察者訂閱它並接收通
知。


#實例

#優點
觀察者模式的優點

- 觀察者模式可以實現表示層和數據邏輯層的分離，並定義了穩定的消息更新傳遞機制，抽象了更新接口，使得可以有各種各樣不同的表示層作為具體觀察者角色。
- 觀察者模式在觀察目標和觀察者之間建立一個抽象的耦合。
- 觀察者模式支持廣播通信。
- 觀察者模式符合“開閉原則”的要求。

#缺點
觀察者模式的缺點

- 如果一個觀察目標對象有很多直接和間接的觀察者的話，將所有的觀察者都通知到會花費很多時間。
- 如果在觀察者和觀察目標之間有循環依賴的話，觀察目標會觸發它們之間進行循環調用，可能導致系統崩潰。
- 觀察者模式沒有相應的機制讓觀察者知道所觀察的目標對象是怎麼發生變化的，而僅僅只是知道觀察目標發生了變化。


#適用環境
在以下情況下可以使用觀察者模式：

- 一個抽象模型有兩個方面，其中一個方面依賴於另一個方面。將這些方面封裝在獨立的對象中使它們可以各自獨立地改變和複用。
- 一個對象的改變將導致其他一個或多個對象也發生改變，而不知道具體有多少對象將發生改變，可以降低對象之間的耦合度。
- 一個對象必須通知其他對象，而並不知道這些對象是誰。
- 需要在系統中創建一個觸發鏈，A對象的行為將影響B對象，B對象的行為將影響C對象……，可以使用觀察者模式創建一種鏈式觸發機制。


#模式應用
觀察者模式在軟件開發中應用非常廣泛，如某電子商務網站可以在執行發送操作後給用戶多個發送商品打折信息，某團隊戰鬥遊戲中某隊友犧牲將給所有成員提示等等，凡是涉及到一對一或者一對多的對象交互場景都可以使用觀察者模式。

#模式擴展
MVC模式

- MVC模式是一種架構模式，它包含三個角色：模型(Model)，視圖(View)和控制器(Controller)。觀察者模式可以用來實現MVC模式，觀察者模式中的觀察目標就是MVC模式中的模型(Model)，而觀察者就是MVC中的視圖(View)，控制器(Controller)充當兩者之間的中介者(Mediator)。當模型層的數據發生改變時，視圖層將自動改變其顯示內容。

#總結
- 觀察者模式定義對象間的一種一對多依賴關係，使得每當一個對象狀態發生改變時，其相關依賴對象皆得到通知並被自動更新。觀察者模式又叫做發佈-訂閱模式、模型-視圖模式、源-監聽器模式或從屬者模式。觀察者模式是一種對象行為型模式。
- 觀察者模式包含四個角色：目標又稱為主題，它是指被觀察的對象；具體目標是目標類的子類，通常它包含有經常發生改變的數據，當它的狀態發生改變時，向它的各個觀察者發出通知；觀察者將對觀察目標的改變做出反應；在具體觀察者中維護一個指向具體目標對象的引用，它存儲具體觀察者的有關狀態，這些狀態需要和具體目標的狀態保持一致。
- 觀察者模式定義了一種一對多的依賴關係，讓多個觀察者對象同時監聽某一個目標對象，當這個目標對象的狀態發生變化時，會通知所有觀察者對象，使它們能夠自動更新。
- 觀察者模式的主要優點在於可以實現表示層和數據邏輯層的分離，並在觀察目標和觀察者之間建立一個抽象的耦合，支持廣播通信；其主要缺點在於如果一個觀察目標對象有很多直接和間接的觀察者的話，將所有的觀察者都通知到會花費很多時間，而且如果在觀察者和觀察目標之間有循環依賴的話，觀察目標會觸發它們之間進行循環調用，可能導致系統崩潰。
- 觀察者模式適用情況包括：一個抽象模型有兩個方面，其中一個方面依賴於另一個方面；一個對象的改變將導致其他一個或多個對象也發生改變，而不知道具體有多少對象將發生改變；一個對象必須通知其他對象，而並不知道這些對象是誰；需要在系統中創建一個觸發鏈。
- 在JDK的java.util包中，提供了Observable類以及Observer接口，它們構成了Java語言對觀察者模式的支持。
