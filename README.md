# swig-android-csharedlibray
java native interface / c code using swig

swig, jdk, android ndk 21d

http://www.swig.org/tutorial.html#Building%20a%20Java%20module

 
 ```sh
 /* File : example.c */
 #include <time.h>
 double My_variable = 3.0;
 #include <android/log.h>

 int fact(int n) {
      __android_log_print(ANDROID_LOG_ERROR,"11","22");
     if (n <= 1) return 1;
     else return n*fact(n-1);
 }
 
 int my_mod(int x, int y) {
     return (x%y);
 }
 	
 char *get_time()
 {
     time_t ltime;
     time(&ltime);
     return ctime(&ltime);
 }
 

Interface file
Now, in order to add these files to your favorite language, you need to write an "interface file" which is the input to SWIG. An interface file for these C functions might look like this :
 /* example.i */
 %module example
 %{
 /* Put header files here or function declarations like below */
 extern double My_variable;
 extern int fact(int n);
 extern int my_mod(int x, int y);
 extern char *get_time();
 %}
 
 extern double My_variable;
 extern int fact(int n);
 extern int my_mod(int x, int y);
 extern char *get_time();
 
 
 --

swig -java example.i

gcc -c example.c example_wrap.c -I/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/include -I/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/include/darwin (for mac dylib)

gcc -dynamiclib -o libexample.dylib -dy example.o example_wrap.o. (for mac dylib)

sudo ./aarch64-linux-android21-clang -shared -o libexample.so -Xlinker -soname=libexample.so /Users/berkintatlisu/example.c /Users/berkintatlisu/example_wrap.c
(for android .so arm64) 

sudo ./aarch64-linux-android21-clang -shared -o libexample.so -Xlinker -soname=libexample.so /Users/berkintatlisu/example.c /Users/berkintatlisu/example_wrap.c -llog

 $ cat main.java
 public class main {
   public static void main(String argv[]) {
     System.loadLibrary("example");
     System.out.println(example.getMy_variable());
     System.out.println(example.fact(5));
     System.out.println(example.get_time());
   }
 }
 $ javac main.java
 $ java main
 
```


