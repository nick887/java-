# java细节
### 1.数组的拷贝
```java
int[] a={1,2,3,4};
int[] b;
Arrays.copyOf(a,a.length);
//a.length可以扩展，多余空间赋值为0
//相当与分配了一个新的数组空间放置b
```
ps：java中的格式化输出数组要用到```Arrays.toString()```
### 2.java命令行的使用
```java
class Solution{
public static void main(String[] args)
{
//若运行是使用的是 java Solution -g message abc
//则args[0]="-g";args[1]="message";args[2]="abc"
}
}
```
### 3.类的结构
```java
class name{
	private int a;//不可以被直接访问如name.a，但在类的方法中可以直接访问
	public String b;//可以被访问
	private static int c;//此域属于类name而不属于类的实例
	public static final int d;//此域与上面不类似，多用到
	//实例域
	public name(int a,String b)
	{
		this.a=a;
		this.b=b;
	}
	//该方法为构造器，用于初始化对象实例，this.a为隐式实例，a，b，为显示实例，puclic代表可以被访问
	public static int add(int a,int b)
	{
		return a+b;
	}
	//此为静态方法，静态方法不能访问类的实例域，可以访问类的静态实例域，静态方法只能被类引用不能被实例引用
	public static person createPerson()
	{
		return new person();
	}
}
```
### 4.方法参数的传递
java中方法参数的传递都是对传递对象拷贝后的引用。
当传入基本数据类型如int型时，接受的显示参数对该值进行一个拷贝，函数内改变此值不影响原值。
当传入一个对象时，拷贝对象的引用，只有通过对象访问实例域是才能改变对象的状态，直接改变对象的引用对原对象无用。
### 5.类的构造
#### 5.1 类的重载
类中所有方法都可以重载，重载指方法名相同，输入显示参数不同，当输入不同参数时会编译器自动选择方法
#### 5.2类的初始化
1. 如果类一开始未给定一个构造器函数，编译器会自动给类的实例域赋值，一般整数为0，字符串为null。
2. 类的初始化还可以在定义实例域时进行，如```private int a=100; ```  
3. 当构造器的显示参数与隐式参数相同时为了避免冲突，隐式参数一般使用this.a来引用
4. 调用另一个构造器，在构造器内写入this（参数1，参数2）来调用
#### 5.3初始化块
在类的实例域下写入如下代码
```java
class person
{
	private int age;
	private String name;
	{
		age=100;
		name="100";
	}
}
```
每次构造该类的具体对象时都会运行该代码块来初始化
综上初始化的过程为：
1. 所有实例域初始化为0，null
2. 调用实例域的显示初始化和初始化块
3. 调用构造起初始化
### 6.类的继承
#### 6.1继承范围
子类继承了超类的实例域以及方法，子类可以直接使用超类的方法，但不能访问超类的私有域，若要访问需要使用超类的方法来调用，如果方法重名了，需要使用```super.fucntion()```来调用超类方法
#### 6.2super的作用
1. 调用超类方法
2. 调用超类构造器，因为无法访问超类私有域，所以需要使用```super(int age,String name)```这样的形式来创建子类的构造器
#### 6.3多态
超类的引用可以引用子类，但超类的引用引用子类时不能使用子类特有的方法与域，此时相当于把子类看作超类
#### 6.4覆盖
在子类中重写域超类方法名相同的方法可以覆盖超累方法，返回值可以不同
#### 6.5final类与方法
形如```public final class person```这样的类的定义保证了该类不会被其他类所继承
形如```public final int add(int a,int b)```这样的方法可以保证该类无法被子类覆盖
#### 6.6抽象类
抽象类起占位作用定义时需要写做```public abstract class person```,抽象类里包含了抽象方法，如```public abstract void getName();```不需要内容。
子类可以继承抽象类，如果无法实现全部抽象方法，子类也必须是抽象类。
抽象类不能构造实例。
可以定义抽象类的对象引用，但只能引用非抽象的子类
#### 6.7protected修饰符
当修饰实例域时表示可以被子类引用，但不可以被其他类使用
当修饰方法时表示可以被子类引用该方法，但不可以被其他类使用该方法--常用方法
#### 6.8强制类型转化
超类对象引用可以引用子类，但不能通过该引用使用子类的功能，此时需要进行强制类型转化
```java
	person aPerson=new student(); 
	student a=(student) aperson;
```
#### 6.9Object类
##### 6.9.1 equals方法
所有类都有从object类继承而来的equals方法，可以如下改写``` public boolean equals(Object other)```来改写，有时可能覆盖错误，需要在覆盖语句前加入@override来检查
##### 6.9.2hashcode()
使用Object.hashCode(type a,type b,type c)来给类构造hashcode方法
##### 6.9.3toString()
数组的打印使用
```java
Arrays.toString(int[] nums);//一维数组
Arrays.deepToString(int[][] nums)//多维数组
```
#### 6.10泛型数组
##### 6.10.1ArrayList容量的缩减
```java
ArrayList<Integer> a=new ArrayList<>();
a.add(1);
a.trimToSize();//用处是将a的容量缩减至当前所有元素，该操作应在确定a不会再添加或减少容量后使用
```
##### 6.10.2ArrayList转化为数组
```java
ArrayList<Integer> x=new ArrayList<>();
int[] a=new int[x.size()];
x.toArray(a);
```
##### 6.11对象包装器
```int a=Integer.parseInt(s);```将字符串s转化为整数值，字符串需要时整数表示的数
##### 6.12参数数量可变方法
```java
public int max(int...vals)
{
	int max=Integer.MIN_VALUE;
	for(int i:vals)
	{
		if(i>max)
			max=i;
	}
	return max;
}
```
### 7 接口
#### 7.1接口的实现
```java
public interface comparable<T>
{
	int compareTo(T other);
}
```
实现接口
```java
public class person implements comparable<person>
{
	public int compareTo(person a)
	{
		return ...;
	}
}
```
实现接口就要实现接口中的方法，接口内的方法默认时public的不需要标识符
#### 7.2接口的特性
1. 接口可以创建引用对象，但无法实现实例对象，引用的对象必须实现了接口功能
2. 接口可以被继承
3. 接口中可以有域，写作```int a=100```该类型自动被转换为```public static final int```
4. 可以使用```a instanceof interface```来判断a是否实现了接口
5. 一个类可以实现多个接口
#### 7.3comparator接口
comparator接口实现对比
例子：
```java
public interface comparator<T>
{
	int compare(T first,T second);
}
public class lengthComparator implements comparator<String>
{
	public int compare(String first,String second)
	{
		return first.length()-second.length();
	}
}
//需要比较时需要创建一个实例
Comparator<String>a=new lengthComparator<String>;
a.compare(x,y);
```
该比较器也可以传给```Arrays.sort()```函数

#### 7.4克隆接口
##### 7.4.1浅克隆
指开辟一个新空间，基本类型拷贝过来，对象类型仍时指向被克隆对象的实例域

##### 7.4.2深克隆
需要实现```cloneable```接口，该接口是一个标记接口，无内容
需要自己重写clone方法，一般调用```return (T)super.clone()```来实现浅克隆。object类的克隆方法是protected修饰符，故需要自己实现。注意需要进行类型强制转换。
#### 7.5 lambda表达式
