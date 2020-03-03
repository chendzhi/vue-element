<template>
  <div class="app-container">
      <div class="searchCon">
        <el-row>
          <el-col :span="20">
            <el-form :inline="true" :model="formInline" class="demo-form-inline">
              <el-form-item label="">
                <el-input v-model="formInline.user" placeholder="请输入内容"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="onSubmit" size="small">查询</el-button>
              </el-form-item>
            </el-form>
          </el-col>
          <el-col :span="4" align="right">
            <el-button type="primary" size="small" class="addButton" @click="dialogFormVisible = true">新增</el-button>
          </el-col>
        </el-row>
      </div>
    <el-table
    :data="tableData"
    :border="true"
    style="width: 100%">
    <el-table-column
      label="姓名"
      align="center"
      prop="fromName">
    </el-table-column>
    <el-table-column
      label="邮箱"
      align="center"
      prop="email">
    </el-table-column>
    <el-table-column
      label="来信目的"
      align="center"
      prop="objectiveType">
    </el-table-column>
    <el-table-column
      label="手机号"
      align="center"
      prop="phone">
    </el-table-column>
    <el-table-column
      label="固定电话"
      align="center"
      prop="fixedPhone">
    </el-table-column>
    <el-table-column
      label="地址"
      align="center"
      prop="address">
    </el-table-column>
    <el-table-column
      label="邮政编码"
      align="center"
      prop="postcode">
    </el-table-column>
    <el-table-column
      label="标题"
      align="center"
      prop="title">
    </el-table-column>
    <el-table-column
      label="正文"
      align="center"
      prop="content">
    </el-table-column>
    <el-table-column
      label="是否公开"
      align="center"
      prop="isPublic">
      <template slot-scope="scope">
          <span>{{scope.row.isPublic === 1 ? '公开' : '未公开'}}</span>
      </template>
    </el-table-column>
  </el-table>
  <!-- 新增弹窗 -->
  <el-dialog title="新增信件" :visible.sync="dialogFormVisible">
  <el-form :model="form" :label-width="formLabelWidth" :rules="rules" ref="form" >
    <el-form-item label="姓名" prop="fromName">
      <el-input v-model="form.fromName"></el-input>
    </el-form-item>
    <el-form-item label="邮箱" prop="email">
      <el-input v-model="form.email"></el-input>
    </el-form-item>
    <el-form-item label="来信目的" prop="objectiveType">
      <el-select v-model="form.objectiveType" placeholder="请选择来信目的">
        <el-option label="投诉" value="0"></el-option>
        <el-option label="建议" value="1"></el-option>
        <el-option label="举报" value="2"></el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="手机号" prop="phone">
      <el-input v-model="form.phone"></el-input>
    </el-form-item>
    <el-form-item label="固定电话" prop="fixedPhone">
      <el-input v-model="form.fixedPhone"></el-input>
    </el-form-item>
    <el-form-item label="来信地址" prop="address">
      <el-input v-model="form.address"></el-input>
    </el-form-item>
    <el-form-item label="邮政编码" prop="postcode">
      <el-input v-model="form.postcode"></el-input>
    </el-form-item>
    <el-form-item label="标题" prop="title">
      <el-input v-model="form.title"></el-input>
    </el-form-item>
    <el-form-item label="正文" prop="content">
      <el-input type="textarea" v-model="form.content"></el-input>
    </el-form-item>
    <el-form-item label="是否公开" prop="isPublic">
      <el-radio-group v-model="form.isPublic">
        <el-radio :label="1">是</el-radio>
        <el-radio :label="0">否</el-radio>
      </el-radio-group>
    </el-form-item>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible = false">取 消</el-button>
    <el-button type="primary" @click="submitForm('form')">确 定</el-button>
  </div>
</el-dialog>


  </div>
</template>

<script>
// import { getList } from '@/api/table'
import axios from 'axios'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'gray',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
     var validateEmail = (rule, value, callback) => {
       var regEmail = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
        if (value === '') {
          callback(new Error('请输入邮箱'))
        } else {
          if (regEmail.test(value)) {
             callback()
          }else{
            callback(new Error('邮箱格式不正确'))
          }
        }
      };
    return {
      list: null,
      listLoading: true,
      formInline: {
          user: '',
          region: ''
      },
      tableData: [{
          fromName: '王小虎',
          email: '506930754@qq.com',
          objectiveType: '投诉',
          phone: '18483600807',
          fixedPhone: '028-88888',
          address: '成都高新区',
          postcode: '621111',
          title: '道路堵塞',
          content: '成都高新区道路严重堵塞',
          isPublic: 1
        },
        {
          fromName: '王大虎',
          email: '666666666@qq.com',
          objectiveType: '建议',
          phone: '18200504514',
          fixedPhone: '028-88888',
          address: '成都武侯区',
          postcode: '621111',
          title: '交通拥堵',
          content: '成都武侯区交通拥堵',
          isPublic: 0
        }],
        dialogFormVisible: false,
        form: {
          fromName: '',
          email: '',
          objectiveType: '0',
          phone: '',
          fixedPhone: '',
          address: '',
          postcode: '',
          title: '',
          content: '',
          isPublic: 1
        },
        formLabelWidth: '80px',
        rules: {
            fromName: [
              { required: true, message: '请输入来信人姓名', trigger: 'blur' },
            ],
            email: [
              { validator: validateEmail, trigger: 'blur' }
            ],
            phone: [
            { required: true, message: '请输入手机号', trigger: 'blur' },
            { min: 11, max: 11, message: '请输入正确的手机号', trigger: 'blur' }
           ],
           title: [
              { required: true, message: '标题不能为空', trigger: 'blur' },
           ],
           content: [
              { required: true, message: '内容不能为空', trigger: 'blur' },
           ]
        }
    }
  },
  created() {
    // this.fetchData()
  },
  methods: {
    // fetchData() {
    //   this.listLoading = true
    //   getList().then(response => {
    //     this.list = response.data.items
    //     this.listLoading = false
    //   })
    // },
    onSubmit() {
      console.log('submit!')
    },
    submitForm(formName) {
    this.$refs[formName].validate((valid) => {
      if (valid) {
        // var formData = {
        //   fromName: this.form.fromName,
        //   email: this.form.email,
        //   objectiveType: this.form.objectiveType,
        //   phone: this.form.phone,
        //   fixedPhone: this.form.fixedPhone,
        //   address: this.form.address,
        //   postcode: this.form.postcode,
        //   title: this.form.title,
        //   content: this.form.content,
        //   isPublic: this.form.isPublic
        // }
        this.$axios.post('/api-mailbox/mails/save',this.form).then(res => {
          console.log(res)
        }).catch(res => {
          console.log(res)
        })
      } else {
        console.log('error submit!!');
        return false;
      }
    })
  }
  }
}
</script>
<style scoped>
.addButton{
  margin-top: 5px;
}
.searchCon{
  margin: 30px 0;
}
</style>
