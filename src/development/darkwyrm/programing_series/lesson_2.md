# 第二课

在第一节里面，我们学习了如何使用模板和一些标准模板库（STL）中非常灵活的数据容器来泛化类型处理。列表，队列和适量容器都非常适合存储顺序访问数据，但是对于 STL 来说，它们不仅仅只有三个。我们将要学习其他重要的容器类型，并了解它们彼此之间的异同点。我们还会涉及到可移植字符串类，它将提供比标准 C 函数更简单的字符串操作。在本节中，将会涉及到很多内容。遇到不懂得问题，请不要慌张，多读几遍，其义自现。

## C++ 字符串

在开始了解其他容器之前，我们需要了解 C++ 标准库（Standard C++ library），其中用到了 STL 的内容，但是提供了其他有用的开发工具。而通用的字符串（string）类就是其中之一。

使用标准 C 函数，诸如 strcpy() 和 strstr() 等来处理字符串非常方便。Support Kit 中的 Bstring 类也提供了更为便利的操作，而 C++ string 类也同样。通常您可能会选用 Bstring 类，因为它会比较快，而且提供的操作也很多，并且整合到了 Haiku API，但是 C++ string 类提供了 Bstring 类中所没有的一些方法。

    string substr(size_t pos = 0, size_t n = npos);

substr() 返回一个以 pos 位置字符开始，并以 n 位置结束的子字符串。npos 是一个静态值，其值与字符串最大长度相等。毫无疑问，我们不会使用这两个默认值来调用该函数，因为它返回的还是整个字符串。

    size_t find_first_not_of (const string& str, size_t pos = 0) const;
    size_t find_first_not_of (const char* s, size_t pos, size_t n) const;
    size_t find_first_not_of (const char* s, size_t pos = 0) const;
    size_t find_first_not_of (char c, size_t pos = 0) const;
    size_t find_last_not_of (const string& str, size_t pos = npos) const;
    size_t find_last_not_of (const char* s, size_t pos, size_t n) const;
    size_t find_last_not_of (const char* s, size_t pos = npos) const;
    size_t find_last_not_of (char c, size_t pos = npos) const;

这两个函数用以查询 string 对象中第一（最末）个并不是函数中第一个参数所指定的字符所出现的位置。使用方法如下：

    std::string myString = “/boot/home/config/setting”;
    size_t pos = myString.find_first_not_of(‘/’);

上述示例中，find_first_not_of() 中的 char 字符用于查找首个非反斜线的字符。该示例中 pos 的值为1。配合使用，那么它可以用于将文件路径拆分为一系列的目录名称，并且还不需要使用 strtok()。

C++ 字符串需要在 std 命名空间中使用，并且需要添加前缀 std::string 以便和常规 C 风格的字符串相区分。它们需要和 头文件一起使用。我们不会太多的使用 C++ 字符串，但是最好知道它们的存在，因为在其他平台上它们的使用非常广泛。

## 关联容器

由标准模板库提供的关联容器对我们来说是非常必要的，尤其是在容器中使用整型查询数据非常慢或无法实现时。尽管我们能够使用循环来手工查询数组，矢量容器或者其他顺序容器，但是速度非常慢。对于关连查询，它非常适用，但是如果您必须以这种方式重复查询信息，它可能会给你的代码带来严重的问题。

### map

头文件：`<map>`

map 容器以键值对为中心，例如查询值，那么也就获取到了相关的数据。map 对象的声明需要同时指明键和值得类型，如下所示：

    map<Bstring, int32> myMap;
	
该声明创建了一个 map，其使用字符串来查询整型数据。使用字符串作为键来查询数据的 map 很常见。如果使用 std::string 或者 BString 作为键类型，那么也能够使用常规 C 类型的字符串来获取值。

    printf(“The value for %s is %d\n”, “Some value”, myMap[“Some value”]);
	
使用 map 时，唯一的要求就是键值必须是唯一的。map 对象中的元素实际上是另一个 STL 容器：pair。pair 容器仅将两种类型关联起来。可以通过 first 和 second 属性来访问这两个成对的类型。

### set

set 容器和 map 容器非常相似，除了其中的值也是键，并且它们都做了排序。和 map 一样，set 中的所有元素都必须是唯一的。但是它并不经常使用，因为还有些容器要更加的灵活。通常使用 set 容器是为了更加快速的插入和查询。因为 set 容器的实现通常比较复杂，所以它可以提供这些特性。

