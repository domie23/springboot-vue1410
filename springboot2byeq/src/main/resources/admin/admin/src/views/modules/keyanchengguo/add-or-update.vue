<template>
	<div class="addEdit-block" :style='{"padding":"0"}' style="width: 100%;">
		<el-form
			:style='{"width":"90%","padding":"30px","margin":"0 auto","borderRadius":"6px","background":"#fff"}'
			class="add-update-preview"
			ref="ruleForm"
			:model="ruleForm"
			:rules="rules"
			label-width="140px"
		>
			<template >
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' class="input" v-if="type!='info'"  label="成果名称" prop="chengguomingcheng">
					<el-input v-model="ruleForm.chengguomingcheng" placeholder="成果名称" clearable  :readonly="ro.chengguomingcheng"></el-input>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' v-else class="input" label="成果名称" prop="chengguomingcheng">
					<el-input v-model="ruleForm.chengguomingcheng" placeholder="成果名称" readonly></el-input>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' class="select" v-if="type!='info'"  label="队伍名称" prop="duiwumingcheng">
					<el-select :disabled="ro.duiwumingcheng" v-model="ruleForm.duiwumingcheng" placeholder="请选择队伍名称" >
						<el-option
							v-for="(item,index) in duiwumingchengOptions"
							v-bind:key="index"
							:label="item"
							:value="item">
						</el-option>
					</el-select>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' v-else class="input" label="队伍名称" prop="duiwumingcheng">
					<el-input v-model="ruleForm.duiwumingcheng"
						placeholder="队伍名称" readonly></el-input>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' class="select" v-if="type!='info'"  label="成果类型" prop="chengguoleixing">
					<el-select :disabled="ro.chengguoleixing" v-model="ruleForm.chengguoleixing" placeholder="请选择成果类型" >
						<el-option
							v-for="(item,index) in chengguoleixingOptions"
							v-bind:key="index"
							:label="item"
							:value="item">
						</el-option>
					</el-select>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' v-else class="input" label="成果类型" prop="chengguoleixing">
					<el-input v-model="ruleForm.chengguoleixing"
						placeholder="成果类型" readonly></el-input>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' class="upload" v-if="type!='info'&& !ro.chengguofujian" label="成果附件" prop="chengguofujian">
					<file-upload
						tip="点击上传成果附件"
						action="file/upload"
						:limit="1"
						:multiple="true"
						:fileUrls="ruleForm.chengguofujian?ruleForm.chengguofujian:''"
						@change="chengguofujianUploadChange"
					></file-upload>
				</el-form-item>  
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' v-else-if="ruleForm.chengguofujian" label="成果附件" prop="chengguofujian">
					<el-button :style='{"border":"0","cursor":"pointer","padding":"0 15px","margin":"0 20px 0 0","outline":"none","color":"rgba(255, 255, 255, 1)","borderRadius":"4px","background":"rgba(121, 121, 121, 1)","width":"auto","lineHeight":"40px","fontSize":"14px","height":"40px"}' type="text" size="small" @click="download($base.url+ruleForm.chengguofujian)">下载</el-button>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' v-else-if="!ruleForm.chengguofujian" label="成果附件" prop="chengguofujian">
					<el-button :style='{"border":"0","cursor":"pointer","padding":"0 15px","margin":"0 20px 0 0","outline":"none","color":"rgba(255, 255, 255, 1)","borderRadius":"4px","background":"rgba(121, 121, 121, 1)","width":"auto","lineHeight":"40px","fontSize":"14px","height":"40px"}' type="text" size="small">无</el-button>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' class="upload" v-if="type!='info' && !ro.fengmian" label="封面" prop="fengmian">
					<file-upload
						tip="点击上传封面"
						action="file/upload"
						:limit="3"
						:multiple="true"
						:fileUrls="ruleForm.fengmian?ruleForm.fengmian:''"
						@change="fengmianUploadChange"
					></file-upload>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' class="upload" v-else-if="ruleForm.fengmian" label="封面" prop="fengmian">
					<img v-if="ruleForm.fengmian.substring(0,4)=='http'" class="upload-img" style="margin-right:20px;" v-bind:key="index" :src="ruleForm.fengmian.split(',')[0]" width="100" height="100">
					<img v-else class="upload-img" style="margin-right:20px;" v-bind:key="index" v-for="(item,index) in ruleForm.fengmian.split(',')" :src="$base.url+item" width="100" height="100">
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' class="date" v-if="type!='info'" label="上传日期" prop="shangchuanriqi">
					<el-date-picker
						format="yyyy 年 MM 月 dd 日"
						value-format="yyyy-MM-dd"
						v-model="ruleForm.shangchuanriqi" 
						type="date"
						:readonly="ro.shangchuanriqi"
						placeholder="上传日期"
					></el-date-picker> 
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' class="input" v-else-if="ruleForm.shangchuanriqi" label="上传日期" prop="shangchuanriqi">
					<el-input v-model="ruleForm.shangchuanriqi" placeholder="上传日期" readonly></el-input>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' class="input" v-if="type!='info'"  label="队伍账号" prop="duiwuzhanghao">
					<el-input v-model="ruleForm.duiwuzhanghao" placeholder="队伍账号" clearable  :readonly="ro.duiwuzhanghao"></el-input>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' v-else class="input" label="队伍账号" prop="duiwuzhanghao">
					<el-input v-model="ruleForm.duiwuzhanghao" placeholder="队伍账号" readonly></el-input>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' class="input" v-if="type!='info'"  label="组长姓名" prop="zuzhangxingming">
					<el-input v-model="ruleForm.zuzhangxingming" placeholder="组长姓名" clearable  :readonly="ro.zuzhangxingming"></el-input>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' v-else class="input" label="组长姓名" prop="zuzhangxingming">
					<el-input v-model="ruleForm.zuzhangxingming" placeholder="组长姓名" readonly></el-input>
				</el-form-item>
			</template>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' v-if="type!='info'"  label="成果内容" prop="chengguoneirong">
					<editor 
						style="min-width: 200px; max-width: 600px;"
						v-model="ruleForm.chengguoneirong" 
						class="editor" 
						action="file/upload">
					</editor>
				</el-form-item>
				<el-form-item :style='{"width":"50%","margin":"0 0 20px 0","display":"inline-block"}' v-else-if="ruleForm.chengguoneirong" label="成果内容" prop="chengguoneirong">
                    <span :style='{"fontSize":"14px","lineHeight":"40px","color":"#333","fontWeight":"500","display":"inline-block"}' v-html="ruleForm.chengguoneirong"></span>
                </el-form-item>
			<el-form-item :style='{"padding":"0","margin":"0"}' class="btn">
				<el-button :style='{"border":"0","cursor":"pointer","padding":"0","margin":"0 20px 0 0","outline":"none","color":"rgba(255, 255, 255, 1)","borderRadius":"4px","background":"rgba(63, 168, 196, 1)","width":"128px","lineHeight":"40px","fontSize":"14px","height":"40px"}'  v-if="type!='info'" type="primary" class="btn-success" @click="onSubmit">提交</el-button>
				<el-button :style='{"border":"1px solid rgba(63, 168, 196, 1)","cursor":"pointer","padding":"0","margin":"0","outline":"none","color":"rgba(63, 168, 196, 1)","borderRadius":"4px","background":"rgba(255, 255, 255, 1)","width":"128px","lineHeight":"40px","fontSize":"14px","height":"40px"}' v-if="type!='info'" class="btn-close" @click="back()">取消</el-button>
				<el-button :style='{"border":"1px solid rgba(63, 168, 196, 1)","cursor":"pointer","padding":"0","margin":"0","outline":"none","color":"rgba(63, 168, 196, 1)","borderRadius":"4px","background":"rgba(255, 255, 255, 1)","width":"128px","lineHeight":"40px","fontSize":"14px","height":"40px"}' v-if="type=='info'" class="btn-close" @click="back()">返回</el-button>
			</el-form-item>
		</el-form>
    

  </div>
