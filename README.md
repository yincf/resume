//git
//数组的去重
Array.prototype.myUnique = function(){
var obj = {};
for(var i = 0;i< this.length; i++){
var cur = this[i];
if(obj[cur]==cur){
this[i] = this[this.length-1];
this.length --;
i--;
continue;
}
obj[cur] = cur;
}
obj = null;
return this;
};
//var ary = [1,1,2,3,5,21,1,2]
//arr.myUnique().sort(function(a,b){retuen a-b});console.log(ary);
/\*
//获取与设置 transform 的值，PS->只有通过这里设置的值才可以通过这里去获取
var cssTransform = (function(){
return {
init:function(element,attr,val){
if(!element.transform){
element.transform = {};
}
if(typeof val == 'undefined'){
if(!element.transform[attr]){
switch(attr){
case 'scale':
case 'scaleX':
case 'scaleY':
case 'scaleZ':
element.transform[attr] = 100;
break;
default:
element.transform[attr] = 0;
}
}
return element.transform[attr];
}else{
element.transform[attr] = val;
var transformVal = "";
for(var s in element.transform){
switch(s){
case 'scale':
case 'scaleX':
case 'scaleY':
case 'scaleZ':
transformVal +=" " + s + "("+(val/100)+")";
break;
case 'rotate':
case 'rotateX':
case 'rotateY':
case 'rotateZ':
case 'skewX':
case 'skew':
transformVal += " " + s + "("+val+"deg)";
break;
default:
transformVal += " " + s + "("+val+"px)";
}
}

                element.style.WebkitTransform =element.style.transform= transformVal;
            }
        }
    }

})();

