## 数组转字符串

```
int[] array = new int[]{1, 2, 3};
out.println(Arrays.toString(array)); //[1, 2, 3]
```
如果是一维数组，toString方法可以很好的适用。但遇到多维数组时，需要使用deepToString把数组完全转成字符串。
```
int[][] deepArray = new int[][]{{1, 3},{2, 4}};
out.println(Arrays.toString(deepArray)); //[[I@1540e19d, [I@677327b6]
out.println(Arrays.deepToString(deepArray)); //[[1, 3], [2, 4]]
```

## 填充数组


```
array = new int[5];
Arrays.fill(array, 2);
out.println(Arrays.toString(array)); //[2, 2, 2, 2, 2]

array = new int[5];
Arrays.fill(array, 1, 4, 2); //部分填充
out.println(Arrays.toString(array));//[0, 2, 2, 2, 0]
```

## 数组元素排序

```
array = new int[]{3, 10, 4, 0, 2};
Arrays.sort(array);
out.println(Arrays.toString(array)); //[0, 2, 3, 4, 10]

array = new int[]{3, 10, 4, 0, 2};
Arrays.parallelSort(array); //和sort相比是这个是并行的
out.println(Arrays.toString(array)); //[0, 2, 3, 4, 10]

array = new int[]{3, 10, 4, 0, 2};
Arrays.sort(array, 0, 4); //部分排序
out.println(Arrays.toString(array)); //[0, 3, 4, 10, 2]
```

## 数组的比较

```
array = new int[]{1, 2, 3};
int[] array2 = new int[]{1, 2, 3};
out.println(Arrays.equals(array, array2)); //true
```
和toString方法一样，equals方法遇到多维数组时也会出现问题。
```
int[][] deepArray1 = new int[][]{{1, 3},{2, 4}};
int[][] deepArray2 = new int[][]{{1, 3},{2, 4}};
out.println(Arrays.equals(deepArray1, deepArray2)); //false
out.println(Arrays.deepEquals(deepArray1, deepArray2)); //true
```
deepEquals用于判定两个指定数组彼此是否深层相等，此方法适用于任意深度的嵌套数组。

equals用于判定两个数组是否相等，如果两个数组以相同顺序包含相同元素，则返回true。

如果两个数组使用equals返回true，则使用deepEquals必定也返回true，也就是说在比较的两个数组均为一维数组的前提下，equals和deepEquals的比较结果没有差别。

## 数组复制

```
array = new int[]{3, 10, 4, 0, 2};
int[] arrayCopy = Arrays.copyOf(array, 3);
out.println(Arrays.toString(arrayCopy)); //[3, 10, 4]

arrayCopy = Arrays.copyOf(array, 7);
out.println(Arrays.toString(arrayCopy)); //[3, 10, 4, 0, 2, 0, 0], 多出的长度补0

arrayCopy = Arrays.copyOfRange(array, 1, 4);
out.println(Arrays.toString(arrayCopy)); //[10, 4, 0]
```

## 二分查找返回下标

```
array = new int[]{0, 3, 4, 10, 20};
out.println(Arrays.binarySearch(array, 10)); //3, array必须是排序的，否则得到的是错误的结果
out.println(Arrays.binarySearch(array, 6)); //-4, 找不到的值，从-1开始，6如果存在下标是3， 所以返回-4
out.println(Arrays.binarySearch(array, 2, 5, 10)); //3, 返回的还是全局的下标值。
```

## 数组转List

```
int array = new int[]{3, 10, 4, 0, 2};
out.println(Arrays.asList(array).contains(3)); //false

Integer arr[] = new Integer[]{3, 10, 4, 0, 2};
out.println(Arrays.asList(arr).contains(3)); //true
```
这里是比较有意思的地方，实际上拆开来看是这样的

```
int array = new int[]{3, 10, 4, 0, 2};
List<int[]> ints = Arrays.asList(array);
```

