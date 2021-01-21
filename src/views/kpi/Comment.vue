<template>
  <a-table :columns="columns" :data-source="dataSource"  bordered>
    <template slot="name" slot-scope="text">
      <a>{{ text }}</a>
    </template>
    <template slot="inputNumber" slot-scope="text,record" >
        <a-input-number  v-model="record.rate" :min="1" :max="20" @change="onChange(record)" />
        {{ checkLevel(record.rate)  }}
    </template>
    <template slot="action" slot-scope="text,record" >
      <a-button type="primary" @click="onSubmit(record)" >
        提交
      </a-button>
    </template>
    <template slot="status" slot-scope="text,record">
      <a-tag color="pink">
        {{getLabel(record.rate)}}
      </a-tag>
    </template>
  </a-table>
</template>
<script>
  // In the fifth row, other columns are merged into first column
  // by setting it's colSpan to be 0
  const renderContent = (value, row, index) => {
    const obj = {
      children: value,
      attrs: {},
    };
    if (index === 4) {
      obj.attrs.colSpan = 0;
    }
    return obj;
  };


  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  export default {
    mixins: [JeecgListMixin],
    data() {
      const columns = [
        {
          title: '姓名',
          width:'8%',
          dataIndex: 'realname'
        },
        {
          title: '部门',
          width:'10%',
          dataIndex: 'departName',
        },
        {
          title: '职务',
          width:'10%',
          dataIndex: 'post',
        },
        {
          title: '评价科目(评分说明: 优秀[18-20] ; 良好[15-17] ; 合格[12-14] ; 不合格[1-11] )',
          children: [
            {
              title: '德',
              dataIndex: 'rate',
              key: 'rate',
              width: 100,
              scopedSlots: { customRender: 'inputNumber' }
            },
            {
              title: '勤',
              dataIndex: 'post2',
              key: 'post2',
              width: 100,
              scopedSlots: { customRender: 'inputNumber' }
            },
            ,
            {
              title: '能',
              dataIndex: 'number1',
              key: 'number1',
              width: 100,
              scopedSlots: { customRender: 'inputNumber' }
            },
            ,
            {
              title: '绩',
              dataIndex: 'number2',
              key: 'number2',
              width: 100,
              scopedSlots: { customRender: 'inputNumber' }
            },
            {
              title: '廉',
              dataIndex: 'number3',
              key: 'number3',
              width: 100,
              scopedSlots: { customRender: 'inputNumber' }
            },
          ],
          colSpan: 0,
          dataIndex: 'realname2',
          customRender: (value, row, index) => {
            const obj = {
              children: value,
              attrs: {},
            };
            if (index === 2) {
              obj.attrs.rowSpan = 2;
            }
            // These two are merged into above cell
            if (index === 3) {
              obj.attrs.rowSpan = 0;
            }
            if (index === 4) {
              obj.attrs.colSpan = 0;
            }
            return obj;
          }
        },
        {
          title: '状态',
          width:'5%',
          scopedSlots: { customRender: 'status' }
        }
        ,
        {
          title: '操作',
          width:'5%',
          scopedSlots: { customRender: 'action' }
        }
      ];
      return {
        columns,
        url: {
          list: '/kpi/evaluationResult/queryByOrgCodeForAddressList'
        }
      };
    },
    methods: {
      onChange(record) {
        console.log('changed', record);
      },
      onSubmit(record) {
        console.log('changed', record);
        alert(record.rate)
      },
      checkLevel(value){
        if(value <= 11 && value >=0){
          return "不合格";
        }
        if(value <= 14 && value >=11){
          return "合格";
        }
        if(value <= 17 && value >=15){
          return "良好";
        }
        if(value >= 18){
          return "优秀";
        }
        return "";
      },
      getLabel(status) {
        if(status == 1){
          return "已提交";
        }
        return "未提交";
      }
    }
  };
</script>
<style lang="less">
  th {
    font-size: 15px;
  }
  tr {
    font-size: 12px;
  }
  tr.column-money {
    text-align: right !important;
  }
</style>