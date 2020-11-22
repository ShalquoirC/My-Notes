#### Coroutine协同程序(较难) 

MonoBehaviour.StartCoroutine(string methodName)方法打开另一个协同程序（线程）

IEnumerator 

yield: 声明序列中的下一个值或者是一个无意义的值。 

* 如果使用yield x（x是指一个具体的对象或数值）的话，那么movenext返回为true并且current被赋值为x
* 如果使用yield break使得movenext()返回false。 

```c#
IEnumerator LongComputation()
{
    while(someCondition)
    {
        /* 做一系列的工作 */
 
        // 在这里暂停然后在下一帧继续执行
        yield return null;
    }
}
```



StopCoroutine(string methodName)

StopAllCoroutines()

