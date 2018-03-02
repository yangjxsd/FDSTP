<template>
    <div class="motorcade">
        <el-popover ref="popover" placement="bottom" trigger="click" v-model="visible">
            <el-table ref="multipleTable" :data="dataSource" style="width: 100%" @selection-change="handleSelectionChange">
                <el-table-column label="车辆列表" width="360" prop="motorcadeName">
                </el-table-column>
            </el-table>
            <!-- <div class="motorcade-btn">
                <el-button type="primary" size="mini" @click="issued">确定</el-button>
            </div> -->
        </el-popover>
        <el-button type="text" size="small" v-popover:popover @click="visible =true">{{tableName}}</el-button>
    </div>
</template>
<script>
    export default {
        props: {
            dataSource: { // 表单的数据源
                type: Array,
                default () {
                    return []
                }
            },
            row: {
                type: Object,
                default () {
                    return {}
                }
            },
            tableName: {
                type: String,
                default: ''
            }
        },
        data() {
            return {
                visible: false,
                multipleSelection: []
            }
        },
        mounted() {
            console.log(this.dataSource)
        },
        methods: {
            handleSelectionChange(val) {
                this.multipleSelection = val;
                console.log(this.dataSource)
            },
            // 下发
            issued() {
            }
        }
    }

</script>
