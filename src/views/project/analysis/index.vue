<template>
    <div class="analysis-index">
        <div class="page-wrapper">
            <a-row :gutter="24">
                <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
                    <chart-card :loading="loading" title="项目总数" :total="projectData.selfCount | NumberFormat">
                        <a-tooltip title="指标说明" slot="action">
                            <a-icon type="info-circle-o"/>
                        </a-tooltip>
                        <div class="chart-wrapper">
                            <ve-histogram
                                    :data="projectData.chartData"
                                    :settings="projectData.chartSettings"
                                    :extend="chartExtend"
                                    :legend-visible="false"
                                    height="55px"></ve-histogram>
                        </div>
                        <template slot="footer">本月立项 <span>{{projectData.monthCreated}}</span></template>
                    </chart-card>
                </a-col>
                <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
                    <chart-card :loading="loading" title="任务总数" :total="taskData.taskCount | NumberFormat">
                        <a-tooltip title="指标说明" slot="action">
                            <a-icon type="info-circle-o"/>
                        </a-tooltip>
                        <div>
                            <div class="chart-wrapper">
                                <ve-line
                                        :data="taskData.chartData"
                                        :settings="taskData.chartSettings"
                                        :extend="chartExtend"
                                        :legend-visible="false"
                                        height="55px"></ve-line>
                            </div>
                        </div>
                        <template slot="footer">今日任务<span> {{ taskData.todayTaskCount | NumberFormat }}</span></template>
                    </chart-card>
                </a-col>
                <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
                  
                    <chart-card :loading="loading" title="逾期任务" :total="taskData.overdueCount | NumberFormat">
                        <a-tooltip title="指标说明" slot="action">
                            <a-icon type="info-circle-o"/>
                        </a-tooltip>
                        <div>
                            <mini-progress color="#ffd401" :target="80" :percentage="taskData.overdueRatio" height="8px"/>
                        </div>
                        <template slot="footer">逾期率<span> {{ taskData.overdueRatioStr}}</span></template>
                    </chart-card>
                </a-col>
                <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
                    <chart-card :loading="loading" title="整体进度" :total="taskData.doneRatioStr">
                        <a-tooltip title="指标说明" slot="action">
                            <a-icon type="info-circle-o"/>
                        </a-tooltip>
                        <div>
                            <mini-progress color="#ffd401" :target="80" :percentage="taskData.doneRatio" height="8px"/>
                        </div>
                        <!-- <template slot="footer">
                            <trend flag="down" style="margin-right: 16px;">
                                <span slot="term">周同比</span>
                                12%
                            </trend>
                            <trend flag="up">
                                <span slot="term">日环比</span>
                                80%
                            </trend>
                        </template> -->
                    </chart-card>
                </a-col>
            </a-row>
            <a-card :loading="loading" :bordered="false" :body-style="{padding: '0'}">
                <div class="salesCard">
                    <a-tabs default-active-key="1" size="large"
                            :tab-bar-style="{marginBottom: '24px', paddingLeft: '16px'}">
                        <!-- <div class="extra-wrapper" slot="tabBarExtraContent">
                            <div class="extra-item">
                                <a>今日</a>
                                <a>本周</a>
                                <a>本月</a>
                                <a>本年</a>
                            </div>
                            <a-range-picker :style="{width: '256px'}"/>
                        </div> -->
                        <a-tab-pane forceRender tab="项目数" key="1">
                            <a-row>
                                <a-col :xl="16" :lg="12" :md="12" :sm="24" :xs="24">
                                    <div class="chart-wrappers-single">
                                        <ve-histogram
                                                :data="projectTotalData.chartData"
                                                :settings="projectTotalData.chartSettings"
                                                :extend="projectTotalData.chartExtend"
                                                :legend-visible="false"
                                                height="300px"></ve-histogram>
                                    </div>
                                </a-col>
                                <a-col :xl="8" :lg="12" :md="12" :sm="24" :xs="24">
                                    <rank-list title="项目数排行榜" :list="rankList"/>
                                </a-col>
                            </a-row>
                        </a-tab-pane>
                        <!-- <a-tab-pane forceRender tab="任务数" key="2">
                            <a-row>
                                <a-col :xl="16" :lg="12" :md="12" :sm="24" :xs="24">
                                    <div class="chart-wrappers-single">
                                        <ve-histogram
                                                :data="projectTotalData.chartData"
                                                :settings="projectTotalData.chartSettings"
                                                :extend="projectTotalData.chartExtend"
                                                :legend-visible="false"
                                                height="300px"></ve-histogram>
                                    </div>
                                </a-col>
                                <a-col :xl="8" :lg="12" :md="12" :sm="24" :xs="24">
                                    <rank-list title="任务数排行榜" :list="rankList"/>
                                </a-col>
                            </a-row>
                        </a-tab-pane> -->
                    </a-tabs>
                </div>
            </a-card>
            <a-row :gutter="12">
                <a-col :xl="12" :lg="24" :md="24" :sm="24" :xs="24">
                    <a-card :loading="loading" :bordered="false" title="我的项目" :style="{ marginTop: '24px' }">
                        <!-- <a-dropdown :trigger="['click']" placement="bottomLeft" slot="extra">
                            <a class="ant-dropdown-link" href="#">
                                <a-icon type="ellipsis"/>
                            </a>
                            <a-menu slot="overlay">
                                <a-menu-item>
                                    <a href="javascript:;">操作一</a>
                                </a-menu-item>
                                <a-menu-item>
                                    <a href="javascript:;">操作二</a>
                                </a-menu-item>
                            </a-menu>
                        </a-dropdown> -->
                        <p :key="prj.code" v-for="prj in projectData.selfList">{{prj.name}}</p>
                    </a-card>
                </a-col>
                <a-col :xl="12" :lg="24" :md="24" :sm="24" :xs="24">
                    <a-card :loading="loading" :bordered="false" title="任务优先级分布" :style="{ marginTop: '24px' }">
                        <!-- <a-dropdown :trigger="['click']" placement="bottomLeft" slot="extra">
                            <a class="ant-dropdown-link" href="#">
                                <a-icon type="ellipsis"/>
                            </a>
                            <a-menu slot="overlay">
                                <a-menu-item>
                                    <a href="javascript:;">操作一</a>
                                </a-menu-item>
                                <a-menu-item>
                                    <a href="javascript:;">操作二</a>
                                </a-menu-item>
                            </a-menu>
                        </a-dropdown> -->
                        <p v-for="item in taskData.countByPriority" :key="item.priority">{{item.priority}}: {{item.c}}</p>
                    </a-card>
                </a-col>
            </a-row>
        </div>
    </div>
