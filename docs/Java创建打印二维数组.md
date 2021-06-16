## 创建二维数组

-  直接创建并赋值： 

~~~java
int [][] array1 = {{1,2,3,4},{5,6,7,8},{9,10,11,12}};
~~~

* 指定大小进行创建

~~~java
int [][] array2 = new int [4][8];//一边声明一边分配内存

int [][] array3;
array3 = new int [4][8]; //先声明再分配

//两个for循环进行赋值
~~~

## 打印二维数组

*  类似一维数组的Arrays.toString()，二维数组可以直接调用Arrays.deepToString()方法

~~~java
/**
* 源码
*/
 public static String deepToString(Object[] a) {
        if (a == null)
            return "null";

        int bufLen = 20 * a.length;
        if (a.length != 0 && bufLen <= 0)
            bufLen = Integer.MAX_VALUE;
        StringBuilder buf = new StringBuilder(bufLen);
        deepToString(a, buf, new HashSet<>());
        return buf.toString();
    }

    private static void deepToString(Object[] a, StringBuilder buf,
                                     Set<Object[]> dejaVu) {
        if (a == null) {
            buf.append("null");
            return;
        }
        int iMax = a.length - 1;
        if (iMax == -1) {
            buf.append("[]");
            return;
        }

        dejaVu.add(a);
        buf.append('[');
        for (int i = 0; ; i++) {

            Object element = a[i];
            if (element == null) {
                buf.append("null");
            } else {
                Class<?> eClass = element.getClass();

                if (eClass.isArray()) {
                    if (eClass == byte[].class)
                        buf.append(toString((byte[]) element));
                    else if (eClass == short[].class)
                        buf.append(toString((short[]) element));
                    else if (eClass == int[].class)
                        buf.append(toString((int[]) element));
                    else if (eClass == long[].class)
                        buf.append(toString((long[]) element));
                    else if (eClass == char[].class)
                        buf.append(toString((char[]) element));
                    else if (eClass == float[].class)
                        buf.append(toString((float[]) element));
                    else if (eClass == double[].class)
                        buf.append(toString((double[]) element));
                    else if (eClass == boolean[].class)
                        buf.append(toString((boolean[]) element));
                    else { // element is an array of object references
                        if (dejaVu.contains(element))
                            buf.append("[...]");
                        else
                            deepToString((Object[])element, buf, dejaVu);
                    }
                } else {  // element is non-null and not an array
                    buf.append(element.toString());
                }
            }
            if (i == iMax)
                break;
            buf.append(", ");
        }
        buf.append(']');
        dejaVu.remove(a);
    }
~~~

* for循环打印

~~~java
	//简写一下，就这意思 自己修饰下加个括号啥的
	for (i = 0; i < row; i++){
		for (j = 0; j < col; j++){
			//print(arr[i][j]);
		}
	}
	//也可以只用一个for 稍微优化一点
	for (int i = 0, j = 0; i < a.length;) {
            System.out.print(a[i][j] +" ");
            j++;
            if (j >= a[i].length) {
                i++;
                j = 0;
            }
        }
~~~

*好多东西不常用都忘记了，记录一下防止遗忘*

*感谢看到这里~*