### multimap，multiset

这两种 set 和 map 类型不要求键所对应的实例是唯一的。调用 find() 方法仍然只返回给定键所对应的一个实例，但是这些容器包含了一个附加的方法，equal_range()，它返回一对迭代器，而这对迭代器所标定的范围提供了该指定键所对应的所有实例。

## 容器适配器

标准模板库除了提供这些容器外，还提供了一些容器适配器。它们使用常规的STL容器为指定接口承担重任。

### queue

queue 适配器通常建立在 deque 容器之上。从概念上说，对象从队列的尾部进去，而从头部出来，非常类似于排队买电影票。通常这种策略也称之为 FIFO，即先进先出。它提供的方法有 front()，back()，push_back()，和pop_front()。

### priority_queue

priority_queue 适配器和 queue 适配器非常相似，但惟有的不同是：第一个进去的对象并不是第一个出来的。而第一个出来的是具有最高优先级的对象。通常可以用来担此重任的两种容器是 vector（默认的）和 deque。

### stack

stack 可以建立在 deque，vector，和 list 容器之上。它用于 LIFO 处理，也就是后进先出。它可以比作自助餐厅堆叠的瓷盘：最后放在上面的盘子，将会第一个被拿走。

## 常用 STL 容器方法

STL提供了很多不同的容器，有时候我们很难记得它们哪个是哪个。幸运的是，它们有一组通用的方法。

    iterator begin();
    const_iterator begin() const;

返回指向容器首个元素的迭代器。由于关联容器以升序排列所有元素，begin() 将会返回具有最小值的元素。

    iterator end();
    const_iterator end() const;
	
返回指向容器末尾元素之后的位置的迭代器 - 其和最后一个元素并不相同。该方法通常用于循环中，尤其是 for 循环。

    iterator rbengin();
    const_iterator rbgin() const;
    iterator rend();
    const_iterator rend() const;

以上两个方法和 begin() 与 end() 完成同样的任务，但是它们从容器末尾向容器开头进行工作。rbegin() 返回容器的末尾元素，而 rend() 返回首个元素之前位置的迭代器。这两个方法和使用 reverse_iterator 的循环相一致，而并非一般循环。

    size_type size() const;
	
返回容器所包含对象的数量。

    size_type max_size() const;
	
max_size() 返回容器所能够包含的对象的最大数量，而这基于系统所设定的限制。

    bool empty() const;
	
如果容器包含零个元素则返回真。

    void resize(size_type newSize, T from = T());
	
修改容器大小以保存 newSize 个元素。如果这个数小于当前数目，那么多余的元素将被丢弃。如果新的大小比较大，那么将会以参数 from 传递的对象创建新的元素。如果未指定该参数，那么将会使用默认构造函数创建容器对象类型。该方法仅对顺序容器可用，例如 vector。

    reference front();
    const reference front() const;
    reference back();
    const reference back() const;

以上两个方法分别返回容器的头部和尾部元素。它们返回的结果与 begin() 和 rbegin() 返回的迭代器是不同的。这两个方法不适用于关联容器，例如 map。

    vector, deque
    reference operator[size_type index];
    const_reference operator[size_type index] const;
     
    map
    T & operator[const key_type& key];

对 deque，vector 和 map 使用数组操作符将会返回指定索引所对应的元素。对于这里的 map，则是对应于指定键的对象。如果该map 中没有对应于指定键的对象，它将会被创建，并且赋以一个空的对象。该操作符仅对这三种容器适用。

    reference at(size_type index);
    const_reference at(size_type index) const;

at() 和数组操作符非常相似，但是有两点不同：它仅对 deque 和 vector 容器使用，并且如果所用的索引超出了容器边界，它将会抛出一个 out_of_bounds 例外。

    template class<InputIterator>
    void assign(InputIterator first, InputIterator last);
    void assign(size_type newSize, const T& from);

assign 是一种为顺序容器所有元素一次性赋同一个值，以及实现容器拷贝的便捷方式。前一种方法从其他容器中拷贝元素，并将其拷贝到其所属容器的 first 和 last 之间，但并不覆盖 first 迭代器。后一种方法将所有容器元素设置为 from 的值。在这两种情况中，容器大小将被修改为 newSize，或者迭代器范围所指定的元素数量。

    iterator insert(iterator pos, const T& item);
    template <class InputIterator>
    void insert(iterator pos, InputIterator first, InputIterator last);
     
    /* 仅适用于 vector，deque，list */
    void insert(iterator pos, size_type count, const T& item);
     
    /* 仅适用于 map 和 set */
    pair<iterator, bool> insert(const value_type& item);
     
    /* 仅适用于 multimap 和 multiset */
    iterator insert(const value_type& item);

