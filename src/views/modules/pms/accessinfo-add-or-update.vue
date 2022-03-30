<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="用户id" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户id"></el-input>
    </el-form-item>
    <el-form-item label="车牌号" prop="licencePlate">
      <el-input v-model="dataForm.licencePlate" placeholder="车牌号"></el-input>
    </el-form-item>
    <el-form-item label="进入时间" prop="entryTime">
      <el-input v-model="dataForm.entryTime" placeholder="进入时间"></el-input>
    </el-form-item>
    <el-form-item label="出去时间" prop="outTime">
      <el-input v-model="dataForm.outTime" placeholder="出去时间"></el-input>
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
          userId: '',
          licencePlate: '',
          entryTime: '',
          outTime: ''
        },
        dataRule: {
          userId: [
            { required: true, message: '用户id不能为空', trigger: 'blur' }
          ],
          licencePlate: [
            { required: true, message: '车牌号不能为空', trigger: 'blur' }
          ],
          entryTime: [
            { required: true, message: '进入时间不能为空', trigger: 'blur' }
          ],
          outTime: [
            { required: true, message: '出去时间不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/pms/accessinfo/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.userId = data.accessInfo.userId
                this.dataForm.licencePlate = data.accessInfo.licencePlate
                this.dataForm.entryTime = data.accessInfo.entryTime
                this.dataForm.outTime = data.accessInfo.outTime
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
              url: this.$http.adornUrl(`/pms/accessinfo/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'userId': this.dataForm.userId,
                'licencePlate': this.dataForm.licencePlate,
                'entryTime': this.dataForm.entryTime,
                'outTime': this.dataForm.outTime
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