var div = document.querySelector("#div");
cssTransform.init(div,'rotateX',50);
cssTransform.init(div,'rotateY',20);
_/
/_
获取数组中的最大值跟最小值
\*/
//方法一：先排序后取值
var arr = [5,45,1,85,2,7,6,9,10];
arr.sort(function(a,b){
return a-b;
});
var max = arr[0],min = arr[arr.length-1];
//方法二：使用了 apple
var max = Math.max.apply(null,arr),
min = Math.min.apply(null,arr);
//方法三：
var max = eval("Math.max("+ arr.toString() + " ) "),
min = eval("Math.min("+ arr.toString() + " ) ");
//方法四：假设法
var max = arr[0],min = arr[0];
for(var i = 1;i<arr.length;i++){
var cur = arr[i];
cur > max ? max = cur : null;
cur < min ? min = cur : null;
}
//获取平均数---方法一
function Average(){
//将类数组转换成数组--->方法一
// var ary = [];
// for(var i = 0;i<arguments.length;i++){
// ary[aty.length] = arguments[i];
// }
//将类数组转换成数组--->方法二
var ary = Array.prototype.slice(arguments);
//ary = [].slice.call(arguments);
arr.sort(function(a,b){
return a-b;
});
arr.shift();
arr.pop();
return (eval(arr.join("+") / arr.length).toFixed(2))
};
var res = Average(5,45,1,85,2,7,6,9,10);
//获取平均数---方法二
// function Average(){
// Array.prototype.sort.call(arguments,function(a,b){
// return a - b;
// });
// [].shift.call(arguments);
// [].pop.call(arguments);
// return (eval([].join.call(arguments,"+")) / arguments.length).toFixed(2);
// }
var utils = (function(){
var flag = "getComputedStyle" in window;
return {
//把类数组转化成数组-->var ary(自己起的名字) = utils.listToArray(类数组的名称)
listToArray : function(likeAry){
if(flag){
return Array.prototype.slice.call(likeAry);
}
var ary = [];
for(var i = 0 ; i< likeAry.length; i++){
ary[ary.length] = likeAry[i];
}
return ary;
},
//把 JSON 格式的字符串转化为 JSON 格式的对象
formatJSOM:function(jsonStr){
return "JSON" in window ? JSON.parse(jsonStr) : eval("("+jsonStr+")");
// var val = null;
// try{
// val = JSON.parse(str);
// }catch(ev){
// val = eval("("+str+")");
// }
// return val;
},
//判断数组中出现次数最多的数
findMost:function(arr){
var obj = {},MaxNum = 0,maxElem = null;
for(var i = 0 ; i < arr.length; i++){
var cur = arr[i];
obj[cur] === undefined ? obj[cur] = 1 : (obj[cur]++);
if(obj[cur] > MaxNum){
maxElem = cur;
MaxNum = obj[cur]
}
}
return '出现次数最多的元素：'+maxElem+',出现的次数：'+MaxNum;
},
//编写 一个有关浏览器盒子模型方法
// win('clientHeight')//获取
// win('scrollTop',0)//设置
win:function(attr,value){
//如果只传递 attr 没有 value 是获取，如果两个都传递了就是设置值
if(typeof value === "undefined"){
//获取值
return document.documentElement[attr] || document.body[attr];
}
document.documentElement[attr] = value;
document.body[attr] = value;
},
//获取当前元素经过浏览器计算过的样式中 attr 对应的值->curEle 当前要操作的元素对象->attr 我们要获取的属性名称
//conmsole.log(getCss(box,'height'))
//放在 reg = /^(-?\d(\.\d)?)(px|pt|rem|rm)?&/i;的前面
// try{
// val = window.getComputedStyle(curEle,null)[attr]
// }catch(e){
// val = curEle.currentStyle[attr];
// }
getCss:function(curEle,attr){
var val = null,reg = null;
if(flag){
val = window.getComputedStyle(curEle,null)[attr];
}else{
if(attr==='opacity'){
val = curEle.currentStyle["filter"];
reg = /^alpha\(opacity=(\d+(?:\.\d+)?)\)\$/i;
val = reg.test(val)?reg.exec(val)[1]/100:1;
}else{
val = curEle.currentStyle[attr];
}
}
reg = /^(-?\d(\.\d)?)(px|pt|rem|rm)?&/i;
return reg.test(val) ? parseFloat(val) : val;
},
//设置 css--->utils.setCss(box,'background',"red");
setCss:function(curEle,attr,value){

            if(attr==="float"){
                curEle["style"]["cssFloat"] = value;
                curEle["style"]["styleFloat"] = value;
                return;
            }

            if(attr==="opacity"){
                curEle["style"]["opacity"] = value;
                curEle["style"]["filter"] = "alpha(opacity="+value*100+")";
                return;
            }

            var reg = /^(width|height|top|left|bottom|right|((margin|padding)(Top|Left|Right|Bottom)?))$/;
            if(reg.test(attr)){
                if(!isNaN(value)){
                    value+="px";
                }
            }
            curEle["style"][attr] = value;
        },
        //批量设置Css样式属性
        setGroupCss:function(curEle,options){
            options = options || 0;
            if(options.toString() !=="[object Object]"){
                return;
            }
            for(var key in options){
                if(options.hasOwnProperty(key)){
                    this.setCss(curEle, key ,options[key]);
                }
            }
        },
        //实现获取、单独设置、批量设置的样式值
        //获取css--->utils.css(box,'left');
        css:function(curEle){
            var argTwo = arguments[1];
            if(typeof argTwo === "string"){
                var argThree = arguments[2];
                if(typeof argThree==="undefined"){
                    return this.getCss.apply(this,arguments);
                }
                this.setCss.apply(this,arguments);
            }
            argTwo = argTwo || 0;
            if(argTwo.toString()==="[object Object]"){
                this.setGroupCss .apply(this,arguments);
            }
        },
        //offse：等同于jQuery中的offset方法，实现获取页面中任意一个元素，距离body的偏移（包括左偏移和上偏移），不管当前元素参照物是谁
        //console.log(offset(Dom));
        //console.log(offset(Dom).left);
        offset:function(curEle){
            var totalLeft = null,totaleTop = null,par = curEle.offsetParent;
            //首先把自己本身的进行累加
            totalLeft+=curEle.offsetLeft;
            totaleTop+=curEle.offsetTop;
            //只要没有找到body我们就把父级参照物的边框和偏移也进行累加
            while(par){
                if(navigator.userAgent.indexOf("MSIE 8.0")===-1){
                    //累加父级参照物
                    totalLeft+=par.clientLeft;
                    totaleTop+=par.clientTop;
                }
                totalLeft+=par.offsetLeft;
                totaleTop+=par.offsetTop;
                par = par.offsetParent;
            }
            return {left:totalLeft,top:totaleTop};
        },
        //获取curEle下的所有元素子节点//console.log(utils.child(box,'p').length);
        child:function(curEle,tagName){
            //定义一个空数组存储
            var ary = [];
            //判断IE678-->flag
            if(/MSIE (6|7|8)/i.test(navigator.userAgent)){
                //获取传入进来的所有节点
                var nodeList = curEle.childNodes;
                //循环节点
                for(var i=0,len=nodeList.length;i<len; i++){
                    var curNode = nodeList[i];
                    //判断所有节点中的nodeType是否等于1，如果是就添加到数组中
                    if(curNode.nodeType === 1){
                        //添加到数组中
                        ary[ary.length] = curNode;
                    }
                }
                nodeList = null;
            }else{
                ary = this.listToArray(curEle.children)
            }
            //二次筛选
            if(typeof tagName === 'string'){
                for(var k=0;k<ary.length;k++){
                    var curEleNode = ary[k];
                    if(curEleNode.nodeName.toLowerCase() !== tagName.toLowerCase()){
                        ary.splice(k,1);
                        k--;
                    }
                }
            }

            return ary;
        },
        //获取上一个哥哥节点
        prev:function(curEle){
            if(flag){
                return curEle.previousElementSibling;
            }
            var pre = curEle.previousSibling;
            while(pre && pre.nodeType!==1){
                pre = pre.previousSibling;
            }
            return pre;
        },
        //获取下一个弟弟节点
        nex:function(curEle){
            if(flag){
                return curEle.nextElementSibling;
            }
            var next = curEle.nextSibling;
            while(pre && pre.nodeType!==1){
                pre = pre.nextSibling;
            }
            return nex;
        },
        //获取所有哥哥元素的节点
        prevAll:function(curEle){
            var ary = [];
            var pre = this.prev(curEle);
            while(pre){
                ary.unshift(pre);
                pre = this.prev(pre);
            }
            return ary;
        },
        //获取所有弟弟元素的节点
        nextAll:function(curEle){
            var ary = [];
            var nex = this.nex(curEle);
            while(nex){
                ary.push(nex);
                nex = this.nex(nex);
            }
            return ary;
        },
        //获取相邻的两个元素节点
        sibling:function(curEle){
            var pre = this.prev(curEle);
            var nex = this.nex(curEle);
            var ary = [];
            pre ? ary.push(pre) : null;
            nex ? ary.push(nex) : null;
            return ary;
        },
        //获取所有相邻的元素节点
        siblings:function(curEle){
            return this.prevAll(curEle).concat(this.nextAll(curEle));
        },
        //获取当前元素的索引
        indexNum:function(curEle){
            return this.prevAll(curEle).length;
        },
        //获取第一个元素的子节点
        firstchild:function(curEle){
            var chs = this.child(curEle);
            return chs.length > 0 ? chs[0] : null;
        },
        //获取最后一个元素的子节点
        firstchild:function(curEle){
            var chs = this.child(curEle);
            return chs.length > 0 ? chs[chs.length - 1] : null;
        },
        //append:向指定容器末尾追加元素
        append:function(newEle,container){
            container.appendChild(newEle);
        },
        //append:向指定容器开头追加元素
        prepend:function(newEle,container){
            var fir = this.firstchild(container);
            if(fir){
                container,insertBefore(newEle,fir);
                return;
            }
            container.appendChild(newEle);
        },
        //indertBefore：把新元素（newEle）追加到指定元素(oldEle)的前面
        insertBefore:function(newEle,oldEle){
            oldEle.parentNode.insertBefore(newEle,oldEle);
        },
        //indertAfter：把新元素（newEle）追加到指定元素(oldEle)的后面-->相当于追加到oldEle弟弟元素的前面，如果弟弟不存在，也就是当前元素已经是最后一个了，我们把新的额元素放在最末尾即可
        indertAfter:function(newEle,oldEle){
            var nexx = this.nex(oldEle);
            if(nexx){
                oldEle.parentNode.insertBefore(newEle,oldEle);
                return;
            }
            oldEle.parentNode.appendChild(newEle,oldEle);
        },
        //hasClass:判断是否存在某一个类名:存在就为ture,不存在就为false
        //console.log(hasClass(box,"border"))
        hasClass:function(curEle,className){
            var reg = new RegExp("(^| +)"+className+"( +|$)");
            return reg.test(curEle.className);
        },
        //addClass:增加样式类名addClass(box,"color border bg")
        addClass:function(curEle,className){
            //为了防止className传递进来的值包含多个样式名，我们把传递进来的字符串按照一到多个空格拆分为数组中的每一项
            //split是拆分为数组
            //replace(/(^ +| +$)/g, "")清楚首尾空格
            var ary = className.replace(/(^ +| +$)/g, "").split(/ +/g);
            for(var i=0,len=ary.length;i<len;i++){
                var curName = ary[i];
                if(!this.hasClass(curEle,curName)){
                    curEle.className += " "+curName;
                }
            }

        },
        //removeClass:删除样式类名:removeClass(box,"color border bg")
        removeClass:function(curEle,className){
            var ary = className.replace(/(^ +| +$)/g, "").split(/ +/g);
            for(var i=0,len=ary.length;i<len;i++){
                var curName = ary[i];
                if(this.hasClass(curEle,curName)){
                    var reg = new RegExp("(^| +)"+curName+"( +|$)","g");
                    curEle.className = curEle.className.replace(reg," ");
                }
            }
        },
        //getElementsByClassName兼容处理-->className:要获取样式类名-->context元素上下文(范围)，如果不传默认是document
        //console.log(utils.getElementsByClass("box2",box));
        getElementsByClass:function(strClass,context){
            context = context || document;
            if(flag){
                return this.listToArray(context.getElementsByClassName(strClass));
            }
            //把传递进来的样式类名首尾空格先去掉，然后再按照空格把它里面的每一项拆分成数组
            var classNameAry = strClass .replace(/(^ +| +$)/g, "").split(/ +/g);
            var ary = [];
            //获取指定上下文中的所有元素标签，循环这些标签，获取每一个标签的className样式类名的字符串
            var nodeList = context.getElementsByTagName('*');
            for(var i=0,len=nodeList.length;i<len;i++){
                var curNode = nodeList[i];
                var isOk = true;
                for(var k = 0; k < classNameAry.length;k++){
                    var reg = new RegExp("(^| +)"+classNameAry[k]+"( +|$)");
                    if(!reg.test(curNode.className)){
                        isOk =false;
                        break;
                    }
                }
                if(isOk){
                    ary[ary.length] = curNode;
                }
            }
            return ary;
        },
        //获取与设置transform的值，PS->只有通过这里设置的值才可以通过这里去获取var div = document.querySelector("#div");cssTransform.init(div,'rotateX',50);cssTransform.init(div,'rotateY',20);
        cssTransform:function(element,attr,val){
            if(!element.transform){
                element.transform = {};
            }
            if(typeof val == 'undefined'){
                if(!element.transform[attr]){
                    switch(attr){
                        case 'scale':
                        case 'scaleX':
                        case 'scaleY':
                        case 'scaleZ':
                            element.transform[attr] = 100;
                        break;
                        default:
                            element.transform[attr] = 0;
                    }
                }
                return element.transform[attr];
            }else{
                element.transform[attr] = val;
                var transformVal = "";
                for(var s in element.transform){
                    switch(s){
                        case 'scale':
                        case 'scaleX':
                        case 'scaleY':
                        case 'scaleZ':
                            transformVal +=" " + s + "("+(val/100)+")";
                            break;
                        case 'rotate':
                        case 'rotateX':
                        case 'rotateY':
                        case 'rotateZ':
                        case 'skewX':
                        case 'skew':
                            transformVal += " " + s  + "("+val+"deg)";
                            break;
                        default:
                            transformVal += " " + s + "("+val+"px)";
                    }
                }

                element.style.WebkitTransform =element.style.transform= transformVal;
            }
        }
    }

})();

