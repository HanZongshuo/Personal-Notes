# 数据结构
## 1. 数组
```
int[] arr = new int[5];
int[] arr2 = {1, 2, 3, 4, 5};
```

动态数组
```
ArrayList<Integer> list = new ArrayList<>();
list.add(10);
```

# 2. 集成与多态
#### 所有员工的通用属性与行为（父类提取：`Employee`）

- **每周工作时长**：固定 40 小时（`hours = 40`）。
    
- **年假天数**：默认 10 天（`vacationDays = 10`）。
    
- **年终奖金**：根据年薪的一定比例发放（`getBonus(double percentage)`）。
    
- **休假申请表格颜色**：不同职位颜色不同（需根据职位重写 `getVacationFormColor()`）。
    

#### 2. 各职位的特有属性与行为（子类扩展与特化）

- **基础员工（Basic Employee）**：
    
    - 无特殊技能，继承基类的默认规则。
        
- **秘书（Secretary）**：
    
    - 特有技能：能够记录听写（`takeDictation(String text)`）。
        
- **法务秘书（Legal Secretary）**：
    
    - **继承自秘书**（具备普通秘书的一切能力）。
        
    - 特有技能：撰写法律摘要（`fileLegalBriefs()`）。
        
    - 薪资特化：比普通秘书**多拿 10% 的薪水**（重写薪资计算）。
        
- **市场营销专员（Marketer）**：
    
    - 特有技能：投放广告/营销宣传（`advertise()`）。
        
- **律师（Lawyer）**：
    
    - 特有技能：提起诉讼（`sue()`）。
        
    - 年假特化：**多 5 天年假**，总共 15 天（重写年假天数）。

```
public abstract class AbstractEmployee {
    private String name; //父类这个变量是私有的，子类需要通过super来赋值

    public AbstractEmployee(String name) { this.name = name; } //这里是父类给出的构造方法

    // 1. 共有通用行为（所有子类完全复用，无需重写）
    public int getHours() { return 40; }
    public int getVacationDays() { return 10; }

    // 2. 依赖抽象方法的高层通用逻辑（模板方法机制）
    public double bonus(double rate) { return getSalary() * rate; }
    public String toString() { return name + " (" + position() + ")"; }

    // 3. 必须由子类特化实现的纯抽象契约，构造时三个必须全部满足否则报错
    public abstract String position();
    public abstract double getSalary();
    public abstract String getVacationForm();
}
```

通用员工类
```
public class Employee extends AbstractEmployee {
	public Employee(String name) { 
	super(name);
	}
	
	@Override
	public String position() {
		return "employee"; 
	}
	@Override
	public double getSalary() { 
		return 38000; 
	}
	@Override
	public String getVacationForm() { 
	return "blue"; 
	}
}
```

律师类代码
```
public class Lawer extends AbstractEmployee {
    public Lawer(String name) {
        super(name);
    }

    @Override
    public double getSalary() {
        return 61000.0;
    }

    @Override
    public String position() {
        return "lawer";
    }

    @Override
    public String getVacationForm() {
        return "orange";
    }

    @Override
    public int getVacationDays() {
        return 15;
    }

    public void sue() { //其余的子类不具备这个能力 
        System.out.println("I see you in court!");
    }
}
```

## enum
适用于固定，有限，不可变的常量实例集合（如状态，星期，性别），防止被集成，底层被编译器自动修饰为final，构造方法强制为private。

```
enum Color { Black, Blue, Green, Red }
enum Shape { Rectangle, Circle, Square }
enum StudentType { BS, MS, PhD }

Color color = Color.Black;  //声明一个类型为Color的变量color，把Black赋值给它
Shape shape = Shape.Circle;
show(shape, color);

private static void show(Shape shape, Color color) { ... }
```

```
public enum HttpStatus {
    OK(200, "Success"),
    NOT_FOUND(404, "Page Not Found"),
    SERVER_ERROR(500, "Internal Server Error");

    private final int code;
    private final String description;

    HttpStatus(int code, String description) {
        this.code = code;
        this.description = description;
    }

    public int getCode() { return code; }
    public String getDescription() { return description; }
}

// 调用极其清晰：
System.out.println(HttpStatus.NOT_FOUND.getCode());        // 404
System.out.println(HttpStatus.NOT_FOUND.getDescription()); // Page Not Found
```

## Record的用法
用来简洁的定义数据载体，自动生成构造方法，访问器，equals，hashCode(),toString等。

```
public record Student(String name, int age, double score) {
}
```


## Interface
接口不关心代码具体做什么，只定义可以做什么。 
```
public interface Shape {
    public double area();       // 计算面积
    public double perimeter();  // 计算周长
}
```
只要一个类声明自己实现了Shape接口，就必须承诺实现area() 和 perimeter()这两个方法。

5 种树的类型
Binary tree： 每个节点最多有两个子节点
N-ary tree：每个节点最多N个子节点
Full Tree(Perfect Binary Tree): 所有层都填满
Perfect Tree：每个节点有两个或者0个子节点
Complete Tree：除了最后一层，其余的都填满。最后一层的节点必须从左到右。