</template>
<script>
// 数字，邮件，手机，url，身份证校验
import { isNumber,isIntNumer,isEmail,isPhone, isMobile,isURL,checkIdCard } from "@/utils/validate";
export default {
	data() {
		let self = this
		var validateIdCard = (rule, value, callback) => {
			if(!value){
				callback();
			} else if (!checkIdCard(value)) {
				callback(new Error("请输入正确的身份证号码"));
			} else {
				callback();
			}
		};
		var validateUrl = (rule, value, callback) => {
			if(!value){
				callback();
			} else if (!isURL(value)) {
				callback(new Error("请输入正确的URL地址"));
			} else {
				callback();
			}
		};
		var validateMobile = (rule, value, callback) => {
			if(!value){
				callback();
			} else if (!isMobile(value)) {
				callback(new Error("请输入正确的手机号码"));
			} else {
				callback();
			}
		};
		var validatePhone = (rule, value, callback) => {
			if(!value){
				callback();
			} else if (!isPhone(value)) {
				callback(new Error("请输入正确的电话号码"));
			} else {
				callback();
			}
		};
		var validateEmail = (rule, value, callback) => {
			if(!value){
				callback();
			} else if (!isEmail(value)) {
				callback(new Error("请输入正确的邮箱地址"));
			} else {
				callback();
			}
		};
		var validateNumber = (rule, value, callback) => {
			if(!value){
				callback();
			} else if (!isNumber(value)) {
				callback(new Error("请输入数字"));
			} else {
				callback();
			}
		};
		var validateIntNumber = (rule, value, callback) => {
			if(!value){
				callback();
			} else if (!isIntNumer(value)) {
				callback(new Error("请输入整数"));
			} else {
				callback();
			}
		};
		return {
			id: '',
			type: '',
			
			
			ro:{
				chengguomingcheng : false,
				duiwumingcheng : false,
				chengguoleixing : false,
				chengguofujian : false,
				fengmian : false,
				shangchuanriqi : false,
				chengguoneirong : false,
				duiwuzhanghao : false,
				zuzhangxingming : false,
				clicktime : false,
				clicknum : false,
			},
			
			
			ruleForm: {
				chengguomingcheng: '',
				duiwumingcheng: '',
				chengguoleixing: '',
				chengguofujian: '',
				fengmian: '',
				shangchuanriqi: '',
				chengguoneirong: '',
				duiwuzhanghao: '',
				zuzhangxingming: '',
				clicktime: '',
			},
		
			duiwumingchengOptions: [],
			chengguoleixingOptions: [],
			
			rules: {
				chengguomingcheng: [
				],
				duiwumingcheng: [
				],
				chengguoleixing: [
					{ required: true, message: '成果类型不能为空', trigger: 'blur' },
				],
				chengguofujian: [
				],
				fengmian: [
				],
				shangchuanriqi: [
				],
				chengguoneirong: [
				],
				duiwuzhanghao: [
				],
				zuzhangxingming: [
				],
				clicktime: [
				],
				clicknum: [
					{ validator: validateIntNumber, trigger: 'blur' },
				],
			}
		};
	},
	props: ["parent"],
	computed: {



	},
    components: {
    },
	created() {
		this.ruleForm.shangchuanriqi = this.getCurDate()
	},
	methods: {
		
		// 下载
		download(file){
			window.open(`${file}`)
		},
		// 初始化
		init(id,type) {
			if (id) {
				this.id = id;
				this.type = type;
			}
			if(this.type=='info'||this.type=='else'){
				this.info(id);
			}else if(this.type=='logistics'){
				this.logistics=false;
				this.info(id);
			}else if(this.type=='cross'){
				var obj = this.$storage.getObj('crossObj');
				for (var o in obj){
						if(o=='chengguomingcheng'){
							this.ruleForm.chengguomingcheng = obj[o];
							this.ro.chengguomingcheng = true;
							continue;
						}
						if(o=='duiwumingcheng'){
							this.ruleForm.duiwumingcheng = obj[o];
							this.ro.duiwumingcheng = true;
							continue;
						}
						if(o=='chengguoleixing'){
							this.ruleForm.chengguoleixing = obj[o];
							this.ro.chengguoleixing = true;
							continue;
						}
						if(o=='chengguofujian'){
							this.ruleForm.chengguofujian = obj[o];
							this.ro.chengguofujian = true;
							continue;
						}
						if(o=='fengmian'){
							this.ruleForm.fengmian = obj[o];
							this.ro.fengmian = true;
							continue;
						}
						if(o=='shangchuanriqi'){
							this.ruleForm.shangchuanriqi = obj[o];
							this.ro.shangchuanriqi = true;
							continue;
						}
						if(o=='chengguoneirong'){
							this.ruleForm.chengguoneirong = obj[o];
							this.ro.chengguoneirong = true;
							continue;
						}
						if(o=='duiwuzhanghao'){
							this.ruleForm.duiwuzhanghao = obj[o];
							this.ro.duiwuzhanghao = true;
							continue;
						}
						if(o=='zuzhangxingming'){
							this.ruleForm.zuzhangxingming = obj[o];
							this.ro.zuzhangxingming = true;
							continue;
						}
						if(o=='clicktime'){
							this.ruleForm.clicktime = obj[o];
							this.ro.clicktime = true;
							continue;
						}
						if(o=='clicknum'){
							this.ruleForm.clicknum = obj[o];
							this.ro.clicknum = true;
							continue;
						}
				}
				











			}
			
			
			// 获取用户信息
			this.$http({
				url: `${this.$storage.get('sessionTable')}/session`,
				method: "get"
			}).then(({ data }) => {
				if (data && data.code === 0) {
					
					var json = data.data;
					if(((json.duiwumingcheng!=''&&json.duiwumingcheng) || json.duiwumingcheng==0) && this.$storage.get("role")!="管理员"){
						this.ruleForm.duiwumingcheng = json.duiwumingcheng
						this.ro.duiwumingcheng = true;
					}
					if(((json.duiwuzhanghao!=''&&json.duiwuzhanghao) || json.duiwuzhanghao==0) && this.$storage.get("role")!="管理员"){
						this.ruleForm.duiwuzhanghao = json.duiwuzhanghao
						this.ro.duiwuzhanghao = true;
					}
					if(((json.zuzhangxingming!=''&&json.zuzhangxingming) || json.zuzhangxingming==0) && this.$storage.get("role")!="管理员"){
						this.ruleForm.zuzhangxingming = json.zuzhangxingming
						this.ro.zuzhangxingming = true;
					}
				} else {
					this.$message.error(data.msg);
				}
			});
			
            this.$http({
				url: `option/keyanduiwu/duiwumingcheng`,
				method: "get"
            }).then(({ data }) => {
				if (data && data.code === 0) {
					this.duiwumingchengOptions = data.data;
				} else {
					this.$message.error(data.msg);
				}
            });
            this.chengguoleixingOptions = "科研论文,专利,软件著作权,其他".split(',')
			
		},
    // 多级联动参数

    info(id) {
      this.$http({
        url: `keyanchengguo/info/${id}`,
        method: "get"
      }).then(({ data }) => {
        if (data && data.code === 0) {
        this.ruleForm = data.data;
        //解决前台上传图片后台不显示的问题
        let reg=new RegExp('../../../upload','g')//g代表全部
        this.ruleForm.chengguoneirong = this.ruleForm.chengguoneirong.replace(reg,'../../../springboot2byeq/upload');
        } else {
          this.$message.error(data.msg);
        }
      });
    },


    // 提交
    onSubmit() {








	if(this.ruleForm.chengguofujian!=null) {
		this.ruleForm.chengguofujian = this.ruleForm.chengguofujian.replace(new RegExp(this.$base.url,"g"),"");
	}


	if(this.ruleForm.fengmian!=null) {
		this.ruleForm.fengmian = this.ruleForm.fengmian.replace(new RegExp(this.$base.url,"g"),"");
	}













var objcross = this.$storage.getObj('crossObj');

      //更新跨表属性
       var crossuserid;
       var crossrefid;
       var crossoptnum;
       if(this.type=='cross'){
                var statusColumnName = this.$storage.get('statusColumnName');
                var statusColumnValue = this.$storage.get('statusColumnValue');
                if(statusColumnName!='') {
                        var obj = this.$storage.getObj('crossObj');
                       if(statusColumnName && !statusColumnName.startsWith("[")) {
                               for (var o in obj){
                                 if(o==statusColumnName){
                                   obj[o] = statusColumnValue;
                                 }
                               }
                               var table = this.$storage.get('crossTable');
                             this.$http({
                                 url: `${table}/update`,
                                 method: "post",
                                 data: obj
                               }).then(({ data }) => {});
                       } else {
                               crossuserid=this.$storage.get('userid');
                               crossrefid=obj['id'];
                               crossoptnum=this.$storage.get('statusColumnName');
                               crossoptnum=crossoptnum.replace(/\[/,"").replace(/\]/,"");
                        }
                }
        }
       this.$refs["ruleForm"].validate(valid => {
         if (valid) {
		 if(crossrefid && crossuserid) {
			 this.ruleForm.crossuserid = crossuserid;
			 this.ruleForm.crossrefid = crossrefid;
			let params = { 
				page: 1, 
				limit: 10, 
				crossuserid:this.ruleForm.crossuserid,
				crossrefid:this.ruleForm.crossrefid,
			} 
			this.$http({ 
				url: "keyanchengguo/page", 
				method: "get", 
				params: params 
			}).then(({ 
				data 
			}) => { 
				if (data && data.code === 0) { 
				       if(data.data.total>=crossoptnum) {
					     this.$message.error(this.$storage.get('tips'));
					       return false;
				       } else {
					 this.$http({
					   url: `keyanchengguo/${!this.ruleForm.id ? "save" : "update"}`,
					   method: "post",
					   data: this.ruleForm
					 }).then(({ data }) => {
					   if (data && data.code === 0) {
					     this.$message({
					       message: "操作成功",
					       type: "success",
					       duration: 1500,
					       onClose: () => {
						 this.parent.showFlag = true;
						 this.parent.addOrUpdateFlag = false;
						 this.parent.keyanchengguoCrossAddOrUpdateFlag = false;
						 this.parent.search();
						 this.parent.contentStyleChange();
					       }
					     });
					   } else {
					     this.$message.error(data.msg);
					   }
					 });

				       }
				} else { 
				} 
			});
		 } else {
			 this.$http({
			   url: `keyanchengguo/${!this.ruleForm.id ? "save" : "update"}`,
			   method: "post",
			   data: this.ruleForm
			 }).then(({ data }) => {
			   if (data && data.code === 0) {
			     this.$message({
			       message: "操作成功",
			       type: "success",
			       duration: 1500,
			       onClose: () => {
				 this.parent.showFlag = true;
				 this.parent.addOrUpdateFlag = false;
				 this.parent.keyanchengguoCrossAddOrUpdateFlag = false;
				 this.parent.search();
				 this.parent.contentStyleChange();
			       }
			     });
			   } else {
			     this.$message.error(data.msg);
			   }
			 });
		 }
         }
       });
    },
    // 获取uuid
    getUUID () {
      return new Date().getTime();
    },
    // 返回
    back() {
      this.parent.showFlag = true;
      this.parent.addOrUpdateFlag = false;
      this.parent.keyanchengguoCrossAddOrUpdateFlag = false;
      this.parent.contentStyleChange();
    },
    chengguofujianUploadChange(fileUrls) {
	    this.ruleForm.chengguofujian = fileUrls;
    },
    fengmianUploadChange(fileUrls) {
	    this.ruleForm.fengmian = fileUrls;
    },
  }
};
</script>
<style lang="scss" scoped>
	.amap-wrapper {
		width: 100%;
		height: 500px;
	}
	
	.search-box {
		position: absolute;
	}
	
	.el-date-editor.el-input {
		width: auto;
	}
	
	.add-update-preview .el-form-item /deep/ .el-form-item__label {
	  	  padding: 0 10px 0 0;
	  	  color: #666;
	  	  font-weight: 500;
	  	  width: 140px;
	  	  font-size: 14px;
	  	  line-height: 40px;
	  	  text-align: right;
	  	}
	
	.add-update-preview .el-form-item /deep/ .el-form-item__content {
	  margin-left: 140px;
	}
	
	.add-update-preview .el-input /deep/ .el-input__inner {
	  	  border: 2px solid #797979;
	  	  border-radius: 4px;
	  	  padding: 0 12px;
	  	  outline: none;
	  	  color: rgba(121, 121, 121, 1);
	  	  width: 300px;
	  	  font-size: 14px;
	  	  height: 40px;
	  	}
	
	.add-update-preview .el-select /deep/ .el-input__inner {
	  	  border: 2px solid #797979;
	  	  border-radius: 4px;
	  	  padding: 0 10px;
	  	  outline: none;
	  	  color: rgba(121, 121, 121, 1);
	  	  width: 200px;
	  	  font-size: 14px;
	  	  height: 40px;
	  	}
	
	.add-update-preview .el-date-editor /deep/ .el-input__inner {
	  	  border: 2px solid #797979;
	  	  border-radius: 4px;
	  	  padding: 0 10px 0 30px;
	  	  outline: none;
	  	  color: rgba(121, 121, 121, 1);
	  	  width: 200px;
	  	  font-size: 14px;
	  	  height: 40px;
	  	}
	
	.add-update-preview /deep/ .el-upload--picture-card {
		background: transparent;
		border: 0;
		border-radius: 0;
		width: auto;
		height: auto;
		line-height: initial;
		vertical-align: middle;
	}
	
	.add-update-preview /deep/ .upload .upload-img {
	  	  border: 2px dashed #797979;
	  	  cursor: pointer;
	  	  border-radius: 6px;
	  	  color: #797979;
	  	  width: 150px;
	  	  font-size: 32px;
	  	  line-height: 150px;
	  	  text-align: center;
	  	  height: 150px;
	  	}
	
	.add-update-preview /deep/ .el-upload-list .el-upload-list__item {
	  	  border: 2px dashed #797979;
	  	  cursor: pointer;
	  	  border-radius: 6px;
	  	  color: #797979;
	  	  width: 150px;
	  	  font-size: 32px;
	  	  line-height: 150px;
	  	  text-align: center;
	  	  height: 150px;
	  	}
	
	.add-update-preview /deep/ .el-upload .el-icon-plus {
	  	  border: 2px dashed #797979;
	  	  cursor: pointer;
	  	  border-radius: 6px;
	  	  color: #797979;
	  	  width: 150px;
	  	  font-size: 32px;
	  	  line-height: 150px;
	  	  text-align: center;
	  	  height: 150px;
	  	}
	
	.add-update-preview .el-textarea /deep/ .el-textarea__inner {
	  	  border: 2px solid #797979;
	  	  border-radius: 4px;
	  	  padding: 12px;
	  	  outline: none;
	  	  color: rgba(121, 121, 121, 1);
	  	  width: 400px;
	  	  font-size: 14px;
	  	  height: 120px;
	  	}
</style>
