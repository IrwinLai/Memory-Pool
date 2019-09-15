## Readme

### Abstract

1. There are some drawbacks of memory management in C++, including wasting time and space, causing memory leak.

2.	Hence, there is a memory pool, based on arrays, to avoid memory leak and saving more than 70% time in allocating memory.  
3. 'code' is about how to creat and test the memory pool.  

### Idea

Memory management in C / C ++ is a headache. When we write a program, we will undoubtedly need to allocate logic. There are several drawbacks:
 
1.When allocating memory, the system will find a free memory in the memory free block table according to the first matching principle. And when freeing up memory, the system may need to merge free memory blocks, which incurs additional overhead.  
2.Frequent application, allocation, and release of memory will generate a large amount of memory fragmentation, thereby reducing the running speed of the program.  
3.It is easy to cause memory leaks.  
4.When the available memory is insufficient, the application memory will exit with an error.

The correct way to solve these problems should be to consider how to manage and optimize the memory that you have used when you run out of memory, in order to make the software more usable. Therefore, in this project, we will realize the dynamic programming of memory through a memory pool. 

1.The memory pool allows the memory block to be planned in a constant time.  
2.Memory fragmentation is rarely generated.  
3.The phenomenon of memory leak can be avoided, and only one operation is required to return all memory blocks at a time.

We will detail our design ideas and implementation methods, and test the allocation performance provided by our memory pool, and compare the default allocation method to study whether the memory pool can improve the efficiency of using memory.


First, we record the first and last addresses of the allocated memory, PoolStart and PoolEnd. Each time the Allocator requests memory from the memory pool, it allocates memory by returning the first address and changing the position of the first address. If there is not enough memory, change the tail address after applying for a large amount of memory. For the management of allocated memory, and the secondary utilization of the released memory, we use arrays to implement memory management. Like the folling pitcture shows.

<center>  
<img src = "https://tva1.sinaimg.cn/large/006y8mN6ly1g701l59w8ej314f0pj3zw.jpg" width="50%" height="50%" />
</center>

When the Allocator applies for memory, it calculates how much memory is needed, indexes the location to the current first address, and modifies the first address for allocation. Give priority to the remaining space in the memory pool. If there is enough memory left, the function will call the function to apply for a piece of memory, return the first address and adjust it to the appropriate position. If it is still not enough, reapply a block of memory and then return and adjust the first address.

<center>  
<img src = "https://tva1.sinaimg.cn/large/006y8mN6ly1g701lgrr1lj319b0mpabh.jpg" width="660%" height="66%" />
</center>


### Result 
 
Test both point and int type for 10000 times allocation, and do 10 times tests to get the average result.

Test result on Mac OS. 79% time has been saved.
<center>  
<img src = "https://tva1.sinaimg.cn/large/006y8mN6ly1g701dpbvlej311k0i40w6.jpg" width="66%" height="66%" />
</center>

Test result on Window. 69% time has been saved.
<center>  
<img src = "https://tva1.sinaimg.cn/large/006y8mN6ly1g701faeovuj312g0is41h.jpg" width="66%" height="66%" />
</center>
