<template>
    <div class="text-center">
        home---------
        <br />
        <img :src="require('../../assets/img/icon-thumb.png')" />
        <div id="income" :style="{width:'100%',height:'236px'}"></div>
    </div>
</template>

<script>
import {EleResize} from '../../config/esresize';
import { login } from '../../service/getData';

export default {
    data () {
        return {
            incomeDefault: {
                x: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
                y: [
                    {
                        name: '总收入',
                        data: [72.98, 73.32, 76.04, 76.01, 74.30, 75.27, 75.52, 74.82, 77.85, 62.80, 75.83, 67.27],
                        width: 8
                    },
                    {
                        name: '收入1',
                        data: [30.79, 39.88, 41.42, 40.15, 38.48, 34.86, 39.38, 37.46, 42.65, 35.87, 34.36, 37.03],
                        width: 8
                    },
                    {
                        name: '收入2',
                        data: [31.68, 25.11, 26.80, 30.17, 24.19, 31.02, 26.08, 28.09, 26.69, 19.71, 31.63, 22.55],
                        width: 8
                    }
                ]
            }
        }
    },
    mounted () {
        this.setBars('income', this.incomeDefault.x, this.incomeDefault.y, '生命周期统计', [['#FED573', '#F67265'],['#CC7BE4', '#8D5AFA'],['#27FFFB', '#16AEFE']], 0, '（亿元）');
    },
    methods: {
        setBars(id, xData, yData, title, colors, rotate, unit) {
            let chat = this.$echarts.init(document.getElementById(id));
            let resizeDiv = document.getElementById(id);
            this.setBarsOption(chat, xData, yData, title, colors, rotate, unit);
            EleResize.on(resizeDiv, () => {
                chat.resize();
            });
        },
        setBarsOption(chat, xData, yData, title, colors, rotate, unit) {
            let datas = [],
                names = [];
            yData.forEach((value, index) => {
                let obj = {};
                obj.name = value.name;
                obj.type = 'bar';
                obj.barWidth = value.width;
                obj.data = [];
                //obj.stack = '总数';
                value.data.forEach((val, i) => {
                    if (i < 8) {
                        obj.data.push({
                            value: val,
                            itemStyle: {
                                normal: {
                                    color:new this.$echarts.graphic.LinearGradient(
                                        0, 0, 0, 1,//渐变色在下面修改，这里是透明度
                                        [{
                                            offset: 0,
                                            color: colors[index][0]
                                        }, {
                                            offset: 1,
                                            color: colors[index][1]
                                        }]
                                    )
                                }
                            }
                        });
                    } else {
                        obj.data.push({
                            value: val,
                            itemStyle: {
                                normal: {
                                    color: 'transparent',
                                    barBorderColor:new this.$echarts.graphic.LinearGradient(
                                        0, 0, 0, 1,//渐变色在下面修改，这里是透明度
                                        [{
                                            offset: 0,
                                            color: colors[index][0]
                                        }, {
                                            offset: 1,
                                            color: colors[index][1]
                                        }]
                                    ),
                                    barBorderWidth: 2,
                                    barBorderRadius: 0,
                                    borderType: 'dashed'
                                }
                            }
                        });
                    }
                });
                names.push(value.name);
                datas.push(obj);
                // console.log(datas[0].data)
            });
            chat.setOption({
                color: [colors[0][0],colors[1][0],colors[2][0]],
                grid: {
                    top: 35,
                    left: 60,
                    right: 33,
                    bottom: 60
                },
                legend: {
                    data: names,
                    bottom: 15,
                    itemWidth: 7,
                    itemHeight: 7,
                    //padding: [0,5,0,5],
                    top: 'bottom',
                    left: 'center',
                    textStyle: {
                        color: '#ddd',
                        fontSize: 12,
                        lineHeight: 35,
                        padding: 0
                    }
                },
                tooltip: {
                    trigger: 'axis',
                    axisPointer: {
                        type: 'shadow'
                    }
                },
                xAxis: {
                    data: xData,
                    axisLabel: {
                        interval: 0,
                        color: '#ddd',
                        rotate: rotate
                    },
                    axisLine: {
                        lineStyle: {
                            color: '#979797'
                        }
                    },
                    axisTick: {
                        show: false
                    }
                },
                yAxis: {
                    name: unit,
                    nameTextStyle: {
                        color: '#ddd',
                        fontSize: 12,
                        padding: [0, 0, 0, -10],
                        align: 'right'
                    },
                    axisLabel: {
                        color: '#ddd'
                    },
                    axisLine: {
                        lineStyle: {
                            color: '#979797'
                        }
                    },
                    axisTick: {
                        show: false
                    },
                    splitLine: {
                        show: false,
                    }
                },
                series: datas
            });
        }
    }
}
</script>

<style lang="scss" scoped>

</style>