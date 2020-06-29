---
title: HashMap常用操作
date: 2020-06-29 19:56:28
categories: Java
tags: HashMap
---

### HashMap带默认值操作

> public V getOrDefault(Object key, V defaultValue)

返回指定键所映射到的值；如果此映射不包含键的映射关系，则返回defaultValue。

```java

HashMap<String, Integer> map = new HashMap<>();
map.put("a", 200);

// key = a 存在，返回值 k = 200
Integer k = map.getOrDefault("a", 500);

// key = b 不存在，返回默认值 h = 500
Integer h = map.getOrDefault("b", 500);

```

> public V putIfAbsent(K key, V value)

如果指定的键尚未与值关联（或映射为null），则将其与给定值关联并返回null，否则返回当前值。

```java

HashMap<String, Integer> map = new HashMap<>();
map.put("a", 200);
map.put("e", null);

// key = b 不存在，则相当于执行 put("b", 500)，并返回 k = null
Integer k = map.putIfAbsent("b", 500);

// key = b 存在，但 value = null，则相当于执行 put("e", 500)，并返回 h = null
Integer h = map.putIfAbsent("e", 500);

// key = a 存在，且 value != null, 则 原值不变，返回 f = 200
Integer f = map.putIfAbsent("a", 500);

```

### HashMap四种遍历方式

```java
//第一种: 通过Map.keySet遍历key和value
for (String key: map.keySet()) {
	System.out.println("key= " + key + " and value= " + map.get(key));
}

//第二种: 通过Map.entrySet使用iterator遍历key和value
Iterator < Map.Entry < String, String >> it = map.entrySet().iterator();
while (it.hasNext()) {
	Map.Entry < String,
	String > entry = it.next();
	System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
}

//第三种: 推荐，尤其是容量大时,通过Map.entrySet遍历key和value
for (Map.Entry < String, String > entry: map.entrySet()) {
	System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
}

//第四种: 通过Map.values()遍历所有的value，但不能遍历key
for (String v: map.values()) {
	System.out.println("value= " + v);
}
```