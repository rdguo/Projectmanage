<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>
                    <i class="el-icon-lx-calendar"></i> 项目相关
                </el-breadcrumb-item>
                <el-breadcrumb-item>项目添加</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="form-box">
                <el-form ref="form" :model="form" label-width="100px">
                    <el-form-item label="项目名称">
                        <el-input v-model="form.programname"></el-input>
                    </el-form-item>
                    <el-form-item label="临时编号">
                        <el-input v-model="form.temporarynum"></el-input>
                    </el-form-item>
                    <el-form-item label="工程编号">
                        <el-input v-model="form.programnum"></el-input>
                    </el-form-item>
                    <el-form-item label="设计阶段">
                        <el-cascader :options="phase" v-model="form.designphase" placeholder="请选择">
                        </el-cascader>
                    </el-form-item>
                    <el-form-item label="计划时间">
                        <el-col :span="11">
                            <el-date-picker
                                type="date"
                                placeholder="提出时间"
                                v-model="form.proposetime"
                                value-format="yyyy-MM-dd"
                                style="width: 100%;"
                            ></el-date-picker>
                        </el-col>
                        <el-col class="line" :span="2">-</el-col>
                        <el-col :span="11">
                            <el-date-picker
                                placeholder="计划完成时间"
                                v-model="form.plantime"
                                value-format="yyyy-MM-dd"
                                style="width: 100%;"
                            ></el-date-picker>
                        </el-col>
                    </el-form-item>
                    <el-form-item label="设计人员">
                        <el-input v-model="form.designer"></el-input>
                    </el-form-item>
                    <el-form-item label="勘察接收人">
                        <el-input v-model="form.receiver"></el-input>
                    </el-form-item>
                    <el-form-item label="勘察人员">
                        <el-input v-model="form.surveyor"></el-input>
                    </el-form-item>
                    <el-form-item label="估算费用">
                        <el-input v-model="form.plancost"></el-input>
                    </el-form-item>
                     <el-form-item label="实际费用">
                        <el-input v-model="form.actualcost"></el-input>
                    </el-form-item>
                    <el-form-item label="是否外委">
                        <el-input v-model="form.subcontractor"></el-input>
                    </el-form-item>
                    <el-form-item label="要求勘测专业">
                        <el-input v-model="form.professional"></el-input>
                    </el-form-item>
                    <!-- <el-form-item label="多选框">
                        <el-checkbox-group v-model="form.type">
                            <el-checkbox label="步步高" name="type"></el-checkbox>
                            <el-checkbox label="小天才" name="type"></el-checkbox>
                            <el-checkbox label="imoo" name="type"></el-checkbox>
                        </el-checkbox-group>
                    </el-form-item>
                    <el-form-item label="单选框">
                        <el-radio-group v-model="form.resource">
                            <el-radio label="步步高"></el-radio>
                            <el-radio label="小天才"></el-radio>
                            <el-radio label="imoo"></el-radio>
                        </el-radio-group>
                    </el-form-item> -->
                    <el-form-item label="备注">
                        <el-input type="textarea" rows="5" v-model="form.remake"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="onSubmit">项目提交</el-button>
                        <el-button>取消</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>
    </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
    name: 'baseform',
    data() {
        return {
            phase: [
              {
                value: 'feasibilitystudy',
                label: '可研'
              },
              {
                value: 'constructionplan',
                label: '施工图'
              },
              {
                value: 'complete',
                label: '完成'
              },
              
            ],
            form: {
              programname:'',
              temporarynum:'',
              programnum:'',
              designer:'',
              designphase:'',
              receiver:'',
              surveyor:'',
              proposetime:'',
              plantime:'',
              actualtime:'',
              plancost:'',
              actualcost:'',
              subcontractor:'',
              remark:'',
              professional:''
            }
        };
    },
    methods: {
      ...mapActions([
        'addProgram'
      ]),
      onSubmit() {
          this.$message.success('提交成功！');
          if(!this.form.programname){
            this.$message.warning('项目名称必填')
          }else if(!this.form.temporarynum){
            this.$message.warning('临时编号必填')
          }else if(!this.form.designphase){
            this.$message.warning('设计阶段必选')
          }else if(!this.form.receiver){
            this.$message.warning('勘察接收人必选')
          }else if(!this.form.plantime){
            this.$message.warning('计划时间')
          }else if(!this.form.proposetime){
            this.$message.warning('要求完成时间')
          }else{
            this.addProgram(this.form).then(res =>{
              if(res.errno == 0){
                this.$message.success(res.data|| '提交项目成功')
              }
            })           
          }
      }
    }
};
</script>