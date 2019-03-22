### PHP基础函数
	json_encode(); //对变量进行 JSON 编码
	hex2bin() //16进制转为ASCII
	file_exists() //检查文件或目录是否存在
### 字符串相关
explode() 函数把字符串打散为数组
> explode(separator,string,limit)     
      
    参数	        描述
    separator	必需。规定在哪里分割字符串
    string	        必需。要分割的字符串
    limit	        可选。规定所返回的数组元素的数目
file_put_contents() 函数把一个字符串写入文件中
> file_put_contents(file,data,mode,context)

    参数	    描述
    file	    必需。规定要写入数据的文件。如果文件不存在，则创建一个新文件。
    data	    可选。规定要写入文件的数据。可以是字符串、数组或数据流。
    mode	    可选。规定如何打开/写入文件。可能的值：
                FILE_USE_INCLUDE_PATH
                FILE_APPEND
                LOCK_EX
    context	    可选。规定文件句柄的环境。
    context     是一套可以修改流的行为的选项。若使用 null，则忽略。
str_replace() 函数以其他字符替换字符串中的一些字符（区分大小写）
> str_replace(find,replace,string,count)

    参数	    描述
    find	    必需。规定要查找的值。
    replace	    必需。规定替换 find 中的值的值。
    string	    必需。规定被搜索的字符串。
    count	    可选。对替换数进行计数的变量。  
### 数组相关
array_merge() 函数把一个或多个数组合并为一个数组
> array_merge(array1,array2,array3...)      

    参数	    描述
    array1	    必需。规定数组。
    array2	    可选。规定数组。
    array3	    可选。规定数组。
