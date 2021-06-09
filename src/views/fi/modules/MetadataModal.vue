<template>
  <a-modal
    :title="title"
    width="90%"
    :visible="visible"
    :maskClosable="false"
    :confirmLoading="confirmLoading"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">

    <a-spin :spinning="confirmLoading">
      <a-form :form="form"  >
        <a-row :gutter="24">
          <a-col>
            <a-form-item
              :labelCol="labelCol"
              :wrapperCol="wrapperCol"
              style="width: 100%;margin-left: 0px;"
              label="接口类型"
            >
              <j-dict-select-tag
                placeholder="请选接口类型"
                :triggerChange="true"
                dictCode="data_type"
                v-decorator="['postRank', validatorRules.postRank]"
              />
            </a-form-item>
          </a-col>
        </a-row>
        <DataCenterModel  v-if="'dataCenter' == this.form.getFieldValue('postRank')"></DataCenterModel>
        <EnvironmentalMonitoringModel v-if="'environmentalMonitoring' == this.form.getFieldValue('postRank')"></EnvironmentalMonitoringModel>
      </a-form>
    </a-spin>
  </a-modal>
</template>

<script>
  import pick from 'lodash.pick'
  import { httpAction } from '@/api/manage'
  import { duplicateCheck } from '@/api/api'
  import JDictSelectTag from '@/components/dict/JDictSelectTag'
  import DataCenterModel from './DataCenterModel'
  import EnvironmentalMonitoringModel from './EnvironmentalMonitoringModel'


  export default {
    name: 'MetadataModal',
    components: { JDictSelectTag , DataCenterModel,EnvironmentalMonitoringModel},
    data() {
      return {
        title: '操作',
        visible: false,
        model: {},
        labelCol: {
          xs: { span: 3 },
          sm: { span: 3 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 20 },
        },
        confirmLoading: false,
        form: this.$form.createForm(this),
        validatorRules: {
          postRank: { rules: [{ required: true, message: '请选择接口类型' }] },
        },
        url: {
          add: '/sys/position/add',
          edit: '/sys/position/edit',
        },
      }
    },
    created() {
    },
    methods: {
      add() {
        this.edit({})
      },
      edit(record) {
        this.form.resetFields()
        this.model = Object.assign({}, record)
        this.visible = true
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,
            'code',
            'name',
            'postRank',
            // 'companyId'
          ))
        })
      },
      close() {
        this.$emit('close')
        this.visible = false
      },
      handleOk() {
        const that = this
        // 触发表单验证
        this.form.validateFields((err, values) => {
          console.log(values)
          if (!err) {
            that.confirmLoading = true
            let httpurl = ''
            let method = ''
            if (!this.model.id) {
              httpurl += this.url.add
              method = 'post'
            } else {
              httpurl += this.url.edit
              method = 'put'
            }

            let formData = Object.assign(this.model, values)
            httpAction(httpurl, formData, method).then((res) => {
              if (res.success) {
                that.$message.success(res.message)
                that.$emit('ok')
              } else {
                that.$message.warning(res.message)
              }
            }).finally(() => {
              that.confirmLoading = false
              that.close()
            })
          }
        })
      },
      handleCancel() {
        this.close()
      },


    }
  }
</script>

<style lang="less" scoped>

</style>