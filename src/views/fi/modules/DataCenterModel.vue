<template>
  <div v-show="visible"
       style="margin:0;border:0;padding:0;">
    <a-row :gutter="24">
      <a-col :span="8">
        <a-form-item label="设施标识符"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <a-input v-decorator="['facilityDescriptor', validatorRules.facilityDescriptor]"
                   placeholder="请输入设施标识符"/>
        </a-form-item>
      </a-col>
      <a-col :span="8">
        <a-form-item label="设施名称"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <a-input v-decorator="['facilityName', validatorRules.facilityName]"
                   placeholder="请输入设施名称"/>
        </a-form-item>
      </a-col>
      <a-col :span="8">
        <a-form-item label="设施分类标识符"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <a-select  v-decorator="['facilityCategory',{rules: [{ required: true, message: '请选择设施分类标识符' }], initialValue: 'DC00000000'}]" placeholder="请选择设施分类标识符" >
            <a-select-option value="DC00000000" >DC00000000</a-select-option>
          </a-select>
        </a-form-item>
      </a-col>
    </a-row>
    <a-row :gutter="24">
      <a-col :span="8">
        <a-form-item label="设施投产日期"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <j-date
            v-decorator="[ 'facilityUseDate', validatorRules.facilityUseDate]"
            :trigger-change="true"
            :showTime="true"
            date-format="YYYY-MM-DD"
            style="width:100%"
            placeholder="请选择设施投产日期" >
          </j-date>
        </a-form-item>
      </a-col>
      <a-col :span="8">
        <a-form-item label="设施在用状态"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <a-select  v-decorator="['facilityUseState',{rules: [{ required: true, message: '请选择设施在用状态' }]}]" placeholder="请选择设施在用状态">
            <a-select-option value="00" >设施在用</a-select-option>
            <a-select-option value="01" >设施已停用</a-select-option>
            <a-select-option value="02" >设施专用于开发或测试</a-select-option>
            <a-select-option value="03" >设施已拆除或报废</a-select-option>
            <a-select-option value="04" >备用设施</a-select-option>
            <a-select-option value="99" >其它</a-select-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="8">
        <a-form-item label="金融机构编码"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <a-input v-decorator="['facilityOwnershipAgency', validatorRules.facilityOwnershipAgency ]"
                   placeholder="请输入金融机构编码" />
        </a-form-item>
      </a-col>
    </a-row>
    <a-row :gutter="24">
      <a-col :span="8">
        <a-form-item label="LEI全球法人机构识别编码"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <a-input v-decorator="['lei']"
                   placeholder="请输入LEI全球法人机构识别编码"/>
        </a-form-item>
      </a-col>
      <a-col :span="8">
        <a-form-item label="设施信息更新日期"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <j-date
            v-decorator="[ 'facilityUpdateDate', validatorRules.facilityUpdateDate]"
            :trigger-change="true"
            :showTime="true"
            date-format="YYYY-MM-DD"
            style="width:100%"
            placeholder="请选择设设施信息更新日期" >
          </j-date>
        </a-form-item>
      </a-col>
      <a-col :span="8">
        <a-form-item label="建设模式"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <a-select  v-decorator="['constructionMode',{rules: [{ required: true, message: '请选择建设模式' }]}]" placeholder="请选择建设模式">
            <a-select-option value="00" >自主建设</a-select-option>
            <a-select-option value="01" >租赁建设</a-select-option>
            <a-select-option value="02" >托管模式</a-select-option>
            <a-select-option value="03" >自主+租赁（机房场地租赁+机房内部自主建设）</a-select-option>
            <a-select-option value="99" >其它</a-select-option>
          </a-select>
        </a-form-item>
      </a-col>
    </a-row>
    <a-row :gutter="24">
      <a-col :span="24">
        <a-form-item label="备注信息"
                     :style="itemStyle"
                     :labelCol="labelColTextArea"
                     :wrapperCol="wrapperColTextArea">
          <a-textarea v-decorator="['remarksInformation', validatorRules.remarksInformation]"
                   placeholder="请输入备注信息"/>
        </a-form-item>
      </a-col>
    </a-row>
    <a-row :gutter="24">
      <a-col :span="8">
        <a-form-item label="国家地区"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <j-dict-select-tag
            placeholder="请选国家地区"
            :triggerChange="true"
            dictCode="data_type"
            v-decorator="['nationalArea', {rules: [{ required: true, message: '请选国家地区' }]}]"
          />
        </a-form-item>
      </a-col>
      <a-col :span="8">
        <a-form-item label="行政区划"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <j-dict-select-tag
            placeholder="请选接行政区划"
            :triggerChange="true"
            dictCode="data_type"
            v-decorator="['administrativeArea', validatorRules.administrativeArea]"
          />
        </a-form-item>
      </a-col>
      <a-col :span="8">
        <a-form-item label="地理位置"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <a-input v-decorator="['geographicalPosition', validatorRules.geographicalPosition ]"
                   placeholder="请输入地理位置" />
        </a-form-item>
      </a-col>
    </a-row>
    <a-row :gutter="24">
      <a-col :span="16">
        <a-form-item label="机房等级-国标标准"
                     :style="itemStyle"
                     :labelCol="labelColLongSelect"
                     :wrapperCol="wrapperColLongSelect">
          <a-select  v-decorator="['computerRoomGradeNationalStandard',{rules: [{ required: true, message: '请选择机房等级-国标标准' }]}]" placeholder="请选择设机房等级-国标标准">
            <a-select-option value="00" >高于GB50174-2017《数据中心设计规范机房等级标准》A级标准</a-select-option>
            <a-select-option value="01" >等同于GB50174-2017《数据中心设计规范机房等级标准》A级标准</a-select-option>
            <a-select-option value="02" >等同于GB50174-2017《数据中心设计规范机房等级标准》B级标准</a-select-option>
            <a-select-option value="03" >等同于GB50174-2017《数据中心设计规范机房等级标准》C级标准</a-select-option>
            <a-select-option value="04" >低于GB50174-2017《数据中心设计规范机房等级标准》C级标准</a-select-option>
            <a-select-option value="99" >其它</a-select-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="8">
        <a-form-item label="数据中心级别"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <a-select  v-decorator="['dbLevel',{rules: [{ required: true, message: '请选择数据中心级别' }]}]" placeholder="请选择数据中心级别">
            <a-select-option value="00" >总行、总部</a-select-option>
            <a-select-option value="01" >一级分行，省级分支机构</a-select-option>
            <a-select-option value="02" >二级分行，副省级分支机构</a-select-option>
            <a-select-option value="03" >三级分行，地市级分支机构</a-select-option>
            <a-select-option value="04" >四级分行，县区级分支机构</a-select-option>
            <a-select-option value="05" >海外分行</a-select-option>
            <a-select-option value="99" >其它</a-select-option>
          </a-select>
        </a-form-item>
      </a-col>
    </a-row>
    <a-row :gutter="24">
      <a-col :span="16">
        <a-form-item label="机房等级-Uptime TIER等级"
                     :style="itemStyle"
                     :labelCol="labelColLongSelect"
                     :wrapperCol="wrapperColLongSelect">
          <a-select  v-decorator="['computerRoomGradeUptimeTier',{rules: [{ required: true, message: '请选择机房等级-Uptime TIER等级' }]}]" placeholder="请选择机房等级-Uptime TIER等级">
            <a-select-option value="00" >高于 Uptime TIER 数据中心等级认证体系Tier IV级标准</a-select-option>
            <a-select-option value="01" >等同于 Uptime TIER 数据中心等级认证体系Tier IV级标准</a-select-option>
            <a-select-option value="02" >等同于 Uptime TIER 数据中心等级认证体系Tier III级标准</a-select-option>
            <a-select-option value="03" >等同于 Uptime TIER 数据中心等级认证体系Tier II级标准</a-select-option>
            <a-select-option value="04" >等同于 Uptime TIER 数据中心等级认证体系Tier I级标准</a-select-option>
            <a-select-option value="05" >低于 Uptime TIER 数据中心等级认证体系Tier I级标准</a-select-option>
            <a-select-option value="06" >未评级</a-select-option>
            <a-select-option value="99" >其它</a-select-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="8">
        <a-form-item label="功能定位"
                     :style="itemStyle"
                     :labelCol="labelCol"
                     :wrapperCol="wrapperCol">
          <a-select  v-decorator="['functionalOrientation',{rules: [{ required: true, message: '请选择功能定位' }]}]" placeholder="请选择功能定位">
            <a-select-option value="00" >主要生产中心</a-select-option>
            <a-select-option value="01" >同城备份中心</a-select-option>
            <a-select-option value="02" >同城多活中心</a-select-option>
            <a-select-option value="03" >异地备份中心</a-select-option>
            <a-select-option value="04" >异地多活中心</a-select-option>
            <a-select-option value="99" >其它</a-select-option>
          </a-select>
        </a-form-item>
      </a-col>
    </a-row>
    <div style="">
      <label><b>数据中心-机房面积</b></label>
      <a-row :gutter="24">
        <a-col :span="8">
          <a-form-item label="主机房使用面积(㎡)"
                       :labelCol="innelLabelCol"
                       :wrapperCol="innelWrapperCol">
            <a-input-number   :precision="2"  max="99999.99"  style="width: auto" v-decorator="['dataCenter_area.mainMachineRoomUsableArea', validatorRules.dataCenter_area.mainMachineRoomUsableArea]"
                     placeholder="请输入主机房使用面积"/>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="辅助区面积(㎡)"
                       :labelCol="labelCol"
                       :style="itemStyle"
                       :wrapperCol="wrapperCol">
            <a-input-number   :precision="2"  max="99999.99"  style="width: auto" v-decorator="['dataCenter_area.auxiliaryArea', validatorRules.dataCenter_area.auxiliaryArea]"
                              placeholder="请输入辅助区面积"/>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="支持区面积(㎡)"
                       :labelCol="labelCol"
                       :style="itemStyle"
                       :wrapperCol="wrapperCol">
            <a-input-number   :precision="2"  max="99999.99"  style="width: auto" v-decorator="['dataCenter_area.supportArea', validatorRules.dataCenter_area.supportArea]"
                              placeholder="请输入支持区面积"/>
          </a-form-item>
        </a-col>
      </a-row>
      <a-row :gutter="24">
        <a-col :span="8">
          <a-form-item label="行政管理区面积(㎡)"
                       :labelCol="innelLabelCol"
                       :wrapperCol="innelWrapperCol">
            <a-input-number   :precision="2"  max="99999.99"  style="width: auto" v-decorator="['dataCenter_area.administrativeArea', validatorRules.dataCenter_area.administrativeArea]"
                              placeholder="请输入行政管理区面积"/>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="建筑面积(㎡)"
                       :labelCol="labelCol"
                       :style="itemStyle"
                       :wrapperCol="wrapperCol">
            <a-input-number   :precision="2"  max="99999999.99"  style="width: auto" v-decorator="['dataCenter_area.constructionArea', validatorRules.dataCenter_area.constructionArea]"
                              placeholder="请输入建筑面积"/>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="园区面积(㎡)"
                       :labelCol="labelCol"
                       :style="itemStyle"
                       :wrapperCol="wrapperCol">
            <a-input-number   :precision="2"  max="99999999.99"  style="width: auto" v-decorator="['dataCenter_area.parkArea', validatorRules.dataCenter_area.parkArea]"
                              placeholder="请输入园区面积"/>
          </a-form-item>
        </a-col>
      </a-row>
    </div>

    <!--<a-form-item-->
    <!--  :labelCol="labelCol"-->
    <!--  :wrapperCol="wrapperCol"-->
    <!--  label="公司id">-->
    <!--  <a-input placeholder="请输入公司id" v-decorator="['companyId', {}]"/>-->
    <!--</a-form-item>-->
  </div>
