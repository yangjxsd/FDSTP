<template>
    <Layout>
        <div slot="operate">
            <el-date-picker v-model="checkDate" type="datetimerange" @change="changeDate1" range-separator="至" start-placeholder="开始日期"
                end-placeholder="结束日期" format="yyyy-MM-dd" size="small" align="right">
            </el-date-picker>
            <el-button @click="exportTable" size="small">导出</el-button>
        </div>
        <div slot="content" style="">
            <TableInfo :columns="columns" :dataSource="dataSource"  style="width:15%;float:left" @row-click="linkCompany" >
            </TableInfo>
            <div style="width:84%;float:left;">
                <TableInfo :columns="columnsDetail" :dataSource="dataSourceDetail" :searchVisible=false style="height:120px" >
                </TableInfo>
                <TableInfo :columns="columnsDetail1" :dataSource="dataSourceDetail1" style="height:300px">
                </TableInfo>
            </div>
        </div>
        
    </Layout>
</template>
<script>
    import {
        tableSelect
    } from '../../../api/table';
    import TableInfo from 'Components/table/index';
    import Layout from '../layout/index';
    import mixin from '../mixins/tableMixins';
    import FormModal from 'Components/formModal/index';
    const TABLE_NAME = 'govermentcorpNameList';
    export default {
        mixins: [mixin],
        components: {
            TableInfo,
            FormModal,
            Layout
        },
        data() {
            const thisDate = new Date();
            thisDate.setDate(1);
            return {
                columnsDetail1: [],
                dataSourceDetail1: [],
                TABLE_NAME,
                dialogVisible: false,
                checkDate: [thisDate, new Date()],
                corpId: '',
                companyName: ''
            }
        },
        mounted () {
            this.tableName = TABLE_NAME;
            this.queryTable({
                table: this.TABLE_NAME,
                sDate: this.formatDate(this.checkDate[0]),
                eDate: this.formatDate(this.checkDate[1])
            });
            this.queryDetailTable({
                companyId: '',
                companyName: '',
                table: 'govermentStatistics',
                sDate: this.formatDate(this.checkDate[0]),
                eDate: this.formatDate(this.checkDate[1])
            });
            this.queryDetailTable1({
                companyId: '',
                companyName: '',
                table: 'govermentStatisticsDetail',
                sDate: this.formatDate(this.checkDate[0]),
                eDate: this.formatDate(this.checkDate[1])
            })

        },
        methods: {

            queryDetailTable1(params) {
                tableSelect(params)
                .then(res => {
                    this.columnsDetail1 = this.adapterColumns(res.headList);
                    this.dataSourceDetail1 = res.contentList;
                })
                .catch(err => {
                    console.log(err)
                });
            },
            changeDate1(date) {
                this.queryDetailTable({
                    companyId: this.corpId,
                    companyName: this.companyName,
                    table: 'govermentStatistics',
                    sDate: this.formatDate(this.date[0]),
                    eDate: this.formatDate(this.date[1])
                });
                this.queryTable({
                    table: this.TABLE_NAME,
                    sDate: this.formatDate(date[0]),
                    eDate: this.formatDate(date[1])
                });
                this.queryDetailTable1({
                    companyId: this.corpId,
                    companyName: this.companyName,
                    table: 'govermentStatisticsDetail',
                    sDate: this.formatDate(this.checkDate[0]),
                    eDate: this.formatDate(this.checkDate[1])
                 });
            },
            showPopover(id) {
                this.dialogVisible = true;
                this.queryLetterTable(id);
            },
            linkCompany (row, event, column) {
                this.corpId = row.id
                this.companyName = row.corpName
                this.queryDetailTable({
                    companyId: row.id,
                    companyName: row.corpName,
                    table: 'govermentStatistics',
                    sDate: this.formatDate(this.checkDate[0]),
                    eDate: this.formatDate(this.checkDate[1])
                });
                this.queryDetailTable1({
                    companyId: this.corpId,
                    companyName: this.companyName,
                    table: 'govermentStatisticsDetail',
                    sDate: this.formatDate(this.checkDate[0]),
                    eDate: this.formatDate(this.checkDate[1])
                 });
            },
            exportTable () {
                 this.queryDetailTable({
                    companyId: '',
                    download: true,
                    table: 'govermentTraining',
                    sDate: this.formatDate(this.checkDate[0]),
                    eDate: this.formatDate(this.checkDate[1])
                });
            },
            onRowClick (row, event, column) {
                console.log(row)
            }
        }
    }

</script>
