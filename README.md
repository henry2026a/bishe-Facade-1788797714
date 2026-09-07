# 设计模式之-门面 Facade

> **发布时间**: 2026-09-03
> **标签**: 无

---

## 📞 联系方式
> 💬 **微信：sen232sen** | 🚀 **专业定制各类管理系统** | ⭐ **源码获取/技术支持**

---

门面类的设计核心的思想：通过一个统一的接口来隐藏子系统的复杂性。

场景：现在公司需要进一批货：电脑、桌子、椅子、微波炉、冰箱  
在传统的场景中，我们需要跑到不同的市场中购买这些商品，假设现在有一个百货商场，都能买到这些东西，那我们只需要找百货商场的老板一次性进这些货就可以了，我们并不需要去关心老板如何去买这些东西。这就是典型的门面设计思想  
下面看一下代码实现：  
为了满足依赖倒置原则，首先我们定义商场的抽象类

    public abstract class Shop {
        abstract void buy();
    }

然后，我们定义具体的商场类，继承商场的抽象类

    public class AShop extends Shop{
        @Override
        void buy() {
            System.out.println("在A商场里买了冰箱和微波炉");
        }
    }

    public class BShop extends Shop{
        @Override
        void buy() {
            System.out.println("在B商场里买了桌子和椅子");
        }
    }

    public class CShop extends Shop{
        @Override
        void buy() {
            System.out.println("在C商场里买了电脑");
        }
    }

可以看到，我们定义了3个具体的商场，可以买到我们具体的商品。  
接下来需要提供一个门面类，分别调用3个具体的商场中的方法即可。  
这时候，门面类需要创建这三个类的对象，我们可以使用工厂方法的方式来创建对象，但这里我们只关心门面设计模式，就不做演示了

    public class Facade {
    
        private Facade(){}
        private Shop aShop = new AShop();
    
        private Shop bShop = new BShop();
    
        private Shop cShop = new CShop();
    
        public void buyPlanA(){
            aShop.buy();
            bShop.buy();
            cShop.buy();
        }
    
        public static Facade getFacade(){
            return FacadeInner.FACADE;
        }
    
        private static class FacadeInner{
            private static final Facade FACADE =new Facade();
        }
    }

我们此时只需要在外部调用门面类中的方法

        public static void main(String[] args) {
            Facade facade = Facade.getFacade();
            facade.buyPlanA();
        }

假如将来需要只买电脑、桌子、椅子，那我们在门面类提供一个新的方法进行组装就行了

        public void buyPlanB(){
            bShop.buy();
            cShop.buy();
        }

可以看到整个代码体系里面，各个商场只用负责自己单一的职责，而门面类可以根据需要去自行组装，最后我们在外部不用关心门面类是如何组装方法内部的，我们只用调用方法就行

---

![sensen](images/sensen.png)