```
Integer arr[] = new Integer[]{3, 10, 4, 0, 2};
List<Integer> integers = Arrays.asList(arr);
```
现在就知道区别了，原始数据类型int的数组调用asList之后得到的List只有一个元素，这个元素就是元素类型的数组。而封装类Integer数组调用asList是把数组中每个元素加到了List中。


## 对数组元素采用指定的方法计算

```
array = new int[]{3, 10, 4, 0, 2};
Arrays.parallelPrefix(array, (x,y)->(x+y)); //[3, 13, 17, 17, 19]
out.println(Arrays.toString(array));

array = new int[]{3, 10, 4, 0, 2};
Arrays.parallelSetAll(array, (x)->(x*x)); //[0, 1, 4, 9, 16]
out.println(Arrays.toString(array));

Arrays.setAll(array, (x)->(x%3));
out.println(Arrays.toString(array)); //[0, 1, 2, 0, 1], 与parallelSetAll相比只是不并行
```
## 对其他对象数组进行排序
一个对象数组，排序算法需要重复比较数组中的元素。不同的类比较元素的规则是不同的，但是排序算法只应该调用类提供的比较方法，只要所有的类就比较的时候提供的方法达成一致，那么排序算法就能开始工作。这个在排序时对象之间进行比较方法就可以是一个接口，所有需要比较的对象继承这个接口并且实现比较的方法，就可以对这些对象进行排序。
如果一个类想启用对象排序，那么就应该实现Comparable接口。

```
public class Test{
    public static void main(String[] args){
		Employee[] employees = new Employee[3];
		employees[0] = new Employee(20);
		employees[1] = new Employee(10);
		employees[2] = new Employee(30);
		Arrays.sort(employees);
		for(Employee e : employees){
			System.out.println(e); //Employee{id=10} Employee{id=20} Employee{id=30}
		}
		
    }
    static class Employee implements Comparable<Employee>{
        private int id;
        public Employee(int id){this.id = id;}
@Override
        public int compareTo(Employee o) {
            return this.id - o.id;
        }
        @Override
        public String toString() {
            return "Employee{" + "id=" + id + '}';
        }
    }
}
```

## 自定义排序规则
```
String[] names = {"tom", "alice", "fred"};
Arrays.sort(names);
out.println(Arrays.toString(names));
```
假如想根据字符串的长度而不是根据字典顺序对字符串排序，但是String类我们是无法修改的。上面的代码对String数组进行排序，只能按照字典顺序对String数组进行排序。
`Arrays.sort`方法和`Collections.sort`方法都提供了一个可以接收`Comparator`实例作为第二个参数的版本。
要按照长度比较字符串，定义一个实现`Comparator<String>`的类。

```
public class LengthComparator implements Comparator<String> {
    @Override
    public int compare(String o1, String o2) {
        return o1.length() - o2.length();
    }
    public static void main(String[] args){
        String[] names = {"tom", "alice", "fred"};
        Arrays.sort(names, new LengthComparator());
        out.println(Arrays.toString(names));
    }
}
```
像Comparator、Runable等这=一些接口有一个特点就是只有一个抽象方法（其他的都是static或者default的方法），比如继承Comparator接口只需要重写compare方法，继承Runnable接口只需要重写run方法，这种类型的接口被称为函数式接口，可以被lambda表达式所代替。
比如上面根据字符串的长度进行排序的代码，`Arrays.sort`的第二个参数是需要实现了Comparator接口的实例，用lambda表达是就可以写成这样：

```
String[] names = {"tom", "alice", "fred"};
Comparator<String> comp = (first, second) -> first.length() - second.length();
Arrays.sort(names, comp);
```
或者更加简单点

```
String[] names = {"tom", "alice", "fred"};
Arrays.sort(names, (first, second) -> first.length() - second.length());
```
Arrays.sort方法的第二个参数变量接受一个实现了Comparator接口的类的实例，调用该对象的compare方法会执行lambda表达式中的代码，所以这也就是为什么接口只有一个抽象方法的时候可以用lambda表达式代替。