</template>

<script>
  import {httpAction} from '@/api/manage';
  import {duplicateCheck} from '@/api/api';
  import JDictSelectTag from '@/components/dict/JDictSelectTag';
  import JDate from '@/components/jeecg/JDate.vue';
  import ATextarea from 'ant-design-vue/es/input/TextArea';

  export default {
    name: 'DataCenterModel',
    components: {ATextarea, JDictSelectTag,JDate},
    // props:['visible'],
    data() {
      return {
        title: '操作',
        visible: true,
        labelCol: {
          xs: {span: 4},
          sm: {span: 10}
        },
        wrapperCol: {
          sm: {span: 14}
        },
        innelLabelCol: {
          xs: {span: 12},
          sm: {span: 12}
        },
        innelWrapperCol: {
          xs: {span: 12},
          sm: {span: 12}
        },
        labelColLongSelect: {
          xs: {span: 6},
          sm: {span: 6}
        },
        wrapperColLongSelect: {
          xs: {span: 18},
          sm: {span: 18}
        },
        labelColTextArea: {
          xs: {span: 3},
        },
        wrapperColTextArea: {
          xs: {span: 3},
          sm: {span: 20}
        },
        itemStyle: {
          marginBottom: '0px',
          marginRight: '0px',
          'word-wrap': 'break-word',
        },
        confirmLoading: false,
        // form: this.$form.createForm(this),
        validatorRules: {
          facilityDescriptor: {rules: [{required: true, message: '设施标识符'}]},
          facilityName: {rules: [{required: true, message: '设施名称'}]},
          facilityUseDate: {rules: [{required: true, message: '设施投产日期'}]},
          facilityOwnershipAgency: {rules: [{required: true, message: '设施投产日期'}], initialValue: 'C1433532000014' },
          remarksInformation: {rules: [ { min: 1, max: 20, message: '长度不超过 4096 个字符', trigger: 'blur' }]},
          facilityUpdateDate: {rules: [{required: true, message: '设施信息更新日期'}]},
          geographicalPosition: {rules: [ {required: true, message: '地理位置'},{ min: 1, max: 128, message: '地理位置长度不超过 128 个字符', trigger: 'blur' }]},
          dataCenter_area: {
            mainMachineRoomUsableArea: {rules: [{required: true, message: '主机房使用面积'}]},
            auxiliaryArea: {rules: [{required: true, message: '辅助区面积'}]},
            supportArea: {rules: [{required: true, message: '支持区面积'}]},
            administrativeArea: {rules: [{required: true, message: '行政管理区面积'}]},
            constructionArea: {rules: [{required: true, message: '建筑面积'}]},
            parkArea: {rules: [{required: true, message: '园区面积'}]}
          }
        }
      };
    },
    created() {
    },
    methods: {
    }
  };
</script>

<style lang="less" scoped>

</style>
