<template>
  <div>
    <el-container>
      <el-header>{{msg}}</el-header>
      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="客戶姓名" prop="name">
          <el-input v-model="ruleForm.name" style="width: 73%;"></el-input>
        </el-form-item>
        <el-form-item label="客戶手机" prop="telphone">
          <el-input v-model="ruleForm.telphone" style="width: 73%;" placeholder="客户手机唯一"></el-input>
        </el-form-item>
        <el-form-item label="访问方式" prop="visit">
          <el-select v-model="ruleForm.visit" placeholder="请选择活动区域" style="width: 73%;">
            <el-option label="电话" value="1"></el-option>
            <el-option label="去地面谈" value="2"></el-option>
            <el-option label="来地面谈" value="3"></el-option>
            <el-option label="其它" value="4"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="客户意向" prop="clientwill">
          <el-select v-model="ruleForm.clientwill" placeholder="请选择活动区域" style="width: 73%;">
            <el-option label="初次拜访" value="1"></el-option>
            <el-option label="1月内有意向" value="2"></el-option>
            <el-option label="2-3月内有意向" value="3"></el-option>
            <el-option label="近期无意向" value="4"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="访问日期" required>
          <el-col :span="20">
            <el-form-item prop="date1">
              <el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.date1"
                              style="width: 89%;"></el-date-picker>
            </el-form-item>
          </el-col>
        </el-form-item>
        <el-form-item label="客户来源" prop="source">
          <el-select v-model="ruleForm.source" placeholder="请选择客户来源" style="width: 73%;">
            <el-option label="网络" value="1"></el-option>
            <el-option label="转介绍" value="2"></el-option>
            <el-option label="扫街" value="3"></el-option>
            <el-option label="其它" value="4"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="咨询类型" prop="advicetype">
          <el-select v-model="ruleForm.advicetype" placeholder="请选择咨询类型" style="width: 73%;">
            <el-option label="仓储" value="1"></el-option>
            <el-option label="修理区" value="2"></el-option>
            <el-option label="商铺" value="3"></el-option>
            <el-option label="其它" value="4"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="意向平米" prop="meter">
          <el-input v-model="ruleForm.meter" style="width: 73%;"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">立即提交</el-button>
          <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
      <el-footer><span class="copyright-text"><span>©2018&nbsp;wwjswl.com&nbsp;</span> 金沙物流有限责任公</span>
      </el-footer>
    </el-container>

  </div>
</template>

<script>
  import ElHeader from "../../node_modules/element-ui/packages/header/src/main";
  import  commonBase from '../config/base'
  import qs from 'qs'
  export default {
    components: {ElHeader}, name: 'HelloWorld',
    data() {
      return {
        ruleForm: {
          name: '',
          telphone: '',
          visit: '',
          clientwill: '',
          date1: '',
          source: '',
          advicetype: '',
          meter: ''
        },
        msg: '客户管理系统',
        rules: {
          name: [
            {required: true, message: '请输入客戶姓名', trigger: 'blur'},
            {min: 2, max: 11, message: '长度在 3 到 5 个字符', trigger: 'blur'}
          ],
          telphone: [
            {required: true, message: '请填写客户手机号码', trigger: 'blur'},
            {min: 11, max: 11, message: '电话号码格式错误', trigger: 'blur'}
          ],
          visit: [
            {required: true, message: '请选择访问方式', trigger: 'change'}
          ],
          clientwill: [
            {required: true, message: '请选择客户意向', trigger: 'change'}
          ],
          source: [
            {required: true, message: '请选择客户来源', trigger: 'change'}
          ],
          advicetype: [
            {required: true, message: '请选择客户咨询类型', trigger: 'change'}
          ],
          date1: [
            {type: 'date', required: true, message: '请选择日期', trigger: 'change'}
          ]
        }
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            const params = {
              name: this.ruleForm.name,
              visit: this.ruleForm.visit,
              clientwill: this.ruleForm.clientwill,
              source: this.ruleForm.source,
              advicetype: this.ruleForm.advicetype,
              visitDate: this.ruleForm.date1,
              telphone: this.ruleForm.telphone
            }
            console.log(commonBase.reqUrl)
            this.$ajax({
              method: 'get',
              url: commonBase.reqUrl + '/record',
              data: qs.stringify(params),
            }).then(response => {
              let result = response.data
              this.$notify({
                title: '成功',
                message: result.message,
                type: 'success'
              })
              console.log(result)
            });
          }

          else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
  }
</script>

<style>
  .el-header, .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
  }
</style>
