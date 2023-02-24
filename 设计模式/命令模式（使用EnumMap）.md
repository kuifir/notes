命令模式通常由一个只包含一个方法的接口开始，然后为该方法创建多个不同行为的实现。只需要配置好这些命令对象，程序就会根据需要来调用它们。

``` java

interface Command { void action(); }

public class EnumMaps {
  public static void main(String[] args) {
    EnumMap<AlarmPoints,Command> em =
      new EnumMap<>(AlarmPoints.class);
    em.put(KITCHEN,
      () -> System.out.println("Kitchen fire!"));
    em.put(BATHROOM,
      () -> System.out.println("Bathroom alert!"));
    for(Map.Entry<AlarmPoints,Command> e:
        em.entrySet()) {
      System.out.print(e.getKey() + ": ");
      e.getValue().action();
    }
    try { // If there's no value for a particular key:
      em.get(UTILITY).action();
    } catch(Exception e) {
      System.out.println("Expected: " + e);
    }
  }
}
/* Output:
BATHROOM: Bathroom alert!
KITCHEN: Kitchen fire!
Expected: java.lang.NullPointerException
*/
```
