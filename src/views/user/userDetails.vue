<template>
  <div class="">
    <custom-form name="userDetails"
      @on-submit="onSubmit"
      :items="formRule"
      :data="data"
      :rules="rules">
    </custom-form>
  </div>
</template>

<script>
import CustomForm from '@/components/CustomForm'

export default {
  data() {
    return {
      formRule: { // 生成表单的数据
        user_id: {
          type: 'input',
          label: 'ID',
          disabled: true
        },
        date: {
          type: 'date',
          label: '时间',
          placeholder: '请选择时间'
        },
        name: {
          type: 'input',
          label: '用户名',
          placeholder: '请输入用户名'
        },
        account: {
          type: 'input',
          label: '账号',
          placeholder: '请输入账号'
        },
        sex: {
          type: 'radio',
          label: '性别',
          options: [{
            label: '男',
            value: '0'
          }, {
            label: '女',
            value: '1'
          }]
        },
        role: {
          type: 'select',
          label: '角色',
          placeholder: '请选择角色',
          options: [{
            label: '管理员',
            value: 'admin'
          }, {
            label: '编辑',
            value: 'edit'
          }]
        }
      },
      data: { // model数据
        user_id: '',
        date: '',
        name: '',
        account: '',
        sex: '0',
        role: ''
      },
      rules: { // 表单验证
        name: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 6, max: 12, message: '长度在 6 到 12 个字符', trigger: 'blur' }
        ],
        date: [
          { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
        ],
        account: [
          { required: true, message: '请输入账号', trigger: 'blur' },
          { min: 6, max: 16, message: '长度在 6 到 16 个字符', trigger: 'blur' }
        ],
        role: [
          { required: true, message: '请选择性别', trigger: 'change' }
        ]
      }
    }
  },
  methods: {
    onSubmit(data) {
      console.log(data)
    }
  },
  created() {

  },
  components: {
    CustomForm
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>

</style>
