# Proxy2
package testforjava;

import java.io.*;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.net.MalformedURLException;
import java.net.URL;
import java.nio.charset.Charset;
import java.nio.file.*;
import java.nio.file.attribute.BasicFileAttributeView;
import java.nio.file.attribute.BasicFileAttributes;
import java.util.*;
import java.util.zip.*;


//C:\Users\zhakypbekov_30855\Desktop\test1//Записи2.txt
interface SomeInterfaceWithMethods {
    void voidMethodWithoutArgs();

    String stringMethodWithoutArgs();

    void voidMethodWithIntArg(int i);
}
class SomeInterfaceWithMethodsImpl implements SomeInterfaceWithMethods {
    public void voidMethodWithoutArgs() {
        System.out.println("inside voidMethodWithoutArgs");
    }

    public String stringMethodWithoutArgs() {
        System.out.println("inside stringMethodWithoutArgs");
        return null;
    }

    public void voidMethodWithIntArg(int i) {
        System.out.println("inside voidMethodWithIntArg");
        voidMethodWithoutArgs();
    }
}
public  class Person {
	
	public static void main(String[] args) {
        SomeInterfaceWithMethods obj = getProxy();
        obj.stringMethodWithoutArgs();
        obj.voidMethodWithIntArg(1);

        /* expected output
        stringMethodWithoutArgs in
        inside stringMethodWithoutArgs
        stringMethodWithoutArgs out
        voidMethodWithIntArg in
        inside voidMethodWithIntArg
        inside voidMethodWithoutArgs
        voidMethodWithIntArg out
        */
    }

    public static SomeInterfaceWithMethods getProxy() {
    	SomeInterfaceWithMethodsImpl someInterface = new SomeInterfaceWithMethodsImpl();
    	SomeInterfaceWithMethods object = (SomeInterfaceWithMethods) Proxy.newProxyInstance(SomeInterfaceWithMethodsImpl.class.getClassLoader(), 
    																							SomeInterfaceWithMethodsImpl.class.getInterfaces(), 
    																				            new CustomInvocationHandler(someInterface));
        return object;
    }
	
}
class CustomInvocationHandler implements InvocationHandler{
	private SomeInterfaceWithMethods originalObject;
	public CustomInvocationHandler(SomeInterfaceWithMethods obj){
		this.originalObject = obj;
	}
	
	public Object invoke(Object arg0, Method arg1, Object[] arg2) throws Throwable {
		System.out.println(arg1.getName() + " in");
		SomeInterfaceWithMethods s = (SomeInterfaceWithMethods)arg1.invoke(originalObject, arg2);
		System.out.println(arg1.getName() + " out");
		return s;
	}
	
}
