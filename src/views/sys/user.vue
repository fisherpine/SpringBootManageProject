<template>
    <div>
        <!--搜索栏-->
        <el-card id="search">
            <el-row>
                <el-col :span="20">
                    <div class="grid-content bg-purple">
                        <el-input v-model="searchModel.username" placeholder="用户名" clearable></el-input>
                        <el-input v-model="searchModel.phone" placeholder="电话" clearable></el-input>
                        <el-button @click="getUserList" type="primary" round icon="el-icon-search">查询</el-button>
                    </div>
                </el-col>
                <el-col :span="4" align="right">
                    <div class="grid-content bg-purple-light">
                        <el-button @click="openEditUI" type="primary" icon="el-icon-plus" circle></el-button>
                    </div>
                </el-col>
            </el-row>
        </el-card>

        <!--结果列表-->
        <el-card>
            <el-table :data="userList" stripe style="width: 100%">
                <el-table-column label="#" width="180">
                    <template slot-scope="scope">
                        <!--(pageNo-1)*pageSize + index + 1-->
                        {{ (searchModel.pageNo - 1) * searchModel.pageSize + scope.$index + 1 }}
                    </template>
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
        <!--分页组件-->
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
            :current-page="searchModel.pageNo" :page-sizes="[5, 10, 20, 50]" :page-size="searchModel.pageSize"
            layout="total, sizes, prev, pager, next, jumper" :total="total">
        </el-pagination>
        <!--用户信息编辑对话框-->
        <el-dialog @close="clearForm" :title="title" :visible.sync="dialogFormVisible">
            <el-form :model="userForm" ref="userFormRef" :rules="rules">
                <el-form-item label="用户名" prop="username" :label-width="formLabelWidth">
                    <el-input v-model="userForm.username" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="登陆密码" prop="password" :label-width="formLabelWidth">
                    <el-input v-model="userForm.password" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="联系电话" :label-width="formLabelWidth">
                    <el-input v-model="userForm.phone" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="用户状态" :label-width="formLabelWidth">
                    <el-switch v-model="userForm.status" :active-value="1" :inactive-value="0">
                    </el-switch>
                </el-form-item>
                <el-form-item label="电子邮件" prop="email" :label-width="formLabelWidth">
                    <el-input v-model="userForm.email" autocomplete="off"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
import userApi from '@/api/userManage'
export default {
    data() {
        var checkEmail = (rule, value, callback) => {
            var reg =/^\w+@[a-z0-9]+\.[a-z]{2,4}$/;
            if (!reg.test(value)) {
                return callback(new Error('邮箱格式错误'));
            }
            
        };
        return {
            formLabelWidth: '130px',
            userForm: {},
            dialogFormVisible: false,
            title: "",
            total: 0,
            searchModel: {
                pageNo: 1,
                pageSize: 10
            },
            userList: [],
            rules: {
                username: [
                    { required: true, message: '请输入用户名', trigger: 'blur' },
                    { min: 3, max: 50, message: '长度在 3 到 50 个字符', trigger: 'blur' }
                ],
                password: [
                    { required: true, message: '请输入用户名', trigger: 'blur' },
                    { min: 3, max: 50, message: '长度在 3 到 50 个字符', trigger: 'blur' }
                ],
                email: [
                    { required: true, message: '请输入电子邮件', trigger: 'blur' },
                    { validator: checkEmail, trigger: 'blur' }
                ],
            }
        }
    },
    methods: {
        clearForm() {
            this.userForm = {};
            this.$refs.userFormRef.clearValidate();
        },
        openEditUI() {
            this.title = '新增用户';
            this.dialogFormVisible = true;
        },
        handleSizeChange(pageSize) {
            this.searchModel.pageSize = pageSize;
            this.getUserList();
        },
        handleCurrentChange(pageNo) {
            this.searchModel.pageNo = pageNo;
            this.getUserList();
        },
        getUserList() {
            userApi.getUserList(this.searchModel).then(response => {
                this.userList = response.data.row;
                this.total = response.data.total;
            });
        }
    },
    created() {
        this.getUserList();
    }
};
</script>

<style>
#search .el-input {
    width: 200px;
    margin-right: 10px;
}

.el-dialog .el-input {
    width: 75%;
}
</style>