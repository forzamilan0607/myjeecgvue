<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form :form="form" slot="detail">
        <a-row>
          <a-col :span="12">
            <a-form-item label="设备名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['name', validatorRules.name]" placeholder="请输入设备名称"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="规格型号" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['model']" placeholder="请输入规格型号"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="状态" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <!-- <a-input v-decorator="['states']" placeholder="请输入状态"  ></a-input> -->
              <a-select :getPopupContainer="(triggerNode)=>{ return triggerNode.parentNode}" v-decorator="['states', validatorRules.states]"  placeholder="请选择设备状态" >
                <a-select-option v-for="item in devStatus" :key="item.value">{{item.text}}</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="设备编号" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['code', validatorRules.code]" placeholder="请输入设备编号"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="生产厂家" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['manufacturer']" placeholder="请输入生产厂家"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="二维码" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-image-upload isMultiple text="" disabled="true" v-decorator="['qrcodeStringUrl']" ></j-image-upload>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="出产编号" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['manufacturercode']" placeholder="请输入出产编号"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="出产日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择出产日期" v-decorator="['manufacturerdate']" :trigger-change="true" :show-time="false" date-format="YYYY-MM-DD" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="投产日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择投产日期" v-decorator="['commissiondate']" :trigger-change="true" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="单价" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input-number v-decorator="['unitprice', validatorRules.unitprice]" placeholder="请输入单价" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="仪器负责人" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['maintainer']" placeholder="请输入仪器负责人"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="仪器使用部门" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['instrunentdept']" placeholder="请输入仪器使用部门"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="放置地点" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['placementlocation']" placeholder="请输入放置地点"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="操作规程" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-upload v-decorator="['description']" :trigger-change="true"  ></j-upload>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="自校类型" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['selfcalibration']" placeholder="请输入自校类型"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="自校周期" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['selfcalibrationcycle']" placeholder="请输入自校周期"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="自校日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择自校日期" v-decorator="['instrumenttestdate']" :trigger-change="true" :show-time="false" date-format="YYYY-MM-DD" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="下次自校日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择下次自校日期" v-decorator="['instrumenttestdatenew']" :trigger-change="true" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="自校证书" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-image-upload isMultiple  v-decorator="['selfcalibrationimgs']" ></j-image-upload>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="外校类型" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['othercalibration']" placeholder="请输入类型"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="外校周期" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['othercalibrationcycle']" placeholder="请输入外校周期"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="外校日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择外校日期" v-decorator="['nexttestdate']" :trigger-change="true" :show-time="false" date-format="YYYY-MM-DD" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="外校下次日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择外校下次日期" v-decorator="['nexttestdatenew']" :trigger-change="true" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="外校证书" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-image-upload isMultiple  v-decorator="['othercalibrationimgs']" ></j-image-upload>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="维护保养周期" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['maintenancecycle']" placeholder="请输入维护保养周期"  ></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="维保日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-date placeholder="请选择维护保养日期" v-decorator="['maintenancedate']" :trigger-change="true" :show-time="false" date-format="YYYY-MM-DD" style="width: 100%" />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="维保记录" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-image-upload isMultiple  v-decorator="['maintenanceimg']" ></j-image-upload>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-textarea v-decorator="['remarks']" rows="4" placeholder="请输入备注" />
            </a-form-item>
          </a-col>
          <a-col v-if="showFlowSubmitButton" :span="24" style="text-align: center">
            <a-button @click="submitForm">提 交</a-button>
          </a-col>
        </a-row>
      </a-form>
    </j-form-container>
  </a-spin>
</template>

<script>

  import { httpAction, getAction } from '@/api/manage'
  import pick from 'lodash.pick'
  import { validateDuplicateValue } from '@/utils/util'
  import JFormContainer from '@/components/jeecg/JFormContainer'
  import JDate from '@/components/jeecg/JDate'  
  import JUpload from '@/components/jeecg/JUpload'
  import JImageUpload from '@/components/jeecg/JImageUpload'

  export default {
    name: 'DeviceinformationForm',
    components: {
      JFormContainer,
      JDate,
      JUpload,
      JImageUpload,
    },
    props: {
      //流程表单data
      formData: {
        type: Object,
        default: ()=>{},
        required: false
      },
      //表单模式：true流程表单 false普通表单
      formBpm: {
        type: Boolean,
        default: false,
        required: false
      },
      //表单禁用
      disabled: {
        type: Boolean,
        default: false,
        required: false
      }
    },
    data () {
      return {
        form: this.$form.createForm(this),
        model: {},
        devStatus: [{text:'完好',value:'完好'}, {text:'损坏',value:'损坏'}, {text:'报修',value:'报修'}],
        labelCol: {
          xs: { span: 24 },
          sm: { span: 7 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        validatorRules: {
          name: {
            rules: [
              { required: true, message: '请输入设备名称!'},
            ]
          },
          code: {
            rules: [
              { required: false},
              { validator: (rule, value, callback) => validateDuplicateValue('deviceinformation', 'code', value, this.model.id, callback)},
            ]
          },
          unitprice: {
            rules: [
              { required: false},
              { pattern: /^(([1-9][0-9]*)|([0]\.\d{0,2}|[1-9][0-9]*\.\d{0,2}))$/, message: '请输入正确的金额!'},
            ]
          },
        },
        url: {
          add: "/device/deviceinformation/add",
          edit: "/device/deviceinformation/edit",
          queryById: "/device/deviceinformation/queryById"
        }
      }
    },
    computed: {
      formDisabled(){
        if(this.formBpm===true){
          if(this.formData.disabled===false){
            return false
          }
          return true
        }
        return this.disabled
      },
      showFlowSubmitButton(){
        if(this.formBpm===true){
          if(this.formData.disabled===false){
            return true
          }
        }
        return false
      }
    },
    created () {
      //如果是流程中表单，则需要加载流程表单data
      this.showFlowData();
    },
    methods: {
      add () {
        this.edit({});
      },
      edit (record) {
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'name','model','states','code','manufacturer','qrcodeStringUrl','manufacturercode','manufacturerdate','commissiondate','unitprice','maintainer','instrunentdept','placementlocation','description','selfcalibration','selfcalibrationcycle','instrumenttestdate','instrumenttestdatenew','selfcalibrationimgs','othercalibration','othercalibrationcycle','nexttestdate','nexttestdatenew','othercalibrationimgs','maintenancecycle','maintenancedate','maintenanceimg','remarks'))
        })
      },
      //渲染流程表单数据
      showFlowData(){
        if(this.formBpm === true){
          let params = {id:this.formData.dataId};
          getAction(this.url.queryById,params).then((res)=>{
            if(res.success){
              this.edit (res.result);
            }
          });
        }
      },
      submitForm () {
        const that = this;
        // 触发表单验证
        this.form.validateFields((err, values) => {
          if (!err) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            let formData = Object.assign(this.model, values);
            console.log("表单提交数据",formData)
            httpAction(httpurl,formData,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
            })
          }
         
        })
      },
      popupCallback(row){
        this.form.setFieldsValue(pick(row,'name','model','states','code','manufacturer','qrcodeStringUrl','manufacturercode','manufacturerdate','commissiondate','unitprice','maintainer','instrunentdept','placementlocation','description','selfcalibration','selfcalibrationcycle','instrumenttestdate','instrumenttestdatenew','selfcalibrationimgs','othercalibration','othercalibrationcycle','nexttestdate','nexttestdatenew','othercalibrationimgs','maintenancecycle','maintenancedate','maintenanceimg','remarks'))
      },
    }
  }
</script>