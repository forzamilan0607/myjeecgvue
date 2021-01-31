<template>
  <a-spin :spinning="confirmLoading">
    <a-card title="基本信息" style="margin-left:2%;margin-right:2%;">      
      
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
            <td >仪器，设备</td><td>{{model.instrumentandequipment}}</td>
          </tr>
          
         <!--  <tr>
            <td >生产厂家</td><td>{{model.manufacturer}}</td>
          </tr>
          <tr>
            <td >出产编号</td><td>{{model.manufacturercode}}</td>
          </tr>
          <tr>
            <td >出产日期</td><td>{{model.manufacturerdate}}</td>
          </tr>
          <tr>
            <td >投产日期</td><td>{{model.commissiondate}}</td>
          </tr>
          <tr>
            <td >数量</td><td>{{model.quantity}}</td>
          </tr> -->
          <tr>
            <td >单价</td><td>{{model.unitprice}}</td>
          </tr>
        </table>
      
    </a-card>
    <a-card title="设备主要负责人" style="margin-left:5%;margin-right:5%;">
      <table class="mytbl">
          <tr>
            <td style="width:30%;">保养人</td><td>{{model.maintainer}}</td>            
          </tr>
          <!-- <tr>
            <td >仪器部门</td><td>{{model.instrunentdept}}</td>
          </tr> -->
          
          <tr>
            <td >性质类别</td><td>{{model.naturecategory}}</td>
          </tr>
          <tr>
            <td >重要性类别</td><td>{{model.importancecategory}}</td>
          </tr>         
          <!-- <tr>
            <td >出产编号</td><td>{{model.manufacturercode}}</td>
          </tr>   -->       
        </table>      
    </a-card>
    <a-card title="检测登记" style="margin-left:5%;margin-right:5%;">
      <j-form-container :disabled="formDisabled">
      <a-form :form="form" slot="detail">
      <table class="mytbl">
          <tr>
            <td style="width:30%;">仪器检测日期</td>
            <td>
              <a-form-item>
                <j-date placeholder="请选择仪器检测日期" v-decorator="['instrumenttestdate', validatorRules.instrumenttestdate]" :trigger-change="true" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
              </a-form-item>
            </td>                      
          </tr>
          <tr>
            <td>下次检测日期</td>
            <td>
              <a-form-item>
                <j-date placeholder="请选择下次检测日期" v-decorator="['nexttestdate', validatorRules.nexttestdate]" :trigger-change="true" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
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
            <td>说明书</td>
            <td>
              <!-- <a :href="model.descriptionString" download="model.descriptionString">{{model.descriptionString}}</a> -->
              <span v-if="!model.descriptionString" style="font-size: 12px;font-style: italic;">无文件</span>              
              <a-button
                v-else
                :ghost="true"
                type="primary"
                icon="download"
                size="small"
                @click="downloadFile(model.descriptionString)">
                下载
              </a-button>
            </td>  
          </tr>
          <tr>
            <td>维护保养记录</td>
            <td>
               <a-form-item> 
			   <div v-html="model.maintenanceimgh5"/>
			   <j-image-upload isMultiple  v-decorator="['maintenanceimg']" ></j-image-upload>
               </a-form-item>
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
      </a-form>
    </j-form-container>   
    </a-card>   
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
          this.form.setFieldsValue(pick(this.model,'code','name','model','qrcodeString','manufacturer','manufacturercode','manufacturerdate','commissiondate','quantity','unitprice','maintainer','instrunentdept','placementlocation','instrumenttestdate','nexttestdate','descriptionString','instrumentandequipment','naturecategory','importancecategory','states','remarks'))
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
        this.form.setFieldsValue(pick(row,'code','name','model','qrcodeString','manufacturer','manufacturercode','manufacturerdate','commissiondate','quantity','unitprice','maintainer','instrunentdept','placementlocation','instrumenttestdate','nexttestdate','descriptionString','instrumentandequipment','naturecategory','importancecategory','states','remarks'))
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
  padding: 7px;
}
.ant-form-item {
  margin-bottom: 1px;
}
img {
  width: 100%;
  height: 100%;
}
</style>