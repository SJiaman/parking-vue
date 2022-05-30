<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="用户名" prop="name">
      <el-input v-model="dataForm.name" placeholder="用户名"></el-input>
    </el-form-item>
    <el-form-item label="性别" prop="gender">
       <el-radio-group  v-model="dataForm.gender">
          <el-radio :label="0">男</el-radio>
          <el-radio :label="1">女</el-radio>
        </el-radio-group>
    </el-form-item>
    <el-form-item label="电话号码" prop="phone">
      <el-input v-model="dataForm.phone" placeholder="电话号码"></el-input>
    </el-form-item>
    <el-form-item label="住址" prop="address">
      <el-input v-model="dataForm.address" placeholder="住址"></el-input>
    </el-form-item>
    <el-form-item label="车位" prop="spaceNumber">
      <el-input v-model="dataForm.spaceNumber" placeholder="车位"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="createTime">
      <el-date-picker
        value-format="yyyy-MM-dd HH:mm:ss"
        v-model="dataForm.createTime"
        type="datetime"
        placeholder="选择日期时间">
      </el-date-picker>
    </el-form-item>
    <el-form-item label="到期时间" prop="expireTime">
       <el-date-picker
        value-format="yyyy-MM-dd HH:mm:ss"
        v-model="dataForm.expireTime"
        type="datetime"
        placeholder="选择日期时间">
      </el-date-picker>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          name: '',
          gender: 0,
          phone: '',
          address: '',
          spaceNumber: '',
          createTime: '',
          expireTime: ''
        },
        dataRule: {
          name: [
            { required: true, message: '用户名不能为空', trigger: 'blur' }
          ],
          gender: [
            { required: true, message: '性别不能为空', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '电话号码不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '住址不能为空', trigger: 'blur' }
          ],
          spaceNumber: [
            { required: true, message: '车位不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          expireTime: [
            { required: true, message: '到期时间不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/pms/member/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.member.name
                this.dataForm.gender = data.member.gender
                this.dataForm.phone = data.member.phone
                this.dataForm.address = data.member.address
                this.dataForm.spaceNumber = data.member.spaceNumber
                this.dataForm.createTime = data.member.createTime
                this.dataForm.expireTime = data.member.expireTime
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/pms/member/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'gender': this.dataForm.gender,
                'phone': this.dataForm.phone,
                'address': this.dataForm.address,
                'spaceNumber': this.dataForm.spaceNumber,
                'createTime': this.dataForm.createTime,
                'expireTime': this.dataForm.expireTime
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
