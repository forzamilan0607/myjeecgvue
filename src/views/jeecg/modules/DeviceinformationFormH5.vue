<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form :form="form" slot="detail">
    <a-card title="基本信息" style="margin-left:1%;margin-right:1%;">      
      
        <table class="mytbl">
          <tr>
            <td style="width:30%;">设备编号</td><td>{{model.code}}</td>            
          </tr>
          <tr>
            <td >设备名称</td><td>{{model.name}}</td>
          </tr>
          <tr>
            <td >规格型号</td><td>{{model.model}}</td>
          </tr>
          <tr>
            <td >放置地点</td><td>{{model.placementlocation}}</td>
          </tr>
          <tr>
            <td >仪器设备</td><td>{{model.instrumentandequipment}}</td>
          </tr>
           <!-- <tr>
            <td >单价</td><td>{{model.unitprice}}</td>
          </tr> -->
        </table>
      
    </a-card>
    
    <a-card title="设备主要负责人" style="margin-left:1%;margin-right:1%;">
      <table class="mytbl">
          <tr>
            <td style="width:30%;">保养人</td><td>
	 		<a-form-item> 
		      <a-input v-decorator="['maintainer']" placeholder="请输入设备主要负责人"  ></a-input>
		  </a-form-item>  </td>            
          </tr>     
        </table>      
    </a-card>
	
	    <a-card title="校验类型（自校）" style="margin-left:2%;margin-right:2%;">
      <table class="mytbl">
         <tr>
            <td>类型</td>
            <td> 
			<a-form-item> 
		      <a-input v-decorator="['selfcalibration']" placeholder="请输入（自校）类型"  ></a-input>
		  </a-form-item>  
            </td>  
          </tr>	 
  <tr>
            <td>校验证书</td>
            <td>
               <!-- <a-form-item>
                 <j-image-upload isMultiple  v-decorator="['selfcalibrationimgs']" ></j-image-upload>
               </a-form-item> -->
               <a-form-item>
                 <j-upload v-decorator="['selfcalibrationimgs']" :trigger-change="true"  ></j-upload>
               </a-form-item>
               <div style="height:40px;"></div>
            </td>  
          </tr>		
     <tr>
            <td style="width:30%;">上次检测日期</td>
            <td>
              <a-form-item>
                <j-date placeholder="请选择上次检测日期" v-decorator="['instrumenttestdate', validatorRules.instrumenttestdate]" :trigger-change="true" :show-time="false" date-format="YYYY-MM-DD" style="width: 100%" />
              </a-form-item>
            </td>                      
          </tr>
          <tr>
            <td>本次检测日期</td>
            <td>
              <a-form-item>
                <j-date placeholder="请选择本次检测日期" v-decorator="['instrumenttestdatenew', validatorRules.instrumenttestdatenew]" :trigger-change="true" :show-time="false" date-format="YYYY-MM-DD" style="width: 100%" />
              </a-form-item>
            </td>  
          </tr>
		   <tr>
            <td >检测周期</td><td>{{model.othercalibrationcycle}}</td>
          </tr>
        </table>   
    </a-card>
	
	    <a-card title="校验类型（外校）" style="margin-left:1%;margin-right:1%;">
      <table class="mytbl">
         
        <tr>
            <td>类型</td>
            <td> 
			<a-form-item> 
		      <a-input v-decorator="['othercalibration']" placeholder="请输入（外校）类型"  ></a-input>
		  </a-form-item>  
            </td>  
          </tr>	 
		 <tr>
            <td>校验证书</td>
            <td>
               <!-- <a-form-item> 			   
			            <j-image-upload isMultiple  v-decorator="['othercalibrationimgs']" ></j-image-upload>                  
               </a-form-item> -->
               <a-form-item>
                 <j-upload v-decorator="['othercalibrationimgs']" :trigger-change="true"  ></j-upload>
               </a-form-item>
               <div style="height:40px;"></div>
            </td>  
          </tr>	 
		    <tr>
            <td style="width:30%;">上次检测日期</td>
            <td>
              <a-form-item>
                <j-date placeholder="请选择上次检测日期" v-decorator="['instrumenttestdate', validatorRules.instrumenttestdate]" :trigger-change="true" :show-time="false" date-format="YYYY-MM-DD" style="width: 100%" />
              </a-form-item>
            </td>                      
          </tr>
          <tr>
            <td>本次检测日期</td>
            <td>
              <a-form-item>
                <j-date placeholder="请选择本次检测日期" v-decorator="['nexttestdatenew', validatorRules.nexttestdatenew]" :trigger-change="true" :show-time="false" date-format="YYYY-MM-DD" style="width: 100%" />
              </a-form-item>
            </td>  
          </tr>
		 <tr>
            <td >检测周期</td><td>
			<a-form-item> 
		      <a-input v-decorator="['othercalibrationcycle']" placeholder="请输入保养周期"  ></a-input>
		  </a-form-item>
            </td>
          </tr>
        </table>      
    </a-card>
	
    <a-card title="维护保养周期" style="margin-left:1%;margin-right:1%;">
      <!-- <j-form-container :disabled="formDisabled">
      <a-form :form="form" slot="detail"> -->
      <table class="mytbl">
          <tr>
            <td style="width:30%;">保养周期</td>
            <td>
              <a-form-item> 
                  <a-input v-decorator="['maintenancecycle']" placeholder="请输入保养周期"  ></a-input>
              </a-form-item>
            </td>                      
          </tr>
          <tr>
            <td>维护日期</td>
            <td>
              <a-form-item>
                <j-date placeholder="请选择维护日期" v-decorator="['maintenancedate', validatorRules.maintenancedate]" :trigger-change="true" :show-time="false" date-format="YYYY-MM-DD" style="width: 100%" />
              </a-form-item>
            </td>  
          </tr>
          <tr>
            <td>状态</td>
            <td>
              <a-form-item>
                <a-select :getPopupContainer="(triggerNode)=>{ return triggerNode.parentNode}" v-decorator="['states', validatorRules.states]"  placeholder="请选择设备状态" >
                  <a-select-option v-for="item in devStatus" :key="item.value">{{item.text}}</a-select-option>
                </a-select>
              </a-form-item>
            </td>  
          </tr>
          <tr>
            <td>操作规程</td>
            <td>
              <!-- <a :href="model.descriptionString" download="model.descriptionString">{{model.descriptionString}}</a> 
              <span v-if="!model.description" style="font-size: 12px;font-style: italic;">无文件</span>              
              <a-button
                v-else
                :ghost="true"
                type="primary"
                icon="download"
                size="small"
                @click="downloadFile(model.description)">
                下载
              </a-button>
              -->
              <a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol">
                <j-upload v-decorator="['description']" :trigger-change="true" />
              </a-form-item>
              <div style="height:40px;"></div>
            </td>  
          </tr>
          <tr>
            <td>维护保养记录</td>
            <td>
			          <!-- 
               <a-form-item> 
                 <j-image-upload isMultiple  v-decorator="['maintenanceimg']" ></j-image-upload>
               </a-form-item>
                -->			   
               <a-form-item :labelCol="labelCol" :wrapperCol="wrapperCol">
                 <j-upload v-decorator="['maintenanceimg']" :trigger-change="true"  ></j-upload>
               </a-form-item>
               <div style="height:40px;"></div>
            </td>  
          </tr>
          <tr>
            <td></td>
            <td>
              <a-li style="text-align: center">
                  <a-button @click="submitForm">提 交</a-button>
              </a-li>
            </td>
          </tr>
      </table>   
    </a-card>
      </a-form>
    </j-form-container>
  </a-spin>
