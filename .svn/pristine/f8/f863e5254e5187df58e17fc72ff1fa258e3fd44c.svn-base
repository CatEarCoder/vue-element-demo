<template>
    <section class="chart-container">
        <el-row>
            <el-col :span="12">
                <div id="chartPie" style="width:100%; height:400px;"></div>
            </el-col>
        </el-row>
    </section>
</template>

<script>
    import echarts from 'echarts'
    import {reportCustSource} from '../../api/api';

    export default {
        data() {
            return {
                chartPie: null,
                dataList: {
                    orderAllNUm: '',
                    orderCYHNum: '',
                    orderSBNum: ''
                }
            }
        },

        methods: {
            drawPieChart() {
                this.chartPie = echarts.init(document.getElementById('chartPie'));
                this.chartPie.setOption({
                    title: {
                        text: '客源分析',
                        subtext: '客源分析',
                        x: 'center'
                    },
                    tooltip: {
                        trigger: 'item',
                        formatter: "{a} <br/>{b} : {c} ({d}%)"
                    },
                    legend: {
                        orient: 'vertical',
                        left: 'left',
                        data: ['总订单数', '晨游会订单数', '商报订单数']
                    },
                    series: [
                        {
                            name: '',
                            type: 'pie',
                            radius: '55%',
                            center: ['50%', '60%'],
                            data: [
                                {value: this.dataList.orderAllNUm, name: '总订单数'},
                                {value: this.dataList.orderCYHNum, name: '晨游会订单数'},
                                {value: this.dataList.orderSBNum, name: '商报订单数'},
                            ],
                            itemStyle: {
                                emphasis: {
                                    shadowBlur: 10,
                                    shadowOffsetX: 0,
                                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                                }
                            }
                        }
                    ]
                });
            },
            drawCharts() {
                this.drawPieChart()
            },
        },

        mounted: function () {
            let _this = this
            reportCustSource().then(function (res) {
                _this.dataList.orderAllNUm = res.data.list.orderAllNUm;
                _this.dataList.orderCYHNum = res.data.list.orderCYHNum;
                _this.dataList.orderSBNum = res.data.list.orderSBNum;
                _this.drawPieChart()
            })
        },
        updated: function () {
            this.drawPieChart()
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
