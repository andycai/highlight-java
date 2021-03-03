# Java - LinkedHashMap

HashMap 和双向链表合二为一即是 LinkedHashMap。所谓 LinkedHashMap，其落脚点在 HashMap，因此更准确地说，它是一个将所有Entry节点链入一个双向链表的 HashMap。

由于 LinkedHashMap 是 HashMap 的子类，所以 LinkedHashMap 自然会拥有 HashMap 的所有特性。比如，LinkedHashMap 的元素存取过程基本与 HashMap 基本类似，只是在细节实现上稍有不同。当然，这是由 LinkedHashMap 本身的特性所决定的，因为它额外维护了一个双向链表用于保持迭代顺序。

此外，LinkedHashMap 可以很好的支持 LRU 算法，代码如下：

```java
import java.util.*;

// 扩展一下LinkedHashMap这个类，让他实现LRU算法
class LRULinkedHashMap<K,V> extends LinkedHashMap<K,V> {
	// 定义缓存的容量
	private int capacity;
	private static final long serialVersionUID = 1L;
	
    // 带参数的构造器	
	LRULinkedHashMap(int capacity) {
		// 调用 LinkedHashMap 的构造器，传入以下参数
		super(16,0.75f,true);
		// 传入指定的缓存最大容量
		this.capacity = capacity;
	}
    
	// 实现 LRU 的关键方法，如果 map 里面的元素个数大于了缓存最大容量，则删除链表的顶端元素
	@Override
	public boolean removeEldestEntry(Map.Entry<K, V> eldest) { 
		System.out.println(eldest.getKey() + "=" + eldest.getValue());  
		return size() > capacity;
	}  
}

// 测试类
class Test {
    public static void main(String[] args) throws Exception {
        // 指定缓存最大容量为4
        Map<Integer,Integer> map=new LRULinkedHashMap<>(4);
        map.put(9,3);
        map.put(7,4);
        map.put(5,9);
        map.put(3,4);
        map.put(6,6);
        //总共 put 了 5 个元素，超过了指定的缓存最大容量
        //遍历结果
        for(Iterator<Map.Entry<Integer,Integer>> it=map.entrySet().iterator(); it.hasNext(); ){
            System.out.println(it.next().getKey());
        }
	}
}
```