insert() 添加元素到容器。该方法是所有的 STL 容器都通用的，尽管每个容器所使用的函数形式各异。元素插入的速度取决于容器的实现方式。例如，在 vector 容器中间添加元素比较慢，但是在其尾部添加则非常快速。

    iterator erase(iterator position);
    iterator erase(iterator first, iterator last);
     
    /* 仅适用于 set，multiset，map，和 multimap */
    size_type erase(const key_type& lookupValue);

erase() 用于删除容器中的元素。和 insert() 相似，它的性能也依赖于容器的实现方式。

    swap(<container to swap with>);
	
该函数接收同样类型的容器，并交换两个容器中的元素。所有和两个容器中的元素相关的指针，引用，以及其他外部数据都保持有效。

    void clear();
	
一言以蔽之，该函数用以删除容器中的所有元素，即将其清空。

    void push_front(const T& item);
    void pop_front();

以上两个函数允许您从 deque 和 list 的头部添加或者删除元素。Pop_front() 不仅从容器中移除该元素，同时还删除钙元素。

    void push_back(const T& item);
    void pop_back();

以上两个函数和前面的 front 操作相同，但是它们在容器尾部进行改动。而且，除了 deque 和 list 可用外，还对 vector 容器可用。

    key_compare key_comp() const;

该函数返回用于比较容器中元素的对象。它可以是函数指针，实现函数调用操作符的类的实例。比较函数比较容器中两个对象的类型，如果第一个元素小于或者其在容器中处于第二个参数之前则返回真，反之则返回假。该函数仅对关联容器可用。

    value_compare val_comp() const;
	
该函数和 key_comp() 相似，但是它返回用以比较两个值的函数。对于 set 容器而言，它和 key_comp() 相同。该函数同样仅对关联容器可用。

    iterator find(const key_type& lookupValue) const;

查询容器中和 lookupValue 相匹配的元素，并且返回指向该元素的迭代器，如果未找到，则返回 end()。该函数仅对关联容器可用。

    size_type count(const key_type& lookupValue) const;

返回容器中和 lookupValue 相匹配的元素个数。尽管该方法对所有关联容器可用，但是它仅对 multiset 和 multimap 有意义，因为对于 map 和 set 所要查询的值总是唯一的。

    iterator lower_bound(const key_type& lookupValue);
    const iterator lower_bound(const key_type& lookupValue) const;
    iterator upper_bound(const key_type& lookupValue) const;
    pair<iterator, iterator> equal_range(const key_type& lookupValue) const;

lower_bound() 返回指向容器中第一个大于或者等于 lookupValue 的元素的迭代器。upper_bound() 返回指向容器中第一个大于lookupValue的元素的迭代器。equal_range() 返回两个迭代器，第一个和 lower_bound（lookupValue）相同，而第二个与 upper_bound（lookupValue）相同。和 count() 相似，以上方法均对所有的关联容器可用，但是它们仅对 multiset 和 multimap 有意义。

## STL 和 标准库：那又怎样？

在简短的介绍玩命名空间和模板，我们又旋风般扫过许多不同的模板类，这可能有点快了，有点难以接受。不过无须过多担心，它们并不会如您所想那般经常使用。Haiku 的 Tracker 内部有一个 BObjectList 类，其提供了 BList 类所有的易用特性，并且还能够实现内存管理。本文覆盖了索引存储的需要。map 和 multimap 容器非常适于用作随机存储容器。其他的容器则更多的用于特殊实例的引用，跨平台编程，以及其他代码的识别。在容器的嵌套中，它们也非常必要，例如 vector 的 map 容器。

对于 C++ 标准库，也是同样。它们中有些对于 Haiku 开发会非常方便，但是另一些可能暂时没那么有用。它们的用途部分会依赖于您在其他平台上的开发，例如 Linux 和 Windows。如果您打算只为 Haiku 编程，您可能不会经常用到标准库和 STL，但是如果您也为其他平台做开发，那么它们的使用将会使平台之间的迁移更加的容易。