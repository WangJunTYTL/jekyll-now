## Java四种引用类型

在java中，类型就分为两种，基本类型和引用类型或自定义类型。

引用类型又分为四种：

1. 强引用 StrongReference
2. 软引用 SoftReference
3. 若引用 WeakReference
4. 虚引用 PhantomReference

划分这些类型的目的是：是为了更灵活的管理对象的生命周期，让垃圾器在最合适的适合回收对象。虽然，我们在实际业务中很少用到这些知识，但是很有必要了解到这些，去帮助我们去做些程序性能优化。

#### 使用方式


##### 强引用

强引用就是直接引用对象比如下面这样，我们在编写程序时，用到的大多都是强引用

    StringBuffer b1 = new StringBuffer("hello world");

强引用对象，当垃圾回收进行时，不会被回收，及时通过```b1 = null;```释放引用，在资源充足时，也不会被垃圾回收立刻回收。如果内存吃紧，Java虚拟机会抛出OutOfMemoryError错误，使程序异常终止，不会靠随意回收具有强引用的对象来解决内存不足的问题。    

##### 软引用

    SoftReference<StringBuffer> softReference = new SoftReference<StringBuffer>(new StringBuffer("hello world"));
    System.gc();
    System.out.print(softReference.get()); // 只有当内存吃紧时，发生gc后，会报Exception in thread "main" java.lang.NullPointerException，
   
软引用的生命周期会比强引用弱点，在内存空间足够时，垃圾回收器就不会回收它；如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。
它也经常和ReferenceQueue配合使用，如果gc在回收掉引用对象后，会把保存引用对象的softReference送入队列，这时可以通过下面这行代码判断是否被回收。

    // 创建softReference并提供给注册队列
    ReferenceQueue<StringBuffer> referenceQueue = new ReferenceQueue<StringBuffer>();
    SoftReference<StringBuffer> softReference = new SoftReference<StringBuffer>(new StringBuffer("hello world"),referenceQueue);
    
    // 判断入队列来判断是否被回收，或直接判断softReference.get() == null
    softReference.get() == null || softReference.enqueue()   

另外也可以手动清除这些保存引用对象的reference对象

    Reference ref;
    while ((ref = referenceQueue.poll()) != null) {
        // poll出即清除，也不必手动清除，等待gc清除
    }


使用案列：common-pool2的SoftReferenceObjectPool，用于实现一种可以随着需要而增长的池对象管理，当gc时可以清除空闲的实例。
    
下面是common-pool2的部分代码，具体可以参照其里面的`SoftReferenceObjectPool`类
    
    @Override
    public synchronized void addObject() throws Exception {
        assertOpen();
        if (factory == null) {
            throw new IllegalStateException(
                    "Cannot add objects without a factory.");
        }
        T obj = factory.makeObject().getObject();
        createCount++;
        // Create and register with the queue
        PooledSoftReference<T> ref = new PooledSoftReference<T>(
                new SoftReference<T>(obj, refQueue));
        allReferences.add(ref);
        boolean success = true;
        if (!factory.validateObject(ref)) {
            success = false;
        } else {
            factory.passivateObject(ref);
        }
        boolean shouldDestroy = !success;
        if (success) {
            idleReferences.add(ref);
            notifyAll(); // numActive has changed
        }
        if (shouldDestroy) {
            try {
                destroy(ref);
            } catch (Exception e) {
                // ignored
            }
        }
        
        @Override
        public synchronized void returnObject(T obj) throws Exception {
            boolean success = !isClosed();
            final PooledSoftReference<T> ref = findReference(obj);
            if (ref == null) {
                throw new IllegalStateException(
                    "Returned object not currently part of this pool");
            }
            if (factory != null) {
                if (!factory.validateObject(ref)) {
                    success = false;
                } else {
                    try {
                        factory.passivateObject(ref);
                    } catch (Exception e) {
                        success = false;
                    }
                }
            }
            boolean shouldDestroy = !success;
            numActive--;
            if (success) {
                // Deallocate and add to the idle instance pool
                ref.deallocate();
                idleReferences.add(ref);
            }
            notifyAll(); // numActive has changed
            if (shouldDestroy && factory != null) {
                try {
                    destroy(ref);
                } catch (Exception e) {
                    // ignored
                }
            }
        }
    
##### 弱引用

    WeakReference<StringBuffer> weakReference = new WeakReference<StringBuffer>(new StringBuffer("hello world"));
    WeakReference<StringBuffer> weakReference2 = new WeakReference<StringBuffer>(new StringBuffer("hello world"));
    System.out.print(weakReference.get()); //  hello world
    StringBuffer buffer = weakReference2.get();
    System.gc();
    System.out.print(weakReference.get()); // Exception in thread "main" java.lang.NullPointerException
    System.out.print(weakReference2.get()); //  hello world
    
弱引用会在发生gc时，没有对象在去引用时会被立刻被回收，不管内存是否充裕。
    
使用案列：WeakHashMap的实现
    
##### 虚引用

    ReferenceQueue<StringBuffer> stringBufferReferenceQueue = new ReferenceQueue<StringBuffer>();
    PhantomReference<StringBuffer> phantomReference = new PhantomReference<StringBuffer>(new StringBuffer("hello world"), stringBufferReferenceQueue);
    System.out.print(phantomReference.get());Exception in thread "main" java.lang.NullPointerException

虚引用，不顾是否被垃圾回收，都不可以拿到真正的引用对象。一般用法是    
程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。
    
    
具体Demo代码请参照：ReferenceDemo    
    
    
    
    

   










