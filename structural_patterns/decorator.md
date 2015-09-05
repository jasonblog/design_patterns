#裝飾模式

#模式動機
一般有兩種方式可以實現給一個類或對象增加行為：

- 繼承機制，使用繼承機制是給現有類添加功能的一種有效途徑，通過繼承一個現有類可以使得子類在擁有自身方法的同時還擁有父類的方法。但是這種方法是靜態的，用戶不能控制增加行為的方式和時機。
- 關聯機制，即將一個類的對象嵌入另一個對象中，由另一個對象來決定是否調用嵌入對象的行為以便擴展自己的行為，我們稱這個嵌入的對象為裝飾器(Decorator)

裝飾模式以對客戶透明的方式動態地給一個對象附加上更多的責任，換言之，客戶端並不會覺得對象在裝飾前和裝飾後有什麼不同。裝飾模式可以在不需要創造更多子類的情況下，將對象的功能加以擴展。這就是裝飾模式的模式動機。


#模式定義
裝飾模式(Decorator Pattern) ：動態地給一個對象增加一些額外的職責(Responsibility)，就增加對象功能來說，裝飾模式比生成子類實現更為靈活。其別名也可以稱為包裝器(Wrapper)，與適配器模式的別名相同，但它們適用於不同的場合。根據翻譯的不同，裝飾模式也有人稱之為“油漆工模式”，它是一種對象結構型模式。


#模式結構
裝飾模式包含如下角色：

- Component: 抽象構件
- ConcreteComponent: 具體構件
- Decorator: 抽象裝飾類
- ConcreteDecorator: 具體裝飾類


![](../_static/Decorator.jpg)


#時序圖
![](../_static/seq_Decorator.jpg)

#代碼分析
```cpp
// main.cpp
#include <iostream>
#include "ConcreteComponent.h"
#include "ConcreteDecoratorA.h"
#include "ConcreteDecoratorB.h"
#include "Component.h"
using namespace std;

int main(int argc, char* argv[])
{
    ConcreteComponent* pRealProd = new ConcreteComponent();
    //动态增加行为
    Component* pA = new ConcreteDecoratorA(pRealProd);
    pA->operation();
    //继续动态增加行为
    Component* pB = new ConcreteDecoratorB(pA);
    pB->operation();

    delete pRealProd;
    delete pA;
    delete pB;
    return 0;
}
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteComponent.cpp
//  Implementation of the Class ConcreteComponent
//  Created on:      03-十月-2014 18:53:00
//  Original author: colin
///////////////////////////////////////////////////////////

#include "ConcreteComponent.h"
#include <iostream>
using namespace std;


ConcreteComponent::ConcreteComponent()
{

}

ConcreteComponent::~ConcreteComponent()
{

}

void ConcreteComponent::operation()
{
    cout << "ConcreteComponent's normal operation!" << endl;
}
```
```cpp
///////////////////////////////////////////////////////////
//  ConcreteDecoratorA.h
//  Implementation of the Class ConcreteDecoratorA
//  Created on:      03-十月-2014 18:53:00
//  Original author: colin
///////////////////////////////////////////////////////////

#if !defined(EA_6786B68E_DCE4_44c4_B26D_812F0B3C0382__INCLUDED_)
#define EA_6786B68E_DCE4_44c4_B26D_812F0B3C0382__INCLUDED_

#include "Decorator.h"
#include "Component.h"

class ConcreteDecoratorA : public Decorator
{

public:
    ConcreteDecoratorA(Component* pcmp);
    virtual ~ConcreteDecoratorA();

    void addBehavior();
    virtual void operation();

};
#endif // !defined(EA_6786B68E_DCE4_44c4_B26D_812F0B3C0382__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteComponent.cpp
//  Implementation of the Class ConcreteComponent
//  Created on:      03-十月-2014 18:53:00
//  Original author: colin
///////////////////////////////////////////////////////////

#include "ConcreteComponent.h"
#include <iostream>
using namespace std;


ConcreteComponent::ConcreteComponent()
{

}

ConcreteComponent::~ConcreteComponent()
{

}

void ConcreteComponent::operation()
{
    cout << "ConcreteComponent's normal operation!" << endl;
}
```

#運行結果：

![](../_static/Decorator_run.jpg)

#模式分析
- 與繼承關係相比，關聯關係的主要優勢在於不會破壞類的封裝性，而且繼承是一種耦合度較大的靜態關係，無法在程序運行時動態擴展。在軟件開發階段，關聯關係雖然不會比繼承關係減少編碼量，但是到了軟件維護階段，由於關聯關係使系統具有較好的鬆耦合性，因此使得系統更加容易維護。當然，關聯關係的缺點是比繼承關係要創建更多的對象。
- 使用裝飾模式來實現擴展比繼承更加靈活，它以對客戶透明的方式動態地給一個對象附加更多的責任。裝飾模式可以在不需要創造更多子類的情況下，將對象的功能加以擴展。


#實例
實例：變形金剛

