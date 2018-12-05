<template>
    <section class="chart-container">
        <el-row>
            <el-col :span="12">
                <div id="chartColumn" style="width:100%; height:400px;"></div>
            </el-col>

        </el-row>
    </section>
</template>

<script>
    import echarts from 'echarts'
    import {reportOccupancy} from '../../api/api';

    export default {
        data() {
            return {
                chartColumn: null,
                dataList: {
                    D1: "",
                    D2: "",
                    D3: "",
                    D4: "",
                    D5: "",
                    D6: "",
                    D7: ""
                }
            }
        },

        methods: {
            drawColumnChart() {
                let that = this
                this.chartColumn = echarts.init(document.getElementById('chartColumn'));
                console.log(this.dataList.D7)
                this.chartColumn.setOption({
                    title: {text: '入住率分析'},
                    tooltip: {
                        formatter: '{c}%'
                    },
                    xAxis: {
                        data: ["第一天", "第二天", "第三天", "第四天", "第五天", "第六天", "第七天"]
                    },
                    yAxis: {
                        max:100,
                        scale:false,
                        splitNumber:10,
                        type: 'value',
                        axisLabel: {
                            show: true,
                            interval: 'auto',
                            formatter: '{value} %'
                        },
                        show: true
                    },
                    series: [{
                        name: '',
                        type: 'bar',
                        data: [this.dataList.D1, this.dataList.D2, this.dataList.D3, this.dataList.D4, this.dataList.D5, this.dataList.D6, this.dataList.D7],
                        label: {
                            normal: {
                                show: true,
                                position: 'top',
                                formatter: '{c}%'
                            }
                        }
                    }]
                });
            },
            drawCharts() {
                this.drawColumnChart()
            },
        },

        mounted: function () {
            let _this = this
            reportOccupancy().then(function (res) {
                _this.dataList.D1 = parseFloat(res.data.list.D1);
                _this.dataList.D2 = parseFloat(res.data.list.D2);
                _this.dataList.D3 = parseFloat(res.data.list.D3);
                _this.dataList.D4 = parseFloat(res.data.list.D4);
                _this.dataList.D5 = parseFloat(res.data.list.D5);
                _this.dataList.D6 = parseFloat(res.data.list.D6);
                _this.dataList.D7 = parseFloat(res.data.list.D7);
                _this.drawCharts();
            })
        },
        updated: function () {
            this.drawCharts()
        }
    }
</script>

<style scoped>
    .chart-container {
        width: 100%;
        float: left;
    }

    /*.chart div {
        height: 400px;
        float: left;
    }*/

    .el-col {
        padding: 30px 20px;
    }
</style>
