<template>
    <div class="main-view">
        <h2 style="text-align: center">题目列表</h2>
        <Row style="margin-top: 10px">
            <Col span="16">
                <ModTable v-bind:tableLoading="tableLoading"
                          v-bind:total="tableRowCount"
                          v-bind:columns="tableColLayout"
                          v-bind:data="tableData"
                />
            </Col>
            <Col span="8" style="padding-left: 20px">
                <ModFilter ref="FilterComponent" @handlerFilter="handlerFilter"/>
            </Col>
        </Row>

    </div>
</template>

<script>
import ModTable from "@/components/problem/list/ModTable";
import ModFilter from "@/components/problem/list/ModFilter";
import API from "@/utils/api.js";

export default {
    name: "ProblemList",
    components: {ModFilter, ModTable},
    data() {
        return {
            tableLoading: true,
            tableRowCount: 0,
            tableColLayout: [                                   // 列布局
                    {
                        title: "题目编号",
                        key: "id"
                    },
                    {
                        title: "标题",
                        key: "title"
                    }
            ],
            tableData: [ ],
        }
    
    },
    mounted() {
        API.getProblemList().then(                              // 取得题目列表
            (responseData) =>
            {
                let lstdat = responseData.data.data;
                
                this.tableRowCount = lstdat.count;              // 题目总数
                this.tableData = lstdat.results;                // 题目的标题(title)和id(id)
            }
        );
        this.tableLoading = false;                              // 列表加载完毕
    },
    methods: {
        handlerFilter()                                         // 筛选
        {
            // 拿到筛选条件
            let filterForm = this.$refs.FilterComponent.formItem;
            let filterRule = {};
            if (filterForm.id == undefined)                     // TODO FIXME 是否有必要保证id为数字呢
            {
                filterRule.id = "";
            }
            else {
                filterRule.id = filterForm.id;
            }
            if (filterForm.title == undefined)
            {
                filterRule.title = "";
            }
            else {                                              // 删除两头的空格, 并保证是小写字符
                filterRule.title = filterForm.title.replace(/^\s+|\s+$/g, "").toLowerCase();
            }
            // 请求数据, 并进行筛选
            API.getProblemList().then(
                (responseData) =>
                {
                    let count = 0;                              // 筛选结果计数
                    let resTableData = [];
                    let lstdat = responseData.data.data;
                    for (let i of lstdat.results)
                    {
                        let willAppend = true;
                        if (filterRule.id.length > 0 && filterRule.id != i.id)
                        {
                            willAppend = false;
                        }
                        if (filterRule.title.length > 0 && i.title.toLowerCase().indexOf(filterRule.title) == -1)
                        {
                            willAppend = false;
                        }
                        if (willAppend)                         // 该项通过筛选, 就append
                        {
                            resTableData.push({
                                id: i.id,
                                title: i.title
                            });
                            count += 1;
                        }
                    }
                    this.tableData = resTableData;
                    this.tableRowCount = count;                 // 筛选结果总数
                }
            );
            this.tableLoading = false;
        }
    }
}
</script>

<style scoped>
.main-view {
    width: 95%;
    margin-left: auto;
    margin-right: auto;
    margin-top: 10px;
}
</style>
