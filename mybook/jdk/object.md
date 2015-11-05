## Object

在Java中Object对象是一切对象都会**自动继承**的一个类，在这个类中定义的属性和方法可以说是每个类都必须的。

这里有必要说说这里对象里面的几个方法

### hashCode()

返回该对象的哈希码值。

#### 为什么需要这个方法

我在面试时，问一些基本的Java知识时，很多时候会问到这个问题。但大多数人，没有很明白的说明这个问题。

首先这个问题，还是先要说下hash表的数据结构，如果还想复习下关于hash表结构的知识，可以参看这篇文章，写的不错：[http://www.cnblogs.com/dolphin0520/archive/2012/09/28/2700000.html](http://www.cnblogs.com/dolphin0520/archive/2012/09/28/2700000.html)

这个方法的存在主要是配合一些基于hash表的数据结构的集合，像HashMap。在这些基于hash结构的数据集合中，存放的对象要有自己的hashCode方法，除了String类型。以HashMap为例:

    /**
     * Retrieve object hash code and applies a supplemental hash function to the
     * result hash, which defends against poor quality hash functions.  This is
     * critical because HashMap uses power-of-two length hash tables, that
     * otherwise encounter collisions for hashCodes that do not differ
     * in lower bits. Note: Null keys always map to hash 0, thus index 0.
     */
    final int hash(Object k) {
        int h = hashSeed;
        if (0 != h && k instanceof String) {
            return sun.misc.Hashing.stringHash32((String) k);
        }
        h ^= k.hashCode();
        // This function ensures that hashCodes that differ only by
        // constant multiples at each bit position have a bounded
        // number of collisions (approximately 8 at default load factor).
        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
    }

HashMap在存放或者检索数据时，都会先去计算的key的hash值。这些基于hash表的集合，只能要求被存放的对象实现自己的hash方法，保证hash的均匀性。在jdk中，为了保证一个通用的计算hash的方法，jvm采用将对象的内部地址转换成一个整数来实现
我们也可以根据自己的逻辑修改hashcode方法


### equals(Object o)

用于测试某个对象是否同另一个对象相等

在Java语言中要比较两个对象是否相等，有时只用"=="是不行的，还有这个equals方法。比如在Java中比较两个字符串相等，那就必须使用equals方法
因为，`==`计算表达式在判断引用对象时，只是去判断内存地址引用是否一样，而equals方法才会去判断内存的值是否一样或是按照自实行逻辑去判断。


#### 必要时重写equals

equals方法在很多地方会调用，包括我们直接调用equals方法，还有判断集合对象是否相等时的间接调用。在这种间接调用时，我们一般都会去重写它的equals方法。
比如，有个User对象，我们认为User的id相等就是同一个对象
    
    Public class User{
        public long id;
        public String name;
    }
           
现在有个两个存放User对象的集合userListA、usersListB,这时我们判断这俩集合是否相等，其目的就是判断这俩集合存放的对象是否相等(只简单要求List各个索引位置的User对象相等)。
那这是我们就必须重写User对象的equals方法
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return id == user.id;
    }

重写equals方法的要求：

1. 自反性：对于任何非空引用x，x.equals(x)应该返回true。
2. 对称性：对于任何引用x和y，如果x.equals(y)返回true，那么y.equals(x)也应该返回true。
3. 传递性：对于任何引用x、y和z，如果x.equals(y)返回true，y.equals(z)返回true，那么x.equals(z)也应该返回true。
4. 一致性：如果x和y引用的对象没有发生变化，那么反复调用x.equals(y)应该返回同样的结果。
5. 非空性：对于任意非空引用x，x.equals(null)应该返回false。

#### 重写了equals方法要不要重写hashCode方法

当我们重写hashcode方法时，都会有一套模板，我们使用到的编辑器一般都会支持基于模板自动生成。如果留心，你会发现当你使用这个功能时重写equals方法会自动也把hashcode方法重写。

在Java规范中，要求：

如果a.equals(b),那么a和b的hashcode方法一定要相等，但a和b不相等时，hashcode方法可以相等也可以不相等。所以当我们重写了它的equals方法后，最好遵从这份规范，修改它的hashcode方法。


#### equals 方法未来会不会舍弃

如果`==`这种计算，不仅仅是看内存地址是否相等，如果在不相等时也去判断下不同内存地址下的值也是相等，就返回true，那有一天Object对象会不会就会舍弃这个equals方法

