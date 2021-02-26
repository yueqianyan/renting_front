<template>
    <div>
        <el-menu
                :default-active="activeIndex2"
                class="el-menu-demo"
                mode="horizontal"
                @select="handleSelect"
                background-color="#545c64"
                text-color="#fff"
                active-text-color="#ffd04b">
            <el-menu-item index="1">首页</el-menu-item>
            <el-menu-item index="2" @click="selectHousingInfo">查询小区信息</el-menu-item>
        </el-menu>
        <h1 style="color: blue">租房小区评级系统</h1>
    <div style="display: flex;">
        <el-form style="width: 45%" ref="form" :model="form" label-width="180px">
            <el-form-item label="小区名称">
                <el-input v-model="form.housingEstate"></el-input>
            </el-form-item>
            <el-form-item label="到地铁距离">
                <el-input v-model="form.subway"></el-input>
            </el-form-item>
            <el-form-item label="到公司距离">
                <el-input v-model="form.company"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="onSubmit">记录信息</el-button>
                <el-button>取消</el-button>
            </el-form-item>
        </el-form>

        <div class="block" style="margin-left: 10%">
            <span class="demonstration">
                <el-table
                        :data="tableData"
                        style="width: 100%;">
                    <el-table-column
                            prop="housingEstate"
                            label="小区名称"
                            width="180">
                    </el-table-column>
                    <el-table-column
                            prop="score"
                            label="评分"
                            width="180">
                    </el-table-column>
                    <el-table-column
                            prop="score"
                            label="操作"
                            width="180">
                        <el-button @click="handleClick(scope.row)" type="text" size="small">删除</el-button>
                    </el-table-column>
                </el-table>
            </span>
            <el-pagination
                    @current-change="handleCurrentChange"
                    :current-page.sync="currentPage"
                    :page-size="size"
                    layout="total, prev, pager, next"
                    :total="total">
            </el-pagination>
        </div>
    </div>
    </div>
</template>

<script>
    import axios from "axios";
    export default {
        name: "Main",
        data() {
            return {
                form: {
                    housingEstate: '',
                    subway: '',
                    company: ''
                },
                tableData: {
                    housingEstate: '',
                    score: ''
                },
                currentPage: 1,
                total: 100,
                size: 8
            }
        },
        methods: {
            handleClick(row) {
                console.log(row)
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                console.log(`当前页: ${val}`);
                this.selectHousingInfo();
            },
            selectHousingInfo() {
                let data={
                    current:this.currentPage,
                    size:this.size
                };
                axios({
                    method: 'post',
                    url: 'http://127.0.0.1:8080/queryHouseInfo',
                    data: data
                }).then((res) => {
                    this.tableData = res.data.data.list;
                    this.total = res.data.data.total;
                })
            },
            onSubmit() {
                let data={
                    housingEstate:this.form.housingEstate,
                    subway:this.form.subway,
                    company:this.form.company
                };
                axios({
                    method: 'post',
                    url: 'http://127.0.0.1:8080/setHouseInfo',
                    data: data
                }).then(() =>{
                    this.housingEstate = '';
                    this.subway = '';
                    this.company = '';
                    alert("添加成功");
                    this.selectHousingInfo();
                })
            }
        }
    }
</script>

<style scoped>

</style>