//var str = "2015-6-10 14:53:00";
//console.log(str.myFormatTime("{0}年{1}月{2}日"))
String.prototype.myFormatTime = function(){
var reg = /^(\d{4})[-/.:](\d{1,2})[-/.:](\d{1,2}) +(\d{1,2}):(\d{1,2}):(\d{1,2})\$/g
var ary = [];
this.replace(reg,function(){
ary = ([].slice.call(arguments)).slice(1, 7);
});
var format = arguments[0] || "{0}年{1}月{2}日 {3}:{4}:{5}";
return format.replace(/{(\d+)}/g,function(){
val = ary[arguments[1]];
return val.length === 1 ? "0" + val : val;
});
};
//二维数组根据名字或者年龄去排序
/\*
var ary = [
{name:'阿里',age:18},
{name:'吃饭',age:52},
{name:'鄙视',age:25}
];

    这是根据年龄排序
    ary.sort(function(a,b){
        return parseFloat(a.age) - parseFloat(b.age);
    });
    这是根据名字排序
    ary.sort(function(a,b){
        return a.name.localeCompare(b.name);
    });
    console.log(ary);

\*/
//forEach-->它的作用是使用定义的函数处理数组中的每个元素,实际上 forEach 方法接收第二个参数，如果传入这个参数，则回调函数中的 this 就指向这个参数值，如果没有传入，则 this 指向全局变量 window
Array.prototype.forEach = Array.prototype.forEach || function(fn,context){
for(var i = 0;i<this.length;i++){
if(typeof fn === "function" && Object.prototype.hasOwnProperty.call(this,k)){
fn.call(context,this[i],i,this);
}
}
};
//map 方法会将数组中每个元素做处理得到新的元素，然后返回这些新的元素组成的数组。其回调函数中接收的参数和 forEach 一样
Array.prototype.map = Array.prototype.map || function(fn,context){
var arr = [];
if(typeof fn === "function"){
for(var k = 0;k<this.length;k++){
arr.push(fn.call(context,this[k],k,this));
}
}
return arr;
};
//filter 顾名思义，过滤数组中满足条件的数值，得到一个新的数组。在 filter 的回调函数中需要返回 true 或者 false，true 代表满足条件，通过筛选；false 代表不满足条件，不通过筛选
Array.prototype.filter = Array.prototype.filter || function(fn,context){
var arr = [];
if(typeof fn === "function"){
for(var j = 0;j<this.length;j++){
fn.call(context, thi[j], j, this) && arr.push(this[j])
}
}
return arr;
};
//中国标准真实姓名 2-4 位汉字
//var reg = /^[\u4e00-\u9fa5]{2,4}$/;
//手机号码的认证
//var reg = /^1[3|5|8][0-9]\d{4,8}$/; 1 5 21827 2362