</template>
<script>
    import {mapState} from 'vuex'
    import moment from "moment";
    import VeLine from 'v-charts/lib/line.common'
    import VeHistogram from 'v-charts/lib/histogram.common'
    import ChartCard from '@/components/chart/ChartCard'
    import Trend from '@/components/Trend'
    import MiniProgress from '@/components/chart/MiniProgress'
    import RankList from '@/components/chart/RankList'
    import pagination from "@/mixins/pagination"
    import {stat} from '@/api/project/'
    import {taskStatInfos, overdueCount} from '@/api/task/'

    const rankList = [];
    const taskList = [];
    const projectList = [];
    export default {
        components: {
            VeLine,
            VeHistogram,
            ChartCard,
            MiniProgress,
            Trend,
            RankList
        },
        mixins: [pagination],
        data() {
            return {
                loading: false,
                rankList,
                chartExtend: {
                    grid: {
                        left: '-25',
                        right: '0',
                        top: '10',
                        bottom: '-15'
                    },
                    series: {
                        barWidth: 15,
                    },
                    xAxis: {
                        show: false,
                    },
                    yAxis: {
                        show: false,
                    },
                    tooltip: {
                        backgroundColor: '#fff',
                        textStyle: {
                            color: '#333'
                        },
                        borderWidth: 1,
                        borderColor: '#e8e8e8',
                    },
                    axisPointer: {
                        lineStyle: {
                            width: 0
                        }
                    }
                },
                projectData: {
                    chartData: {
                        columns: ['日期', '数量'],
                        rows: projectList
                    },
                    chartSettings: {
                        itemStyle: {
                            color: '#1890ff'
                        },
                    },
                    selfCount: 0, 
                    monthCreated: 0,
                    selfList: []
                },
                taskData: {
                    chartData: {
                        columns: ['日期', '任务'],
                        rows: taskList
                    },
                    chartSettings: {
                        area: true,
                        itemStyle: {
                            color: '#b68eec'
                        },
                        areaStyle: {
                            color: '#b68eec'
                        }
                    },
                    todayTaskCount: 0, 
                    taskCount: 0, 
                    overdueCount: 0,
                    overdueRatio: 0,
                    doneRatio: 0, 
                    doneRatioStr: '0%', 
                    countByPriority: []
                },
                projectTotalData: {
                    chartData: {
                        columns: ['日期', '数量'],
                        rows: projectList
                    },
                    chartSettings: {
                        itemStyle: {
                            color: '#1890ff'
                        },
                    },
                    chartExtend: {
                        grid: {
                            left: '30',
                            right: '0',
                            top: '15',
                            bottom: '0'
                        },
                        series: {
                            barWidth: 45,
                        },
                    }
                },
            }
        },
        computed: {
            ...mapState({
                userInfo: state => state.userInfo,
            }),
        },
        created() {
            this.init();
        },
        methods: {
            init(reset = true) {
                if (reset) {
                    this.pagination.page = 1;
                    this.pagination.pageSize = 9;
                }

                let app = this
                stat(this.requestData).then(res => {
                    app.projectData.selfCount = res.data.count
                    app.projectData.selfList = res.data.list
                    
                    projectList.splice(0, projectList.length)
                    if (res.data.countByMonth.length) {                        
                        let nowDate = new Date()
                        let nowYear = nowDate.getFullYear()
                        let nowMonth = nowDate.getMonth() + 1

                        res.data.countByMonth.forEach(element => {
                            projectList.push({
                                "日期": element.year + "." + element.month, 
                                "数量": element.c
                            })

                            // 获取本月立项的项目数
                            if (element.year === nowYear && element.month === nowMonth) {
                                app.projectData.monthCreated = element.c
                            }
                        });

                        // 用户项目数排行
                        rankList.splice(0, rankList.length)
                        if (res.data.topByPerson) {
                            res.data.topByPerson.forEach(item => {
                                rankList.push({
                                    name: item.name, 
                                    total: item.c
                                })
                            })
                        }
                    }
                })
              

                // 获取任务统计清单
                taskStatInfos().then(res => {
                    taskList.splice(0, taskList.length)
                    if (res.data.taskStatList.length) {
                        res.data.taskStatList.forEach(item =>{
                            taskList.push({
                                "日期": item.month + "月" + item.day + "日", 
                                "任务": item.c
                            })
                        })
                    }

                    app.taskData.taskCount = res.data.taskCount
                    app.taskData.todayTaskCount = res.data.todayTaskCount
                    app.taskData.overdueCount = res.data.overdueCount

                    let doneCount = res.data.doneCount
                    let totalCount = Number(app.taskData.taskCount) + Number(doneCount)
                    if (app.taskData.taskCount) {
                        app.taskData.doneRatio = Math.round(100 * doneCount / totalCount)
                        app.taskData.doneRatioStr = app.taskData.doneRatio + '%'
                    } else {
                        app.taskData.doneRatio = 100
                        app.taskData.doneRationStr = "100%"
                    }

                    // 计算逾期率
                    if (app.taskData.taskCount) {
                        app.taskData.overdueRatio = Math.round(100 * app.taskData.overdueCount / app.taskData.taskCount)
                        app.taskData.overdueRatioStr = app.taskData.overdueRatio + '%'
                    } else {
                        app.taskData.overdueRatio = 0
                        app.taskData.overdueRatioStr = '0%'
                    }

                    // 任务按优先级统计结果
                    app.taskData.countByPriority = res.data.countByPriority
                })
            },
        }
    }
</script>
<style lang="less">
    .analysis-index {
        .page-wrapper {
            margin: 24px;

            .extra-wrapper {
                line-height: 55px;
                padding-right: 24px;

                .extra-item {
                    display: inline-block;
                    margin-right: 24px;

                    a {
                        margin-left: 24px;
                    }
                }
            }

            .chart-wrapper {
                position: absolute;
                bottom: -10px;
                width: 100%;
            }
            .chart-wrappers-single{
                /*width: 500px;*/
                div{
                    width: auto !important;
                }
            }
        }
    }
</style>
