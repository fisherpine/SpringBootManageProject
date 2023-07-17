<template>
    <div>
        <!--搜索栏-->
        <el-card id="search">
            <el-row>
                <el-col :span="20">
                    <div class="grid-content bg-purple">
                        <el-input v-model="searchModel.username" placeholder="用户名"></el-input>
                        <el-input v-model="searchModel.phone" placeholder="电话"></el-input>
                        <el-button type="primary" round icon="el-icon-search">查询</el-button>
                    </div>
                </el-col>
                <el-col :span="4" align="right">
                    <div class="grid-content bg-purple-light">
                        <el-button type="primary" icon="el-icon-plus" circle></el-button>
                    </div>
                </el-col>
            </el-row>
        </el-card>

        <!--结果列表-->
        <el-card>
            <el-table :data="userList" stripe style="width: 100%">
                <el-table-column type="index" label="#" width="180">
                </el-table-column>
                <!--prop需要和和数组里元素对应-->
                <el-table-column prop="id" label="用户名ID" width="180">
                </el-table-column>
                <el-table-column prop="username" label="用户名" width="180">
                </el-table-column>
                <el-table-column prop="phone" label="电话" width="180">
                </el-table-column>
                <el-table-column prop="email" label="电子邮箱">
                </el-table-column>
                <el-table-column label="操作" width="180">
                </el-table-column>
            </el-table>
        </el-card>
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="searchModel.pageNo"
            :page-sizes="[5,10,20,50]" 
            :page-size="searchModel.pageSize" 
            layout="total, sizes, prev, pager, next, jumper"
            :total="total">
        </el-pagination>
    </div>
</template>

<script>
export default {
    data() {
        return {
            total: 0,
            searchModel: {
                pageNo: 1,
                pageSize: 10
            },
            userList: []
        }
    },
    methods:{
        handleSizeChange(){

        },
        handleCurrentChange(){

        }
    }
};
</script>

<style>
#search .el-input {
    width: 200px;
    margin-right: 10px;
}
</style>