<template>
    <div id="cartesian_heart_box">
        <canvas id="cartesian_hear"></canvas>
    </div>
</template>
<script>
export default {
    data () {
        return {
            radians: '', // 弧度
            radianDecrement: '', // 弧度增量
            time: 2, // 绘制每个点之间的时间间隔
            interval: '', // 定时器
            number: 1314, // 绘制点的数量
            startRadian: Math.PI,
            ctx: '', // canvas上下文
            index: '',
            radius: 4
        }
    },
    comments: {},
    computed: {},
    watch: {},
    methods: {
        lisner () {
            // 判断点是否画完
            if (this.index >= this.number) {
                // 当所有的点画完即进行初始化
                this.index = 0
                let box = document.getElementById("cartesian_heart_box")
                // 清空画布
                box.empty()
                // 重新建立画布
                box.append('<canvas id="cartesian_hear"></canvas>')
                // 开始进行内容绘制
                this.startAnimation()
            }
        },
        // 设置画布
        startAnimation () {
            this.ctx = document.getElementById("cartesian_hear").getContext("2d")
            // 让画布撑满整个屏幕，-20是滚动条的位置，需留出。如滚动条出现则会挤压画布
            this.ctx.width = document.documentElement.clientHeight - 20
            this.ctx.heigh = document.documentElement.clientWidth - 20
            this.drawHeart()
        },
        // 开始绘制
        drawHeart () {
            this.ctx.strokeStyle = "red";
            this.ctx.lineWidth = 1 // 设置线的宽度
            this.radian = this.startRadian // 弧度设为初始弧度
            this.radianDecrement = Math.PI / this.number * 2
            this.ctx.moveTo(this.getX(this.radian), this.getY(this.radian)) // 移动到初始点
            this.index = 0
            this.interval = setInterval(() => {
                this.radian += this.radianDecrement
                this.ctx.lineTo(this.getX(this.radian), this.getY(this.radian)) // 在旧点和新点之间连线
                this.index++
                this.ctx.stroke() // 画线
                if (this.index >= this.number) {
                    clearInterval(this.interval)
                }
            }, this.time)
        },
        // 由弧度得到 X 坐标
        getX (t) {
            return 100 + this.radius * (16 * Math.pow(Math.sin(t), 3))
        },
        // 由弧度得到 Y 坐标
        getY (t) {
            return 50 - this.radius * (13 * Math.cos(t) - 5 * Math.cos(2 * t) - 2 * Math.cos(3 * t) - Math.cos(4 * t))
        }
    },
    beforeCreate () {
    },
    created () {
    },
    beforeMount () {
    },
    mounted () {
        this.startAnimation()
        // 监听事件，监听是否画所有的点
        window.setInterval(this.lisner(), 1)
    },
    beforeUpdate () {
    },
    updated () {
    },
    beforeDestroy () {
    },
    destroyed () {
    }
}
</script>

<style lang="scss" scoped>
    #cartesian_heart_box {
        @extend .cover;
        text-align: center;
        display: flex;
        justify-content: center;
        align-items: center;

        #cartesian_heart {
            height: 500px;
            width: auto;
        }
    }
</style>
