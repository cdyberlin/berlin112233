
    <style>
        * {
            padding: 0;
            margin: 0;
        }

        div {
            width: 100px;
            height: 100px;
            background: linear-gradient(to right, #e92322, blue);
        }
    </style>
</head>
<body>
<div>1</div>
<div>2</div>
<script>
    var div = document.querySelector("div");
    var startX, startY, moveX, moveY,distanceX,distanceY;
    document.addEventListener("touchstart", function (e) {
//        console.log(e.targetTouches[0].clientX + " " + e.targetTouches[0].clientY);
        startX = e.targetTouches[0].clientX;
        startY = e.targetTouches[0].clientY;
    });
    document.addEventListener("touchmove", function (e) {
        moveX = e.targetTouches[0].clientX;
        moveY = e.targetTouches[0].clientY;
        distanceX = moveX - startX;
        distanceY = moveY - startY;
        e.target.style.transform = "translate(" + distanceX + "px," + distanceY + "px)"
    })
    document.addEventListener("touchend", function (e) {
    });
</script>
</body>
</html>