變形金剛在變形之前是一輛汽車，它可以在陸地上移動。當它變成機器人之後除了能夠在陸地上移動之外，還可以說話；如果需要，它還可以變成飛機，除了在陸地上移動還可以在天空中飛翔。

![](../_static/Decorator_eg.jpg)

![](../_static/seq_Decorator_eg.jpg)



#優點
裝飾模式的優點:

- 裝飾模式與繼承關係的目的都是要擴展對象的功能，但是裝飾模式可以提供比繼承更多的靈活性。
- 可以通過一種動態的方式來擴展一個對象的功能，通過配置文件可以在運行時選擇不同的裝飾器，從而實現不同的行為。
- 通過使用不同的具體裝飾類以及這些裝飾類的排列組合，可以創造出很多不同行為的組合。可以使用多個具體裝飾類來裝飾同一對象，得到功能更為強大的對象。
- 具體構件類與具體裝飾類可以獨立變化，用戶可以根據需要增加新的具體構件類和具體裝飾類，在使用時再對其進行組合，原有代碼無須改變，符合“開閉原則”


#缺點
裝飾模式的缺點:

- 使用裝飾模式進行系統設計時將產生很多小對象，這些對象的區別在於它們之間相互連接的方式有所不同，而不是它們的類或者屬性值有所不同，同時還將產生很多具體裝飾類。這些裝飾類和小對象的產生將增加系統的複雜度，加大學習與理解的難度。
- 這種比繼承更加靈活機動的特性，也同時意味著裝飾模式比繼承更加易於出錯，排錯也很困難，對於多次裝飾的對象，調試時尋找錯誤可能需要逐級排查，較為煩瑣。


#適用環境
在以下情況下可以使用裝飾模式：

- 在不影響其他對象的情況下，以動態、透明的方式給單個對象添加職責。
- 需要動態地給一個對象增加功能，這些功能也可以動態地被撤銷。
- 當不能採用繼承的方式對系統進行擴充或者採用繼承不利於系統擴展和維護時。不能採用繼承的情況主要有兩類：第一類是系統中存在大量獨立的擴展，為支持每一種組合將產生大量的子類，使得子類數目呈爆炸性增長；第二類是因為類定義不能繼承（如final類）.


#模式應用

#模式擴展
裝飾模式的簡化-需要注意的問題:

- 一個裝飾類的接口必須與被裝飾類的接口保持相同，對於客戶端來說無論是裝飾之前的對象還是裝飾之後的對象都可以一致對待。
- 儘量保持具體構件類Component作為一個“輕”類，也就是說不要把太多的邏輯和狀態放在具體構件類中，可以通過裝飾類
對其進行擴展。
- 如果只有一個具體構件類而沒有抽象構件類，那麼抽象裝飾類可以作為具體構件類的直接子類。

#總結
- 裝飾模式用於動態地給一個對象增加一些額外的職責，就增加對象功 能來說，裝飾模式比生成子類實現更為靈活。它是一種對象結構型模 式。
- 裝飾模式包含四個角色：抽象構件定義了對象的接口，可以給這些對 象動態增加職責（方法）；具體構件定義了具體的構件對象，實現了 在抽象構件中聲明的方法，裝飾器可以給它增加額外的職責（方法）； 抽象裝飾類是抽象構件類的子類，用於給具體構件增加職責，但是具 體職責在其子類中實現；具體裝飾類是抽象裝飾類的子類，負責向構 件添加新的職責。
- 使用裝飾模式來實現擴展比繼承更加靈活，它以對客戶透明的方式動 態地給一個對象附加更多的責任。裝飾模式可以在不需要創造更多子 類的情況下，將對象的功能加以擴展。
- 裝飾模式的主要優點在於可以提供比繼承更多的靈活性，可以通過一種動態的 方式來擴展一個對象的功能，並通過使用不同的具體裝飾類以及這些裝飾類的 排列組合，可以創造出很多不同行為的組合，而且具體構件類與具體裝飾類可 以獨立變化，用戶可以根據需要增加新的具體構件類和具體裝飾類；其主要缺 點在於使用裝飾模式進行系統設計時將產生很多小對象，而且裝飾模式比繼承 更加易於出錯，排錯也很困難，對於多次裝飾的對象，調試時尋找錯誤可能需 要逐級排查，較為煩瑣。
- 裝飾模式適用情況包括：在不影響其他對象的情況下，以動態、透明的方式給 單個對象添加職責；需要動態地給一個對象增加功能，這些功能也可以動態地 被撤銷；當不能採用繼承的方式對系統進行擴充或者採用繼承不利於系統擴展 和維護時。
- 裝飾模式可分為透明裝飾模式和半透明裝飾模式：在透明裝飾模式中，要求客 戶端完全針對抽象編程，裝飾模式的透明性要求客戶端程序不應該聲明具體構 件類型和具體裝飾類型，而應該全部聲明為抽象構件類型；半透明裝飾模式允 許用戶在客戶端聲明具體裝飾者類型的對象，調用在具體裝飾者中新增的方法。


