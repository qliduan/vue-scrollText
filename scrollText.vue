<template>
    <div class="notice-wrap" :id="compId">
        <ul class="notice-list">
            <li v-for="item in dataList">
                <a href="javascript:;">
                    恭喜 {{item.user}} 赢得了{{item.goods}}
                </a>
            </li>
            <li v-if="dataList && dataList.length > 1">
                <a href="javascript:;">
                    恭喜 {{dataList[0].user}} 赢得了{{dataList[0].goods}}
                </a>
            </li>
        </ul>
    </div>
</template>
<script>
/*eslint-disable no-new*/

export default {
    props: ['dataList','itemHeight','unit','time'],
    data() {
        return {
            compId:''  //组件id，用于样式覆盖
        }
    },
    watch:{
        dataList:function(newVal, oldVal) {
            this.dataChange(newVal);
        }
    },
    methods: {
        dataChange:function(data) {
            if(data && data.length && data.length > 1) {
                this.compId = 'compId_' + this.time + data.length;
                this.init('#' + this.compId, this.dataList.length, Number(this.time), 0.1);
            } else {
                this.compId = '';
            }
        },
        init:function(selector, length, gapTime, time) {
            var unit = this.unit || 'px';
            var nod = document.createElement('style'),
                str = generateStyle(selector + ' ul', this.itemHeight, unit, length, gapTime, time);
            str += this.initHeight(selector, this.itemHeight + this.unit)
            nod.type = 'text/css';
            nod.innerHTML = str;
            document.getElementsByTagName('head')[0].appendChild(nod);
        },
        //初始化高度
        initHeight:function(selector, height) {
            var style = (selector + '{height:' + height + ';}'  
            + selector + ' ul {line-height:' + height + ';}' 
            + selector + ' ul li {height:' + height + ';}')
            return style;
        }
    },
    created:function() {
        this.dataChange(this.dataList)
    }
}
/**
    function generateStyle   生成动画所需的样式
    @params
        selector  动画作用的dom的选择器
        height    每次滚动高度，数值
        unit      滚动高度的单位：px、rem、em
        length    数据的长度
        gapTime   轮播间隔时间，单位s
        time      切换时间，单位s
*/

function generateStyle(selector, height, unit, length, gapTime, time) {
    if (length < 2) return;
    var gapTime = gapTime || 3;
    var time = time || 0.1;
    //css3动画总时长
    var durationTime = length * (gapTime + time);
    var curGap = time / (gapTime + time) * 100;
    var eachTime = 100 / length;
    var animationName = 'name_' + length + '_' + gapTime + '_' + time;
    //animation-name不能带小数点
    animationName = animationName.replace('.', '');
    var style = 'keyframes ' + animationName + ' {' + genTransform(0, 0, '-webkit-');
    var step;
    for (var i = 1; i <= length; i++) {
        step = eachTime * i;
        style = style + genTransform(step - curGap, (-(i - 1) * height) + unit, '-webkit-') + genTransform(step, (-i * height) + unit, '-webkit-')
    }
    style += '}'
    style = '@-webkit-' + style + '@' + style;
    //将animation动画绑定到选择器上
    style += selector + ' {-webkit-animation: ' + animationName + ' ' + durationTime + 's linear 0s infinite;' +
        'animation: ' + animationName + ' ' + durationTime + 's linear 0s infinite;}';
    return style;
}

function genTransform(percent, translateY, prefix) {
    var transformStyle = percent + '% {' + prefix + 'transform:translate(0,' + translateY + ');' +
        'transform:translate(0,' + translateY + ');}'
    return transformStyle;
}



</script>
<style>
    .notice-list {
        padding: 0 .15rem;
        line-height: .2rem;
        text-align: left;
        overflow: hidden;
    }
    
    li {
        list-style: none;
        height: .2rem;
    }
    
    .notice-list a {
        color: #333;
        text-decoration: none;
    }
    
    .notice-wrap {
        height: .2rem;
        text-align: center;
        overflow: hidden;
        padding: .025rem 0;
    }
</style>