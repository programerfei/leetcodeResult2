# 使用LinkedHashMap实现LRU机制

```java
public class LRUCache{
    private LinkedHashMap<Integer,Integer> cache;
    private int cacheSize;
    
    public LRUCache(int cacheSize){
        cache=new LinkedHashMap<>();
        this.cacheSize=cacheSize;
    }
    
    public int get(int key){
        if(!cache.containsKey(key)) {
            return -1;
        }
        int res = cache.get(key);
        cache.remove(key);   //先从链表中删除
        cache.put(key,res);  //再把该节点放到链表末尾处
        return res;
    }
    
    public void put(int key,int value){
        if(cache.containsKey(key)) {
            cache.remove(key); //已经存在，在当前链表移除
        }
        if(capacity == cache.size()) {
            //cache已满，删除链表头位置
            Set<Integer> keySet = cache.keySet();
            Iterator<Integer> iterator = keySet.iterator();
            cache.remove(iterator.next());
        }
        cache.put(key,value);  //插入到链表末尾
    }
}
```

