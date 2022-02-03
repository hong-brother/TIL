## Set 클래스 중 가장 빠른 것은? 🏃‍



```java
package com.hsnam;

import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;
import java.util.TreeSet;
import java.util.concurrent.TimeUnit;

import org.openjdk.jmh.annotations.*;

@State(Scope.Thread)
@BenchmarkMode({ Mode.AverageTime })
@OutputTimeUnit(TimeUnit.MICROSECONDS)
public class FastestClassInSet {
    int LOOP_COUNT = 1000;
    Set<String> set;
    String data = "gillogfastestsetimplementaions";

    @Benchmark
    public void addHashSet() {
        set = new HashSet<String>();
        for(int loop = 0; loop < LOOP_COUNT; loop++) {
            set.add(data+loop);
        }
    }

    @Benchmark
    public void addTreeSet() {
        set = new TreeSet<String>();
        for(int loop = 0; loop < LOOP_COUNT; loop++) {
            set.add(data+loop);
        }
    }

    @Benchmark
    public void addLinkedHashSet() {
        set = new LinkedHashSet<String>();
        for(int loop = 0; loop < LOOP_COUNT; loop++) {
            set.add(data+loop);
        }
    }
}
```

- 테스트 결과

|Benchmar                           |Mode|Cnt|  Score|Error|Units|
|-----------------------------------|----|---|-------|-----|-----|
|FastestClassInSet.addHashSet       |avgt|   | 69.193|     |us/op|
|FastestClassInSet.addLinkedHashSet |avgt|   | 70.697|     |us/op|
|FastestClassInSet.addTreeSet       |avgt|   |201.826|     |us/op|

- 결론
HashSet과 LinkedHashSet의 성능이 비슷하고 TreeSet의 순서로 성은이 차이가 발생한다.
