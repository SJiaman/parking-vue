<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="车位编号" prop="number">
      <el-input v-model="dataForm.number" placeholder="车位编号"></el-input>
    </el-form-item>
    <el-form-item label="车位位置" prop="section">
      <el-input v-model="dataForm.section" placeholder="车位位置"></el-input>
    </el-form-item>
    <el-form-item label="车位状态" prop="state">
      <el-input v-model="dataForm.state" placeholder="车位状态"></el-input>
    </el-form-item>
    <el-form-item label="车位类型(0:固定车位，1:出租车位，2:临时车位)" prop="typeId">
      <el-input v-model="dataForm.typeId" placeholder="车位类型(0:固定车位，1:出租车位，2:临时车位)"></el-input>
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
          number: '',
          section: '',
          state: '',
          typeId: ''
        },
        dataRule: {
          number: [
            { required: true, message: '车位编号不能为空', trigger: 'blur' }
          ],
          section: [
            { required: true, message: '车位位置不能为空', trigger: 'blur' }
          ],
          state: [
            { required: true, message: '车位状态不能为空', trigger: 'blur' }
          ],
          typeId: [
            { required: true, message: '车位类型(0:固定车位，1:出租车位，2:临时车位)不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/app/space/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.number = data.space.number
                this.dataForm.section = data.space.section
                this.dataForm.state = data.space.state
                this.dataForm.typeId = data.space.typeId
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
              url: this.$http.adornUrl(`/app/space/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'number': this.dataForm.number,
                'section': this.dataForm.section,
                'state': this.dataForm.state,
                'typeId': this.dataForm.typeId
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
