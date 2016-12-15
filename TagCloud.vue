<style>
    .tagBall {
        width: 250px;
        height: 250px;
        margin: 10px auto;
        position: relative;
    }

    .tag {
        display: block;
        position: absolute;
        left: 0px;
        top: 0px;
        color: #000;
        text-decoration: none;
        font-size: 15px;
        font-family: "微软雅黑";
        font-weight: bold;
    }

    .tag:hover {
        border: 1px solid #666;
    }
</style>

<template>
    <div @mousemove="over"  class="tagBall">
        <a class="tag" href="#">Tag</a>
        <a class="tag" href="#">3DTag</a>
        <a class="tag" href="#">Tag</a>
        <a class="tag" href="#">3DTag</a>
        <a class="tag" href="#">Tag</a>
        <a class="tag" href="#">3DTag</a>
        <a class="tag" href="#">Tag</a>
        <a class="tag" href="#">3DTag</a>
        <a class="tag" href="#">Tag</a>
        <a class="tag" href="#">3DTag</a>
        <a class="tag" href="#">Tag</a>
        <a class="tag" href="#">3DTag</a>
        <a class="tag" href="#">Tag</a>
        <a class="tag" href="#">3DTag</a>
        <a class="tag" href="#">Tag</a>
        <a class="tag" href="#">3DTag</a>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                angleX: Math.PI / 500,
                angleY: Math.PI / 500,
                tags: []
            }
        },
        methods: {
            over: function () {
                this.EX = this.$el.getBoundingClientRect().left + document.documentElement.scrollLeft;
                this.EY = this.$el.getBoundingClientRect().top + document.documentElement.scrollTop;

                var x = event.clientX - this.EX -this.CX;
                var y = event.clientY - this.EY - this.CY;

                this.angleY = x * 0.0001;
                this.angleX = y * 0.0001;
            },
            rotateX(){
                var cos = Math.cos(this.angleX);
                var sin = Math.sin(this.angleX);
                this.tags.forEach(function (v, i, a) {
                    var y1 = v.y * cos - v.z * sin;
                    var z1 = v.z * cos + v.y * sin;
                    v.y = y1;
                    v.z = z1;
                })
            },
            rotateY() {
                var cos = Math.cos(this.angleY);
                var sin = Math.sin(this.angleY);

                this.tags.forEach(function (v, i, a) {
                    var x1 = v.x * cos - v.z * sin;
                    var z1 = v.z * cos + v.x * sin;
                    v.x = x1;
                    v.z = z1;
                })
            }
        },
        mounted: function () {
            let paper = this.$el,
                    tagEle = paper.querySelectorAll(".tag");

            let RADIUS = this.RADIUS = 100,
                    fallLength = 500;
                    this.CX = paper.offsetWidth / 2;
                    this.CY = paper.offsetHeight / 2;

            let that = this;
            let tag = class {
                constructor(ele, x, y, z) {
                    this.ele = ele;
                    this.x = x;
                    this.y = y;
                    this.z = z;
                }

                move() {
                    var scale = fallLength / (fallLength - this.z);
                    var alpha = (this.z + RADIUS) / (2 * RADIUS);
                    this.ele.style.fontSize = 15 * scale + "px";
                    this.ele.style.opacity = alpha + 0.5;
                    this.ele.style.filter = "alpha(opacity = " + (alpha + 0.5) * 100 + ")";
                    this.ele.style.zIndex = parseInt(scale * 100);
                    this.ele.style.left = this.x + that.CX - this.ele.offsetWidth / 2 + "px";
                    this.ele.style.top = this.y + that.CY - this.ele.offsetHeight / 2 + "px";
                }
            }


            for (var i = 0; i < tagEle.length; i++) {
                var a, b;
                var k = -1 + (2 * (i + 1) - 1) / tagEle.length;
                var a = Math.acos(k);
                var b = a * Math.sqrt(tagEle.length * Math.PI);
                var x = RADIUS * Math.sin(a) * Math.cos(b);
                var y = RADIUS * Math.sin(a) * Math.sin(b);
                var z = RADIUS * Math.cos(a);
                var t = new tag(tagEle[i], x, y, z);
                tagEle[i].style.color = "rgb(" + parseInt(Math.random() * 255) + "," + parseInt(Math.random() * 255) + "," + parseInt(Math.random() * 255) + ")";
                this.tags.push(t);
                t.move();
            }

            setInterval(function () {
                that.rotateX();
                that.rotateY();
                that.tags.forEach(function (v) {
                    v.move();
                })
            }, 17)
        }
    }

</script>
