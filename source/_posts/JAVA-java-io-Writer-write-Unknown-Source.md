---
title: JAVA java.io.Writer.write(Unknown Source)
date: 2016-11-05 14:35:51
tags: JAVA
---
今天在做一个使用JAVA的输入输出流将一个文本文件的内容按行读出，每读出一行就顺序添加行号，并写入到另一个文件中的练习时，写入文件的过程中遇到未知错误如下：

```
Exception in thread "main" java.lang.NullPointerException
	at java.io.Writer.write(Unknown Source)
	at sy_10.sy10_7.main(sy10_7.java:26)
```

我程序代码是：

```
package sy_10;
import java.io.*;
public class sy10_7 {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		File f = new File("E:\\Coder\\java\\eclipse\\test\\src\\sy_10\\Example10_7.java");
		File f1 = new File("E:\\Coder\\java\\eclipse\\test\\src\\sy_10","Example10_7.txt");
		String s = null;
		int i = 1;
		String content[] = new String[50];
        try{
        	FileReader inOne = new FileReader(f);
        	BufferedReader inTwo = new BufferedReader(inOne);	
        	while((s = inTwo.readLine())!= null){
        		s = i + ".  " + s;
        		content[i-1] = s;
        		i++;
        		System.out.println(s);        		
        	}
        	inTwo.close();
        	inOne.close();
        	FileWriter outOne = new FileWriter(f1);
        	BufferedWriter outTwo = new BufferedWriter(outOne);
            for(String str:content){
            	outTwo.write(str);
            	outTwo.newLine();
            }           
            outTwo.close();  
            outOne.close();
        }
        catch(IOException e){
        	System.out.println(e);
        }
	}
}
```

这个错误以前没有遇到过，错误地方就在

```
outTwo.write(str);
```

是写入时有问题，刚开始以为是参数str类型不对，然而并不是，str是String类型没问题，单独输出正常。

网上找了一下，很多说是输入输出流关闭不正确，我想可能是字符流跟缓冲流的关闭顺序不对，把各种顺序都试了一遍，哈！！然而也不是..

再把写出的操作注释掉，程序正常显示。好吧，把outTwo.write(str);断点然后debug找找原因，在写入时我发现数组content有点异常，初始化时分配了50个数组元素，即表示写入文件时要写入50行，而实际需要写入的只有32行。

把数组元素改为32个时就可以正常写入了….我想这大概是内存溢出导致的异常。具体原因我也不知道。但程序这样写肯定是不行的。因为文件不能总是只有32行吧。

我觉得这应该是数组到了32个元素之后就没有值了，即为null值。所以往文本中写入null值就会出错，这样Unknown Source就可以解释了。所以改了一下写入文本的for循环：

```
for(String str:content){
            	if(str == null){
            		break;
            	}
            	outTwo.write(str);
            	outTwo.newLine();
            }
```

这样之后，把content数组设置为50个元素也不会出错了！！

debug再次救了我