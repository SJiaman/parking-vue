<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="免费时长" prop="freeTime">
      <el-input v-model="dataForm.freeTime" placeholder="免费时长"></el-input>
    </el-form-item>
    <el-form-item label="计时单位" prop="timeUnit">
      <el-input v-model="dataForm.timeUnit" placeholder="计时单位"></el-input>
    </el-form-item>
    <el-form-item label="单位费用" prop="unitCost">
      <el-input v-model="dataForm.unitCost" placeholder="单位费用"></el-input>
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
          freeTime: '',
          timeUnit: '',
          unitCost: '',
          createTime: ''
        },
        dataRule: {
          freeTime: [
            { required: true, message: '免费时长不能为空', trigger: 'blur' }
          ],
          timeUnit: [
            { required: true, message: '计时单位不能为空', trigger: 'blur' }
          ],
          unitCost: [
            { required: true, message: '单位费用不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/pms/price/info/`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.freeTime = data.price.freeTime
                this.dataForm.timeUnit = data.price.timeUnit
                this.dataForm.unitCost = data.price.unitCost
                this.dataForm.createTime = data.price.createTime
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
              url: this.$http.adornUrl(`/pms/price/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'freeTime': this.dataForm.freeTime,
                'timeUnit': this.dataForm.timeUnit,
                'unitCost': this.dataForm.unitCost,
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
