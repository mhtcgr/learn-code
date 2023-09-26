刚debug了好一会的题：DynamicArray
===
话不多说，上代码：
```
#include <iostream>

class DynamicArray {
public:
    DynamicArray() {
        _capacity = 10; // 初始容量为10
        _arr = new int[_capacity];
        _currentSize = 0;
    }
    ~DynamicArray() {
        delete[] _arr;
    }

    // 向数组末尾添加元素，如果数组已满，扩展容量，扩展为原来的两倍
    void append(int value) {
        _arr[_currentSize]=value;
        _currentSize++;
        if(_currentSize==_capacity){
            _capacity*=2;
            int *pointer = new int[_capacity];
            memcpy(pointer, _arr, sizeof(int) * _currentSize);
            //write ur code here.

        }
    }
    // 删除指定索引的元素，如果剩余元素不足capacity的一半，将capacity缩小一半，最小容量为10
    void erase(int index) {
        //write ur code here.
        for(;index<_currentSize-1;index++)
            _arr[index]=_arr[index+1];
        _arr[(_currentSize)-1]=0;
        _currentSize--;
        if(_currentSize<_capacity/2){
            if(_capacity/2<=10)
                _capacity=10;
            else _capacity/=2;
            int *pointer = new int[_capacity];
            memcpy(pointer, _arr, sizeof(int) * _capacity);
        }
    }

    // 获取指定索引的元素
    int get(int index) {
        //write ur code here.
        return _arr[index];
    }

    // 设置指定索引的元素值
    void set(int index, int value) {
        //write ur code here.
        _arr[index]=value;
    }

    // 返回数组当前大小
    int size() {
        //write ur code here.
        return _currentSize;
    }

    // 返回数组容量
    int capacity() {
        //write ur code here.
        return _capacity;
    }

    // 打印数组的所有元素
    void print() {
        //write ur code here.
        for(int i=0;i< size();i++)
            std::cout << _arr[i] << " ";
        printf("\n");
    }

private:
    int* _arr;
    int _capacity;
    int _currentSize;
};
```
其实题目并不难，一开始就作对了，但是后面助教又加了一个样例，我原来使用的直接malloc动态分配超时了，realloc也是rt error，后来搜了一下，发现这个方法比较快，就是memcpy，复制内存，该函数不检查源中是否有任何终止空字符 - 它始终精确地复制数字字节，所以在确定不会越界的情况下使用它是很好的。
by the way,realloc缩小数组好像有坑？会占用大量CPU资源。一般情况下应避免调用realloc。
