---
title: 泛型的使用注意事项与字节码解读
date: 2019-04-11 15:34:00
tags: [工程,泛型]
categories: 通用技能
---
* 泛型的使用与对应的字节码
* 泛型的弊端
<!-- more -->
![image](2017-09-15-md/night.jpg)

## 泛型的使用与对应的字节码

    Generics  允许开发抽象算法和数据结构，并提供实体类型以供后续操作 ， 即“参数化类型”，编译阶段就直接检查泛型类型的兼容性,泛型信息不会进入到运行期；类型擦除； （接口、类、和方法）

    定义一个泛型的接口：

        public interface GInterface <T,R>{
            T perform(R r); //提供了个开放的接口定义，允许从使用端，调用端来指定方法的入参和返回参数；
        }

        在字节码中表现：

        annotation system Ldalvik/annotation/Signature;
            value = {
                "<T:",
                "Ljava/lang/Object;",
                "R:",
                "Ljava/lang/Object;",
                ">",
                "Ljava/lang/Object;"
            }
        .end annotation

        T，R最终都会再编译器被类型擦除为同一个object类； 所以说：编译阶段的一个使用方式，不会进入运行期，在此期间会被类型擦除掉；


    定义一个类的泛型：
        public class GInterfaceImp3<T,R> implements GInterface  <T,R>{
        // 泛型实现接口的时候，只指定其中的泛型类；

        //    @Override
        //    public <T,R>T perform(R r) {
        //        R r1  = r;
        //        String name = r1.getClass().getName();
        //
        //        return null;
        //    }

            @Override
            public T perform(R r) {
                List<String> stringArrayList = new ArrayList<String>();
                List<Integer> integerArrayList = new ArrayList<Integer>();

                return null;
            }
        }

        字节码中的表现：
            # virtual methods
            .method public perform(Ljava/lang/Object;)Ljava/lang/Object;
                .registers 5
                .annotation system Ldalvik/annotation/Signature;
                    value = {
                        "(TR;)TT;"
                    }
                .end annotation

            从接口中实现过来的方法，也被类型擦除掉了； 从这个方向来说，泛型其实可以使用object父类来进行替换的，然后每次都是向上转型么？


    定义一个方法的泛型：
        //    方法中定义自己的泛型；
            public<U,R> R action(U u){
                R result = null;
                return result;
            }

        字节码中的表现：

        # virtual methods
        .method public action(Ljava/lang/Object;)Ljava/lang/Object;
            .registers 3
            .annotation system Ldalvik/annotation/Signature;
                value = {
                    "<U:",
                    "Ljava/lang/Object;",
                    "R:",
                    "Ljava/lang/Object;",
                    ">(TU;)TR;"
                }
            .end annotation
```

           /**
         * ?  通配符的使用 只能用来填充通配符泛型变量，表示通配任何类型；
         * 注意：利用<? extends Number>定义的变量，只可取其中的值，不可修改
         */
        GInterface<?,?> gInterfaceimp1 = new GInterfaceImp3<String , String>();
//        gInterfaceimp1.perform("");
//        gInterfaceimp1.perform();
        Log.e("" + TAG , "");
//
        /**
         * 如果你想从一个数据类型里获取数据，使用 ? extends 通配符（能取不能存）
         * 通配符的使用
         * 注意：利用<? extends Number>定义的变量，只可取其中的值，不可修改
         */
        List<? extends  Integer> extendsGeneric;
        extendsGeneric = new LinkedList<Integer>();
//        extendsGeneric.add(1); //报错
        /**
         * 如果你想把对象写入一个数据结构里，使用 ? super 通配符（能存不能取）
         */
        List<? super String> superGeneric = new LinkedList<String>();
        superGeneric.add("");
        String s = (String) superGeneric.get(0);
        /**
         * 如果你既想存，又想取，那就别用通配符。
         */
```


## 泛型的弊端：
    1.类型擦除 -- 造成在重载方法的时候，因为此爆出：Erasure of method sort(Collection<String>) is the same as another method  方法重载失败；
         同时，不能以任何有意义的方式来使用该类型；
    2.不能使用基本数据类型，只能使用对应的包装类；
    3.通配符使用限制；












