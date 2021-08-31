<template>
  <div>
<!--    button block-->
    <div style="margin-bottom: 10px;">
      <el-button size="small" type="primary" @click="add">新增</el-button>
      <el-button size="small" type="success">导入</el-button>
      <el-button size="small" type="warning">导出</el-button>
    </div>
<!--    search block-->
    <div style="margin-bottom: 15px;">
      <el-input v-model="search" placeholder="please input" style="width: 20%;" clearable></el-input>
      <el-button type="info" @click="searchUser">GO</el-button>
    </div>
    <el-table
      :data="tableData"
      stripe
      border
      style="width: 90%">
      <el-table-column
        prop="id"
        label="ID">
      </el-table-column>
      <el-table-column
        prop="username"
        label="用户名">
      </el-table-column>
      <el-table-column
        prop="nickName"
        label="昵称">
      </el-table-column>
      <el-table-column
        prop="age"
        label="年龄">
      </el-table-column>
      <el-table-column
        prop="sex"
        label="性别">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址">
      </el-table-column>
      <el-table-column
        fixed="right"
        label="操作"
        width="150">
        <template slot-scope="scope">
          <el-button @click="handleEdit(scope.row)" size="small">编辑</el-button>
          <el-popconfirm title="确定删除吗？" @confirm="handleDelete(scope.row.id)">
            <el-button size="small" slot="reference" type="danger">删除</el-button>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
    <div style="margin: 10px 0px;">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-sizes="[5, 10, 20]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total">
      </el-pagination>
    </div>

    <el-dialog
      title="新增"
      :visible.sync="dialogVisible"
      width="30%">
      <el-form label-width="120px">
        <el-form-item label="用户名" style="width: 80%">
          <el-input v-model="form.username"></el-input>
        </el-form-item>
        <el-form-item label="昵称" style="width: 80%">
          <el-input v-model="form.nickName"></el-input>
        </el-form-item>
        <el-form-item label="年龄" style="width: 80%">
          <el-input v-model="form.age"></el-input>
        </el-form-item>
        <el-form-item label="性别" style="width: 80%">
          <el-radio-group v-model="form.sex">
            <el-radio label="男">男</el-radio>
            <el-radio label="女">女</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="地址" style="width: 80%">
          <el-input type="textarea" v-model="form.address"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="save">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
    import request from "../utils/request";

    export default {
        name: "Main",
        data() {
            return {
                search: '',
                currentPage: 1,
                pageSize: 5,
                total: 20,
                dialogVisible: false,
                form: {
                    username: '',
                    nickName: '',
                    age: '',
                    sex: false,
                    address: '',
                },
                tableData: []
            }
        },
        created() {
            this.searchUser();
        },
        methods: {
            add() {
                this.dialogVisible = true;
                this.form = {};
            },
            save() {
                if(this.form.id) {
                    request.post('/api/user/update', this.form).then(data => {
                        console.log(data);
                        let type = "error", message = data.msg;
                        if(data.code === '0') {
                            type = "success", message = "修改成功"
                        }
                        this.$message({
                            type: type,
                            message: message
                        });
                    });
                    this.searchUser();
                    this.dialogVisible = false;
                } else {
                    request.post('/api/user/insert', this.form).then(data => {
                        console.log(data);
                        let type = "error", message = data.msg;
                        if(data.code === '0') {
                            type = "success", message = "新增成功"
                        }
                        this.$message({
                            type: type,
                            message: message
                        });
                    })
                    this.dialogVisible = false;
                    this.searchUser();
                }
            },
            searchUser() {
                request.get('/api/user/search', {
                    params: {
                        pageNum: this.currentPage,
                        pageSize: this.pageSize,
                        search: this.search
                    }
                }).then(data => {
                    console.log(data);
                    this.tableData = data.data.records;
                    this.total = data.data.total;
                })
            },
            handleEdit(row) {
                console.log(row);
                this.dialogVisible = true;
                this.form = JSON.parse((JSON.stringify(row)));
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
                this.pageSize = val;
                this.searchUser();
            },
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
                this.currentPage = val;
                this.searchUser();
            },
            handleDelete(id) {
                console.log(id);
                request.delete('/api/user/delete/' + id).then(res => {
                    let type = "error", message = res.msg;
                    if(res.code === '0') {
                        type = "success", message = "删除成功"
                    }
                    this.$message({
                        type: type,
                        message: message
                    });
                });
                this.searchUser();
            }
        },
    }
</script>

<style scoped>

</style>
