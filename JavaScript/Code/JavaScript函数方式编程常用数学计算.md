### JavaScript函数方式编程常用数学计算

> 最近工作过程中，需要做一个功能，就是Java调用JS脚本进行数据计算，然后返回结果，而且参数是不固定的。所以这不，赶紧的温习一下JavaScript，然后经过努力写了一些常用的函数计算案例。

> FunctionList : Average(平均值),Sum(求和),Min(最小值),Max(最大值),Variance(方差),SquareSum(平方和)

> 首先列出Code,供大家参考

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS函数计算案例</title>
</head>
<body>

<script type="text/javascript">

    /**
     * 求传来 不定个数的参数 平均值(数据为双精度)
     */
    function average() {
        if (arguments === null || arguments.length === 0)
            return -1;
        var sum = 0;
        var len = arguments.length;
        for (var i = 0; i < arguments.length; ++i)
            sum += arguments[i];
        return sum / len;
    }
    document.write("平均值:" + average() + "<br />")
    document.write("平均值:" + average(1,2,3,4)+ "<br />")
    document.write("平均值:" + average(5,8,13,9)+ "<br />")



    /**
     * 求传来 不定个数的参数 值的和(数据为双精度)
     */
    function sum() {
        var sum = 0;
        for (var i = 0; i < arguments.length; ++i)
            sum += arguments[i];
        return sum;
    }
    document.write("求和:" + sum()+ "<br />")
    document.write("求和:" + sum(1)+ "<br />")
    document.write("求和:" + sum(1,2,3,4)+ "<br />")



    /**
     * 求传来 不定个数的参数 最小值(数据为双精度)
     */
    function min() {
        if (arguments === null || arguments.length === 0)
            return -1;
        var min = arguments[0];
        var len = arguments.length;

        for (var i=1;i<len;i++){
            if(arguments[i] < min)
                min = arguments[i];
        }
        return min;
    }
    document.write("求最小值:" + min()+ "<br />")
    document.write("求最小值:" + min(1,2,3,4)+ "<br />")
    document.write("求最小值:" + min(5,8,3,9)+ "<br />")



    /**
     * 求传来 不定个数的参数 最大值(数据为双精度)
     */
    function max() {
        if (arguments === null || arguments.length === 0)
            return -1;
        var max = arguments[0];
        var len = arguments.length;

        for (var i=1;i<len;i++){
            if(arguments[i] > max)
                max = arguments[i];
        }
        return max;
    }
    document.write("最大值:" + max()+ "<br />")
    document.write("最大值:" + max(1,2,3,4)+ "<br />")
    document.write("最大值:" + max(5,8,13,9)+ "<br />")



    /**
     * 求给定双精度数组中值的 方差
     */
    function variance() {
        if (arguments === null || arguments.length === 0)
            return -1;
        var sum = 0;
        var len = arguments.length;
        for (var i = 0; i < arguments.length; ++i)
            sum += arguments[i];
        var average = sum / len;
        for (var i = 0; i < arguments.length; ++i)
            sumAV = +(arguments[i] - average) * (arguments[i] - average);
        result = sumAV / len;
        return result;
    }
    document.write("求方差:" + variance()+ "<br />")
    document.write("求方差:" + variance(1,2,3,4)+ "<br />")
    document.write("求方差:" + variance(5,8,13,9)+ "<br />")



    /**
     * 求给定双精度数组中值的平方和
     */
    function squaresum() {
        if (arguments === null || arguments.length === 0)
            return -1;
        var len = arguments.length;
        var sqrsum = 0.0;
        for (var i = 0; i < len; i++) {
            sqrsum = sqrsum + arguments[i] * arguments[i];
        }
        return sqrsum;
    }
    document.write("平方和:" + squaresum()+ "<br />")
    document.write("平方和:" + squaresum(1,2,3,4)+ "<br />")
    document.write("平方和:" + squaresum(5,8,13,9)+ "<br />")
</script>

</body>
</html>
```

> 附上结果吧

![2018625175537](http://note.itdeer.cn/2018625175537.png)