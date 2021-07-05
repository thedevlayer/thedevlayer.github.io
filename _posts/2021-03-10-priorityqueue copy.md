---
layout: post
title: 우선순위큐 (Priority Queue)
category: 01_Algorithm
tag: [Priority Queue]
---

우선순위는 일반적으로 먼저 들어간 데이터가 먼저 나오는 큐와는 다른, 별도로 정의된 규칙에 따른 우선순위대로 나오는 큐를 말한다.
클래스 선언 시 Comparable 을 이용하여 구현 가능하며, Comparator 의 클래스로도 사용할 수 있다. 본 장에서는 Comparable 를 활용한 예제를 나타내었다.
동일 조건일 때는 return 이전에 if 문으로 분기하여 추가 구현 가능하다.



- coding

```java
import java.util.PriorityQueue;

class Student implements Comparable<Student>{
	String name;
	int age;
	
	public Student(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public int compareTo(Student o) {
		// age 가 같을 경우 name 첫번째 문자열 기준으로 내림차순
		if(this.age == o.age) return o.toString().charAt(0) - this.name.toString().charAt(0);  
		return this.age - o.age; // age 로 오름차순
	}
}


public class Solution{
	public static void main(String[] args) {
		
		PriorityQueue<Student> pq = new PriorityQueue<Student>();
		
		pq.add(new Student("Park", 23));
		pq.add(new Student("Kim", 12));
		pq.add(new Student("Ace", 66));
		pq.add(new Student("Lee", 45));
		pq.add(new Student("Hong", 44));
		pq.add(new Student("Park", 66));
		pq.add(new Student("Choi", 66));
		
		while(!pq.isEmpty()) {
			Student st = pq.poll();
			System.out.println(st.age + "\t"+ st.name);
		}
 
	}
}
```

- 출력값

```
12	Kim
23	Park
44	Hong
45	Lee
66	Ace
66	Park
66	Choi
``` 