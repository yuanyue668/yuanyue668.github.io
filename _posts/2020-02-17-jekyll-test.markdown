---
layout: post
title:  "Jekyll Test"
date:   2020-02-17 13:51:30 -0000
categories: jekyll update
---
测试是否能够输出中文汉字
{% highlight ruby %}
/*
vector与array数组比较类似 但是array是静态空间,一旦配置了空间就不能够再改变
vecotr是动态空间,随着元素的加入,它的内部机制就会自行扩充空间来容纳新的元素
//下面将真是vector函数的具体实现
//vector是一个连续的线性空间
*/
//alloc是SGI的空间配置器
template <class T,Alloc=alloc>
class vector{
public:
	//vector的嵌套型别定义
	typedef T			value_type;
	typedef value_type* 		pointer;
	typedef value_type*		iterator;
	typedef value_type&		reference;
	typedef size_t			size_type;
	typedef ptrdiff_t		difference_type;
protected:
	//以下,simple_alloc是SGI STL的空间配置 器  专属配置器 每次配置一个元素大小
	typedef simple_alloc<value_type,Alloc>data_allocator;
	iterator 		start;//表示目前使用空间的头
	iterator 		finish;//表示目前使用空间的尾
	iterator 		end_of_storage;//表示目前可用空间的尾
	void insert_aux(iterator position,const T&x);
	void deallocate(){
	if(start)//如果它不是空的
		data_allocator(start,end_of_storage-start);
	}
	void fill_initialize(size_type n,const T& value){
		start=allocate_and_fill(n,value);
		finish=start+n;
		end_of_storage=finish;
	}
public:
	iterator begin(){return start;}
	iterator end(){return end;}
	size_type size() const {return size_type(end()-begin());}
	size_type capacity()const{
	return size_type(end_of_storage-begin());
	}
	bool empty()const{return begin()==end();}
	reference operator[](size_type n){return *(begin()+n);}
	vector():start(0),finish(0),end_of_storage(0){}
	vector(int n,const T& value){fill_initialize(n,value);}
	vector(long n,const T& value){fill_initialize(n,value);}
	explicit vector(size_type n){fill_initialize(n,T());}
	~vector(){
		destory(start,finish);
		deallocate();//这是vector的一个member function
	}
	reference front(){return *begin();}//第一个元素
	reference back(){return *(end()-1);}//最后一个元素
	void push_back(const T&x){//将元素插至最尾端
	if(finish!=end_of_storage){
		construct(finish,x);//全局函数
		++finish;
	}else
	insert_aux(end(),x);//成员函数

	}
	void pop_back(){//将最尾端的元素取出来
	--finish;
	destroy(finish);//全局函数
	}
	iterator erase(iterator position){//清除某个位置上的元素
	if(position+1!=end())
	copy(position+1,finish,position);//后续元素向前移动
	--finish;
	destory(finish);//全局函数
	return position;
	}
	void resize(size_type new_size,const T & x){
	if(new_size<size()){
		erase(begin()+new_size(),end());
	}else{
		insert(end(),new_size-size(),x);
	}
	void resize(size_type new_size){resize(new_size,T());}
	void clear(){erase(begin(),end());}
protected:
	//配置空间并填满内容
	iterator allocate_and_fill(size_type n,const T&x){
		iterator result=data_allocator::allocate(n);
			uninitialized_fill_n(result,n,x);//全局函数
			return result;
			
}
}
};

{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
