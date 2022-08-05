<template>
  <div>
    <div style="margin: 10px 0">
      <el-input placeholder="请输入名称 " style="width: 200px" suffix-icon="el-icon-search" v-model="username"> </el-input>
      <el-input
        placeholder="请输入邮箱 "
        style="width: 200px"
        suffix-icon="el-icon-message"
        class="ml8"
        v-model="email"
      ></el-input>
      <el-input
        placeholder="请输入地址 "
        style="width: 200px"
        suffix-icon="el-icon-position"
        class="ml8"
        v-model="address"
      ></el-input>
      <el-button type="primary" class="ml8" @click="load">搜索</el-button>
      <el-button type="warning" class="ml8" @click="reset">重置</el-button>
    </div>

    <div style="margin: 10px 0; display: flex">
      <el-button type="primary" @click="handleAdd">
        新增
        <i class="el-icon-circle-plus"></i>
      </el-button>
      <el-popconfirm
        confirm-button-text="确认删除"
        cancel-button-text="我再想想"
        icon="el-icon-info"
        icon-color="red"
        title="确认批量删除这些用户吗？"
        @confirm="delBatch"
      >
        <el-button type="danger" class="ml8" slot="reference">
          批量删除
          <i class="el-icon-remove"></i>
        </el-button>
      </el-popconfirm>
      <el-upload
        action="http://localhost:9090/manage/user/import"
        :show-file-list="false"
        :accept="xlsx"
        :on-success="handleExcelImportSucess"
      >
        <el-button type="primary" class="ml8">
          导入
          <i class="el-icon-bottom"></i>
        </el-button>
      </el-upload>

      <el-button type="primary" class="ml8" @click="exportUser">
        导出
        <i class="el-icon-top"></i>
      </el-button>
    </div>

    <!-- 表格数据 -->
    <el-table :data="tableData" @selection-change="handleSelectionChange">
      <el-table-column type="selection" width="55"> </el-table-column>
      <el-table-column prop="id" label="ID" width="80"></el-table-column>
      <el-table-column prop="username" label="用户名" width="140"></el-table-column>
      <el-table-column prop="nickname" label="昵称" width="120"></el-table-column>
      <el-table-column prop="email" label="邮箱"> </el-table-column>
      <el-table-column prop="phone" label="电话"> </el-table-column>
      <el-table-column prop="address" label="地址"> </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button type="success" @click="handleEdit(scope.row)">编辑<i class="el-icon-edit"></i></el-button>
          <el-popconfirm
            class="ml8"
            confirm-button-text="确认删除"
            cancel-button-text="我再想想"
            icon="el-icon-info"
            icon-color="red"
            title="确认删除该用户吗？"
            @confirm="del(scope.row.id)"
          >
            <el-button type="danger" slot="reference">删除<i class="el-icon-remove-outline"></i> </el-button>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>

    <div style="padding: 10px">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pageNum"
        :page-sizes="pageSizes"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      >
      </el-pagination>
    </div>

    <el-dialog title="用户信息" :visible.sync="dialogFormVisible" width="30%">
      <el-form :model="form" :label-width="formLabelWidth" size="small">
        <el-form-item label="用户名">
          <el-input v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码">
          <el-input v-model="form.passwrod" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="昵称">
          <el-input v-model="form.nickname" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="邮箱">
          <el-input v-model="form.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="手机">
          <el-input v-model="form.phone" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="地址">
          <el-input v-model="form.address" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="save">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'User',
  data() {
    return {
      // data
      tableData: [],
      total: 0,
      pageNum: 1,
      pageSize: 5,
      multipleSelection: [],
      // user data
      username: '',
      email: '',
      address: '',
      // setting of insert info
      dialogFormVisible: false,
      formLabelWidth: '60px',
      form: {},
    };
  },
  computed: {
    pageSizes() {
      return [this.pageSize, this.pageSize * 2, this.pageSize * 3, this.pageSize * 4];
    },
  },
  created() {
    this.load();
  },
  methods: {
    load() {
      this.request
        .get('/manage/user/page', {
          params: {
            pageNum: this.pageNum,
            pageSize: this.pageSize,
            nusername: this.username,
            email: this.email,
            address: this.address,
          },
        })
        .then((res) => {
          this.tableData = res.data.records;
          this.total = res.data.total;
        });
    },
    reset() {
      this.username = '';
      this.email = '';
      this.address = '';
      this.load();
    },
    handleSizeChange(val) {
      this.pageSize = val;
      this.load();
    },
    handleCurrentChange(val) {
      this.pageNum = val;
      this.load();
    },
    handleAdd() {
      this.dialogFormVisible = true;
    },
    handleEdit(row) {
      this.form = row;
      this.dialogFormVisible = true;
      this.load();
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    delBatch() {
      let ids = this.multipleSelection.map((v) => v.id);
      this.request.post('/manage/user/del', ids).then((res) => {
        if (res) {
          this.$message.success('批量删除成功');
          this.load();
        } else {
          this.$message.error('批量删除失败');
        }
      });
    },
    del(id) {
      this.request.delete('/manage/user/' + id).then((res) => {
        if (res) {
          this.$message.success('删除成功');
          this.dialogFormVisible = false;
          this.load();
        } else {
          this.$message.error('删除失败');
          this.dialogFormVisible = false;
        }
      });
    },
    save() {
      this.request.post('http://localhost:9090/manage/user', this.form).then((res) => {
        if (res) {
          this.$message.success('保存成功');
          this.dialogFormVisible = false;
          this.load();
        } else {
          this.$message.error('保存失败');
          this.dialogFormVisible = false;
        }
      });
    },
    handleExcelImportSucess() {
      this.$message.success('导入用户数据成功');
      this.load();
    },
    exportUser() {
      window.open('http://localhost:9090/manage/user/export');
    },
  },
};
</script>

<style>
</style>