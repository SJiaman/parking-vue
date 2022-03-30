<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="停车场名" prop="name">
      <el-input v-model="dataForm.name" placeholder="停车场名"></el-input>
    </el-form-item>
    <el-form-item label="联系人" prop="contacts">
      <el-input v-model="dataForm.contacts" placeholder="联系人"></el-input>
    </el-form-item>
    <el-form-item label="停车场电话" prop="phone">
      <el-input v-model="dataForm.phone" placeholder="停车场电话"></el-input>
    </el-form-item>
    <el-form-item label="停车场地址" prop="address">
      <el-input v-model="dataForm.address" placeholder="停车场地址"></el-input>
    </el-form-item>
    <el-form-item label="总车位" prop="totalSpaces">
      <el-input v-model="dataForm.totalSpaces" placeholder="总车位"></el-input>
    </el-form-item>
    <el-form-item label="剩余车位" prop="surplusSpaces">
      <el-input v-model="dataForm.surplusSpaces" placeholder="剩余车位"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="创建时间"></el-input>
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
          contacts: '',
          phone: '',
          address: '',
          totalSpaces: '',
          surplusSpaces: '',
          createTime: ''
        },
        dataRule: {
          name: [
            { required: true, message: '停车场名不能为空', trigger: 'blur' }
          ],
          contacts: [
            { required: true, message: '联系人不能为空', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '停车场电话不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '停车场地址不能为空', trigger: 'blur' }
          ],
          totalSpaces: [
            { required: true, message: '总车位不能为空', trigger: 'blur' }
          ],
          surplusSpaces: [
            { required: true, message: '剩余车位不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/pms/parkinginfo/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.parkingInfo.name
                this.dataForm.contacts = data.parkingInfo.contacts
                this.dataForm.phone = data.parkingInfo.phone
                this.dataForm.address = data.parkingInfo.address
                this.dataForm.totalSpaces = data.parkingInfo.totalSpaces
                this.dataForm.surplusSpaces = data.parkingInfo.surplusSpaces
                this.dataForm.createTime = data.parkingInfo.createTime
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
              url: this.$http.adornUrl(`/pms/parkinginfo/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'contacts': this.dataForm.contacts,
                'phone': this.dataForm.phone,
                'address': this.dataForm.address,
                'totalSpaces': this.dataForm.totalSpaces,
                'surplusSpaces': this.dataForm.surplusSpaces,
                'createTime': this.dataForm.createTime
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