//利用正则修改字符串的数字改成中文大写
// var arr = ["零","壹","贰","叁","肆","伍","陆","柒","捌","玖"];
// str = str.replace(/\d/g,function(){
// return arr[arguments[0]];
// });
/\*
//获取字符串出现次数最多的次数
var str = "kjkdjghkjdfvbnmbeuih";
//获取每一个字符出现的字数
var obj = {};
str.replace(/[a-z]/gi,function(){
var val = arguments[0];
obj[val] >= 1 ? obj[val] += 1 : obj[val] = 1;
});
//获取最多出现的次数
var maxNum = 0;
for(var key in obj){
obj[key] > maxNum ? maxNum = obj[key] : null;
}
//把所有符合出现 maxNum 次数的都获取到
var ary = [];
for(var key in obj){
obj[key] === maxNum ? ary.push(key)　: null;
}
console.log('整个字符串中出现次数最多的字符是：'+ary.toString() + "出现了"+maxNum + '次')

_/
/_
//将时间转化成自己想要的格式
var str = "2017-7-4 16:33:0",
regs = /^(\d{4})[-/](\d{1,2})[-/](\d{1,2}) +(\d{1,2}):(\d{1,2}):(\d{1,2})\$/g,
ary = [];

str.replace(regs,function(){
ary = [].slice.call(arguments);
ary = ary.slice(1, 7);
});
var resStr = "{0}年{1}月{2}日 {3}时{4}分{5}秒",reg = /{(\d+)}/g;
resStr = resStr.replace(reg,function(){
var nul = arguments[1],val = ary[nul];
val.length === 1 ? val = "0" + val : void 0;
return val;
});
console.log(resStr);

\*/

//判断数据类型
//var ary = [];console.log(Object.prototype.toString.call(ary)==="[object Array]");

// 判断随机数
function getRandom(n,m) {
n = Number(n);
m = Number(m);
if(isNaN(n) || isNaN(m)){
return Math.random();
}
if(n>m){
var temp = n;
n = m;
m = temp;
}
return Math.round(Math.random()\*(m - n) + n);
}