</template>

<script>

  import { httpAction, getAction, downFile } from '@/api/manage'
  import pick from 'lodash.pick'
  import { validateDuplicateValue } from '@/utils/util'
  import JFormContainer from '@/components/jeecg/JFormContainer'
  import JDate from '@/components/jeecg/JDate'  
  import JUpload from '@/components/jeecg/JUpload'
  import JImageUpload from '@/components/jeecg/JImageUpload'

  export default {
    name: 'DeviceinformationFormH5',
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
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24},
          sm: { span: 16 },
        },
        confirmLoading: false,
        validatorRules: {
         /*  code: {
            rules: [
              { required: false},
              { validator: (rule, value, callback) => validateDuplicateValue('deviceinformation', 'code', value, this.model.id, callback)},
            ]
          },quantity: {
            rules: [
              { required: false},
              { pattern: /^-?\d+$/, message: '请输入整数!'},
            ]
          } */
          instrumenttestdate: {
            rules: [
              { required: true, message: '请选择仪器检测日期!'},
            ]
          },
          nexttestdate: {
            rules: [
              { required: true, message: '请选择仪器检测日期!'},
            ]
          },
          states: {
            rules: [
              { required: true, message: '请选择状态!'},
            ]
          }
                   
        },
        url: {
          add: "/device/deviceinformation/add",
          edit: "/device/deviceinformation/edit2",
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
      downloadFile(url, fileName, parameter) {        
        return downFile(url, parameter).then((data) => {
          if (!data || data.size === 0) {
            Vue.prototype['$message'].warning('文件下载失败')
            return
          }
          if (typeof window.navigator.msSaveBlob !== 'undefined') {
            window.navigator.msSaveBlob(new Blob([data]), fileName)
          } else {
            let url = window.URL.createObjectURL(new Blob([data]))
            let link = document.createElement('a')
            link.style.display = 'none'
            link.href = url
            link.setAttribute('download', fileName)
            document.body.appendChild(link)
            link.click()
            document.body.removeChild(link) //下载完成移除元素
            window.URL.revokeObjectURL(url) //释放掉blob对象
          }
        })
      },
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
         var search=location.search;
         var idValue = search.replace("?id=", "");

          let params = {id: idValue};          
          getAction(this.url.queryById,params).then((res)=>{
            if(res.success){
              this.edit (res.result);
            }
          });

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
        this.form.setFieldsValue(pick(row,'code','name','model','qrcodeString','manufacturer','manufacturercode','manufacturerdate','commissiondate','quantity','unitprice','maintainer','instrunentdept','placementlocation','instrumenttestdate','instrumenttestdatenew','nexttestdate','description','instrumentandequipment','naturecategory','importancecategory','states','remarks'))
      },
    }
  }
</script>
<style>
.mytbl {
  width: 100%;
  border-collapse: collapse;
}
.mytbl td {
  color:rgba(0, 0, 0, 0.65);
  border: 1px solid #d9d9d9;
  border-radius: 4px;
  padding: 5px;
  word-break: break-all;
}
.ant-form-item {
  margin-bottom: 1px;
}
img {
  width: 100%;
  height: 100%;
}
.ant-upload-list-item-name {
    display: inline-block;
    width: 100%;
    padding-left: 22px;
    overflow: hidden;
    white-space: pre-wrap;
    text-overflow: ellipsis;
}
</style>