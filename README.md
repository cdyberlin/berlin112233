# berlin112233
/**
 * Created by Administrator on 2017/5/16.
 */
window.onload = function () {
    searchEffect()//头部搜索块
    timeBack()//倒计时模块
    bannerEffect()//轮播图模块
}//终极大括号

//头部搜索块
function searchEffect() {
    //1.获取当前banner的高度
    var banner = document.querySelector(".jd_banner");
    var bannerHeight = banner.offsetHeight;
    var search = document.querySelector(".jd_search")
    //2.获取当前屏幕滚动时,banner滚动出屏幕的距离
    window.onscroll = function () { //屏幕滚动时间
        var offsetTop = document.body.scrollTop; //获取屏幕滚动到上面的高度
        var opacity = 0; //设置默认透明度为0
        /*3.判断，如果当banner还没有完全滚出屏幕，也就是滚出屏幕的高度小于广告的高度时，才有必要计算透明度和设置透明度*/
        if (offsetTop < bannerHeight) {
            //4.计算比例值，获取透明度，设置背景颜色的样式
            opacity = offsetTop / bannerHeight;//滚出屏幕的高度除以当前banner的高度 的值赋给opacity
            //设置样式
            search.style.backgroundColor = "rgba(233, 35, 34, " + opacity + ")"
        }
    }
}
//倒计时模块
function timeBack() {
    /*1.获取用于展示时间的span*/
    var spans = document.querySelector(".jd_sk_time").querySelectorAll("span");
    /*2.设置初始的倒计时时间 时间都以秒为单位的 */
    var totalTime = 9999;//  总时间      1小时乘以60分钟乘以60秒钟  1 * 60 * 60
    /*3.开启定时器*/
    var timerID = setInterval(function () {
        totalTime--;// 因为是倒计时  所以要--
        /*判断倒计时时间是否已经完成*/
        if (totalTime < 0) {  //如果时间小于0的话
            clearInterval(timerID);  //就清除计时器
            return;                 //然后return  后面的就不执行了
        }
        /*得到剩余时间中的时 分 秒*/
        var hour = Math.floor(totalTime / 3600);  //用总时间除以3600s(1小时的秒数) 得到小时数
        var minute = Math.floor(totalTime % 3600 / 60); //用总时间与3600取余 （也就是去掉整小时）除以60秒 得到分钟数
        var second = Math.floor(totalTime % 60);
        /*赋值:将时间填充到span中*/
        spans[0].innerHTML = Math.floor(hour / 10);
        //第一位取地板数 如果当前时间大于10  就用当前数字除以10 得到的整数向下取整  如果小于10的话 得到的是1以下的小数 就是0了
        spans[1].innerHTML = Math.floor(hour % 10);
        //第二位取余数 如果当前时间大于10  就用当前数字除以10  余数就是剩下的小时 如果小于10的话 得到的就是当前数字 因为这个数字 小于10 所以第一位的时候肯定是0
        spans[3].innerHTML = Math.floor(minute / 10);
        spans[4].innerHTML = Math.floor(minute % 10);

        spans[6].innerHTML = Math.floor(second / 10);
        spans[7].innerHTML = Math.floor(second % 10);
    }, 1000);
}
//轮播图
function bannerEffect() {
    /*1.设置修改轮播图的页面结构*/
    /*a.在开始位置添加原始的最后一张图片*/
    /*b.在结束位置添加原始的第一张图片*/
    /*1.1.获取轮播图结构*/
    var banner = document.querySelector(".jd_banner");
    /*1.2.获取图片容器*/
    var imgBox = document.querySelector("ul:first-of-type");//为了区分清楚 获取第一个ul
    /*1.3.获取原始的第一张和最后一张图片*/
    var first = imgBox.querySelector("li:first-of-type");
    var last = imgBox.querySelector("li:last-of-type");
    /*1.4.在首尾插入两张图片*/
    imgBox.appendChild(first.cloneNode(true));
    imgBox.insertBefore(last.cloneNode(true), imgBox.firstChild)//insertBefore  插入到某个节点（需要插入的元素，位置）
    /*2.设置对应样式*/
    var lis = imgBox.querySelectorAll("li");    //获取所有li元素
    var count = lis.length;//获取li元素的数量

    //获取banner的宽度
    var bannerWidth = banner.offsetWidth;
    imgBox.style.width = count * bannerWidth + "px"; //设置图片盒子的宽度  banner的宽度乘以图片的数量
    for (var i = 0; i < lis.length; i++) {  //循环遍历获取每一个li
        /*2.设置每一个li（图片）元素的宽度  每一张图片的宽度就是等于图片盒子的宽度*/
        lis[i].style.Width = bannerWidth + "px";
    }
    /*定义图片索引*/
    var index = 1;   //因为第一张图片前多了一张  已经有了一张默认偏移 所以是1
    /*3.设置默认偏移*/
    imgBox.style.left = -bannerWidth + "px";

    /*4.当屏幕变化的时候，重新计算宽度*/
    window.onresize = function () {
        bannerWidth = banner.offsetWidth; //！！！不加var 获取banner的宽度 覆盖全局宽度值
        imgBox.style.width = count * bannerWidth + "px"; //设置图片盒子的宽度  banner的宽度乘以图片的数量
        for (var i = 0; i < lis.length; i++) {
            lis[i].style.Width = bannerWidth + "px";
            /*2.设置每一个li（图片）元素的宽度  每一张图片的宽度就是等于图片盒子的宽度*/
        }
        imgBox.style.left = -index * bannerWidth + "px";
        /*重新设置默认偏移*/
    }

    /*5.实现自动轮播*/
    var timerId
    var startTime = function () {
        timerId = setInterval(function () {
            index++;//变化索引  上面设置过index=1的全局变量
            imgBox.style.left = (-index * bannerWidth) + "px";//设置偏移
            imgBox.style.transition = "left 0.5s";//设置过渡效果
            setTimeout(function () { //setTimeout：设置延时代码  为了两个事件不同时进行 判断之前设置一个延时 时间与过渡效果时间一样则可以保持平稳过渡
                if (index === count - 1) {    //count就是lis.length  如果索引到了最后一张图片的话 自动把索引变为1 过渡停止 重新设置偏移  重新循环排他
                    index = 1;
                    imgBox.style.transition = "none";
                    //如果一个元素的某个属性之前添加过过渡效果，那么过渡属性会一直存在，如果不想要，则需要清除过渡效果
                    imgBox.style.left = (-index * bannerWidth) + "px";//设置偏移

                    for (var i = 0; i < banInd.length; i++) {
                        banInd[i].className = "";
                    }
                    banInd[index - 1].className = "active";       //每次定时器执行完毕的时候瞬间清空过渡 同时赋值给index=1 重新循环

                }
            }, 500)
            // 点标记循环
            var jd_bannerIndicator = document.querySelector(".jd_bannerIndicator");
            var banInd = jd_bannerIndicator.querySelectorAll("li");

            for (var i = 0; i < banInd.length; i++) {
                banInd[i].className = "";
            }
            banInd[index - 1].className = "active";
        }, 2000);
    }
    startTime();

    //6.手动轮播
    var startX, moveX, distanceX;
    //标记当前的过渡效果是否已经执行完毕
    var isEnd = true;


    //为图片添加触摸事件  
    imgBox.addEventListener("touchstart", function (e) {  //为图片添加触摸事件  touch事件触发  必须保证元素有具体的宽高 如果宽高有一个为0 则不会进行触发
        clearInterval(timerId)
        /*因为要手指按住的时候  停止轮播 所以首先要清除定时器*/
        startX = e.targetTouches[0].clientX;      //获取当前手指的起始坐标
    })


    //为图片添加滑动过程事件  
    imgBox.addEventListener("touchmove", function (e) {  //滑动过程
        if (isEnd == true) {        //如果当前过渡效果已经执行完毕的话  就可以为图片添加滑动过程事件
            moveX = e.targetTouches[0].clientX;         //获取滑动后手指的坐标
            distanceX = moveX - startX;                 //计算坐标差异
            imgBox.style.transition = "none"
            /*重大细节  本次的滑动操作应该基于之前轮播图已经偏移的距离
             * 所以最终偏移的距离应该是已经偏移的距离加上我们计算出的距离
             * 为了保证效果正常 如果要以非过渡的形式来操作的话 一定要先清除过渡效果*/
            imgBox.style.left = (-index * bannerWidth + distanceX ) + "px";   //设置偏移   left参照的是最原始的坐标
        }
    })


    //为图片添加触摸结束事件  
    imgBox.addEventListener("touchend", function (e) {
        /*获取当前滑动的距离，判断距离是否超出指定的范围 */
        isEnd = false;
        if (Math.abs(distanceX) > 100) {//如果滑动超过100px 不管是左还是右移动 都要进行判定来决定翻不翻页   所以首先取绝对值

            /*判断滑动方向  然后不取绝对值*/
            if (distanceX > 0) {  //如果滑动后坐标大于0 也就是再往左边移动  进入上一张 对应索引就要减少
                index--;
            } else {
                index++;         //如果滑动后坐标小于0 也就是再往右边移动 进入下一张 对应索引就要增加
            }
            //翻页：如果滑动距离超过100了 index就产生了变化  所以left值就会变化 图片就会根据index的增减来滑动
            imgBox.style.transition = "left 0.5s";
            imgBox.style.left = -index * bannerWidth + "px";
        } else if (Math.abs(distanceX) > 0) {      //得保证用户确实京西过滑动操作
            /*回弹*/     // 如果index值没有产生变化  所以left值不会变化 自然不会翻页 还是弹回当前图片
            imgBox.style.transition = "left 0.5s";
            imgBox.style.left = -index * bannerWidth + "px";
        }
        startX = 0;
        moveX = 0;
        distanceX = 0;


    });
    /* webkitTransitionEnd:可以监听当前元素的过渡效果执行完毕，当一个元素的过渡效果执行完毕的时候，会触发这个事件*/
    imgBox.addEventListener("webkitTransitionEnd", function () {
        /*如果到了最后一张，回到第一张 count-1*/
        /*如果到了第一张，回到索引count-2*/
        if (index === count - 1) {      //如果到了最后一张（因为最后一张就是克隆的索引为1的第一张图片） 所以index直接跳到第一张
            index = 1
            /*清除过渡*/
            imgBox.style.transition = "none";
            /*设置偏移*/
            imgBox.style.left = -index * bannerWidth + "px";
        } else if (index === 0) {       //如果index等于0（因为第一张就是第一张 索引为0的是被克隆的最后一张）。然后为索引最后一张是克隆的第一张图片   所以直接跳到倒数第二张   因
            index = count - 2
            /*清除过渡*/
            imgBox.style.transition = "none";
            /*设置偏移*/
            imgBox.style.left = -index * bannerWidth + "px";
        }
        setTimeout(function () {
            isEnd = true;
            clearInterval(timerId)
            //重新开启定时器
            startTime();
        }, 500)
    });
}


