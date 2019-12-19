## 对于Function<? super V , ? extends T>的理解

/***

当使用father 和 children两个类为例，

Father instanceof Children //false

Children instanceof Father //true

List<Father> list1 = new ArrayList<>();

List<Children> list2 = new ArrayList<>();

list1.addAll(list2);//true

addAll(Collection<? extends E> c);//原因

list1中的children是可以强转出来的，之前所有的Children的值还存在！！！！！！

***/这里的都不是Function相关的