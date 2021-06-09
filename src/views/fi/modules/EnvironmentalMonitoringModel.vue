<template >
    <div :spinning="confirmLoading" v-show="visible">
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="1职务编码">
          <a-input placeholder="请输入职务编码" v-decorator="['code', validatorRules.code]"/>
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="1职务名称">
          <a-input placeholder="请输入职务名称" v-decorator="['name', validatorRules.name]"/>
        </a-form-item>
        <!--<a-form-item-->
        <!--  :labelCol="labelCol"-->
        <!--  :wrapperCol="wrapperCol"-->
        <!--  label="公司id">-->
        <!--  <a-input placeholder="请输入公司id" v-decorator="['companyId', {}]"/>-->
        <!--</a-form-item>-->
    </div>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import { duplicateCheck } from '@/api/api'
  import JDictSelectTag from '@/components/dict/JDictSelectTag'


  export default {
    name: 'EnvironmentalMonitoringModel',
    components: { JDictSelectTag },
    // props:['visible'],
    data() {
      return {
        title: '操作',
        visible: true,
        model: {},
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        form: this.$form.createForm(this),
        validatorRules: {
          name: { rules: [{ required: true, message: '请输入职务名称' }] },
          postRank: { rules: [{ required: true, message: '请选择接口类型' }] },
        }
      }
    },
    created() {
    },
    methods: {}
  }
</script>

<style lang="less" scoped>

</style>