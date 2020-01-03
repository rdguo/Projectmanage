<template>
   
    <div class="index"> 
        <el-tabs type="border-card">
            
            <!--待办任务-->
            <el-tab-pane>
                <span slot="label"><i class="el-icon-document"></i> 待办任务</span>
                <div>
                    <el-col :span="24">
                        <el-button type="primary" @click="addTask()">添加任务</el-button>
                    </el-col>
                    <!--添加任务弹出的空编辑窗口-->
                    <el-dialog title="添加任务" :visible.sync="isDialogAddVisible">
                        <el-form :model="addTaskForm" :rules="addRules" ref="addTaskForm" label-width="100px" class="demo-addTaskForm">
                            <el-form-item label="任务名称" prop="name">
                                <el-input v-model="addTaskForm.name"></el-input>
                            </el-form-item>
                            <el-form-item label="任务详情" prop="detail">
                                <el-input type="textarea" v-model="addTaskForm.detail"></el-input>
                            </el-form-item>
                            <el-form-item label="任务等级" prop="mark">
                                <el-select v-model="addTaskForm.mark" placeholder="请选择任务等级">
                                    <el-option label="普通任务" value="普通任务"></el-option>
                                    <el-option label="重要任务" value="重要任务"></el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="开始时间" required>
                                <el-col :span="11">
                                    <el-form-item prop="date1">
                                        <el-date-picker type="date" placeholder="选择日期" v-model="addTaskForm.date1" style="width: 100%;"></el-date-picker>
                                    </el-form-item>
                                </el-col>
                                <el-col class="line" :span="2">-</el-col>
                                <el-col :span="11">
                                    <el-form-item prop="date2">
                                        <el-time-picker placeholder="选择时间" v-model="addTaskForm.date2" style="width: 100%;"></el-time-picker>
                                    </el-form-item>
                                </el-col>
                            </el-form-item>
                            <el-form-item label="截止时间" required>
                                <el-col :span="11">
                                    <el-form-item prop="date3">
                                        <el-date-picker type="date" placeholder="选择日期" v-model="addTaskForm.date3" style="width: 100%;"></el-date-picker>
                                    </el-form-item>
                                </el-col>
                                <el-col class="line" :span="2">-</el-col>
                                <el-col :span="11">
                                    <el-form-item prop="date4">
                                        <el-time-picker placeholder="选择时间" v-model="addTaskForm.date4" style="width: 100%;"></el-time-picker>
                                    </el-form-item>
                                </el-col>
                            </el-form-item>
                            <el-form-item label="多人协作任务" prop="mutil">
                                <el-switch v-model="addTaskForm.mutil"></el-switch>
                            </el-form-item>
                            <el-form-item label="任务类型" prop="type">
                                <el-checkbox-group v-model="addTaskForm.type">
                                    <el-checkbox label="学习" name="type"></el-checkbox>
                                    <el-checkbox label="工作" name="type"></el-checkbox>
                                    <el-checkbox label="娱乐" name="type"></el-checkbox>
                                </el-checkbox-group>
                            </el-form-item>
                            <el-form-item label="任务完成度" prop="per">
                                <el-radio-group v-model="addTaskForm.per">
                                    <el-radio label="刚刚开始"></el-radio>
                                    <el-radio label="部分完成"></el-radio>
                                    <el-radio label="半数完成"></el-radio>
                                    <el-radio label="多半完成"></el-radio>
                                    <el-radio label="即将完成"></el-radio>
                                </el-radio-group>
                            </el-form-item>
                            <el-form-item label="完成情况" prop="finishState">
                                <el-input type="textarea" v-model="addTaskForm.finishState"></el-input>
                            </el-form-item>
                            <el-form-item>
                                <el-button type="primary" @click="submitForm('addTaskForm')">创建任务</el-button>
                                <el-button @click="resetForm('addTaskForm')">重置</el-button>
                            </el-form-item>
                        </el-form>
                    </el-dialog>

                    
                </div>
                <!--任务列表_未完成-->
                    <el-table :data="unFinishTask" style="width: 100%">
                        <el-table-column label="任务等级" width="300">
                        <template slot-scope="scope">
                            <i class="el-icon-s-flag"></i>
                            <span style="margin-left: 10px">{{ scope.row.mark }}</span>
                        </template>
                        </el-table-column>
                        <el-table-column label="任务名称" width="800">
                        <template slot-scope="scope">
                            <el-popover trigger="hover" placement="top">
                            <p>名称: {{ scope.row.name }}</p>
                            <p>详情: {{ scope.row.detail }}</p>
                            <div slot="reference" class="name-wrapper">
                                <el-tag size="medium">{{ scope.row.name }}</el-tag>
                            </div>
                            </el-popover>
                        </template>
                        </el-table-column>
                        <el-table-column label="操作">
                        <template slot-scope="scope">
                            <el-button type="success" icon="el-icon-check" circle @click="finishTaskF(scope.$index, scope.row)"></el-button>
                            <el-button type="primary" icon="el-icon-edit" circle  @click="handleEdit(scope.$index, scope.row)"></el-button>
                            <el-button type="danger" icon="el-icon-delete" circle @click="handleDelete_unFinishTask(scope.$index, scope.row)"></el-button>
                        </template>
                        </el-table-column>
                    </el-table> 
            </el-tab-pane>

            <!--已完成-->
            <el-tab-pane>
                <span slot="label"><i class="el-icon-document-checked"></i> 已完成</span>

                 <!--任务列表_已完成-->
                    <el-table :data="finishTask" style="width: 100%">
                        <el-table-column label="任务等级" width="300">
                        <template slot-scope="scope">
                            <i class="el-icon-s-flag"></i>
                            <span style="margin-left: 10px">{{ scope.row.mark }}</span>
                        </template>
                        </el-table-column>
                        <el-table-column label="任务名称" width="800">
                        <template slot-scope="scope">
                            <el-popover trigger="hover" placement="top">
                            <p>名称: {{ scope.row.name }}</p>
                            <p>详情: {{ scope.row.detail }}</p>
                            <div slot="reference" class="name-wrapper">
                                <el-tag size="medium">{{ scope.row.name }}</el-tag>
                            </div>
                            </el-popover>
                        </template>
                        </el-table-column>
                        <el-table-column label="操作">
                        <template slot-scope="scope">
                            <el-button type="warning" icon="el-icon-back" circle @click="unFinishTaskF(scope.$index, scope.row)"></el-button>
                            <el-button type="primary" icon="el-icon-edit" circle  @click="handleEdit(scope.$index, scope.row)"></el-button>
                            <el-button type="danger" icon="el-icon-delete" circle @click="handleDelete_finishTask(scope.$index, scope.row)"></el-button>
                        </template>
                        </el-table-column>
                    </el-table> 
            </el-tab-pane>

            <!--所有任务 -->
            <el-tab-pane>
                <span slot="label"><i class="el-icon-document-copy"></i> 所有任务</span>
                <div>
                    <!--
                    <el-row>
                        <el-col :span="12">
                            <el-input placeholder="搜索..." v-model="search_input" clearable></el-input>
                        </el-col>
                        <el-col :span="2">
                            <el-button type="primary">搜索任务</el-button> 
                        </el-col>
                    </el-row> 
                    -->
                    <!--任务列表_所有-->
                    <el-table :data="allTask" style="width: 100%">
                        <el-table-column label="任务等级" width="300">
                        <template slot-scope="scope">
                            <i class="el-icon-s-flag"></i>
                            <span style="margin-left: 10px">{{ scope.row.mark }}</span>
                        </template>
                        </el-table-column>
                        <el-table-column label="任务名称" width="800">
                        <template slot-scope="scope">
                            <el-popover trigger="hover" placement="top">
                            <p>名称: {{ scope.row.name }}</p>
                            <p>详情: {{ scope.row.detail }}</p>
                            <div slot="reference" class="name-wrapper">
                                <el-tag size="medium">{{ scope.row.name }}</el-tag>
                            </div>
                            </el-popover>
                        </template>
                        </el-table-column>
                        <el-table-column label="操作">
                        <template slot-scope="scope">
                            <el-button type="primary" icon="el-icon-edit" circle  @click="handleEdit(scope.$index, scope.row)"></el-button>
                            <el-button type="danger" icon="el-icon-delete" circle @click="handleDelete_allTask(scope.$index, scope.row)"></el-button>
                        </template>
                        </el-table-column>
                    </el-table> 
                </div>
            </el-tab-pane> 
        </el-tabs>

        <!--编辑任务弹出的空编辑窗口-->
                    <el-dialog title="编辑任务" :visible.sync="isDialogEditVisible">
                        <el-form :model="editTaskForm" :rules="editRules" ref="editTaskForm" label-width="100px" >
                            <el-form-item label="任务名称" prop="name">
                                <el-input v-model="editTaskForm.name"></el-input>
                            </el-form-item>
                            <el-form-item label="任务详情" prop="detail">
                                <el-input type="textarea" v-model="editTaskForm.detail"></el-input>
                            </el-form-item>
                            
                            <el-form-item label="任务等级" prop="mark">
                                <el-select v-model="editTaskForm.mark" placeholder="请选择任务等级">
                                    <el-option label="普通任务" value="普通任务"></el-option>
                                    <el-option label="重要任务" value="重要任务"></el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item>
                                <el-button type="primary" @click="submitEditForm('editTaskForm')">提交修改</el-button>
                                <el-button @click="resetForm('editTaskForm')">重置</el-button>
                            </el-form-item>
                        </el-form>
                    </el-dialog>
    </div>
   
   
</template>

<script>
export default {
    
    
    data() {
      return {
        inDex:0,
        unFinishTask: [ {
         
          name: '职业规划',
          detail: '考研还是就业？',
          mark:'普通任务'
          
        }],
        finishTask: [{
          
          name: '高考复习',
          detail: '倒计时30天，加油！',
          mark:'重要任务'
        }, {
          
          name: '听讲座',
          detail: '和小伙伴们一起学习',
          mark:'普通任务'
        }, {
          
          name: '申请大创',
          detail: '好好准备答辩',
          mark:'重要任务'
        }],
        allTask: [{
         
          name: '高考复习',
          detail: '倒计时30天，加油！',
          mark:'重要任务'
        }, {
          
          name: '听讲座',
          detail: '和小伙伴们一起学习',
          mark:'普通任务'
        }, {
          
          name: '申请大创',
          detail: '好好准备答辩',
          mark:'重要任务'
        }, {
          
          name: '职业规划',
          detail: '考研还是就业？',
          mark:'普通任务'
        }],
        //search_input: '',
        isDialogAddVisible:false,
        isDialogEditVisible:false,
        addTaskForm: {
          name: '',
          mark: '',
          date1: '',
          date2: '',
          date3: '',
          date4: '',
          mutil: false,
          type: [],
          per: '',
          detail: '',
          finishState: ''
         

        },
        editTaskForm: {
          name: '',
          
          mark: '',
          
          detail: ''
          
        },
        addRules: {
          name: [
            { required: true, message: '请输入任务名称', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          mark: [
            { required: true, message: '请选择任务等级', trigger: 'change' }
          ],
          date1: [
            { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
          ],
          date2: [
            { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
          ],
          date3: [
            { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
          ],
          date4: [
            { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
          ],
          type: [
            { type: 'array', required: true, message: '请至少选择一个任务类型', trigger: 'change' }
          ],
          finishState: [
            { required: true, message: '请填写任务完成情况', trigger: 'change' }
          ],
          detail: [
            { required: true, message: '请填写任务详情', trigger: 'blur' }
          ]
        },
        editRules: {
          name: [
            { required: true, message: '请输入任务名称', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          
          mark: [
            { required: true, message: '请选择任务等级', trigger: 'change' }
          ],
          
          detail: [
            { required: true, message: '请填写任务详情', trigger: 'blur' }
          ]
        },
      }
    },
    methods:{
        addTask(){
            this.isDialogAddVisible=true;
        },
        // editTask(){
        //     this.isDialogEditVisible=true
        // },
        submitForm(formName) {
            
            this.$refs[formName].validate((valid) => {
            if (valid) { 
                alert('已提交！');
                this.isDialogAddVisible=false;
                var resmble = JSON.parse(JSON.stringify(this.addTaskForm))
                this.unFinishTask.push(resmble)
            } else {
                console.log('错误！');
                return false;
            }
            });
      },
      submitEditForm(formName) {
            
            this.$refs[formName].validate((valid) => {
            if (valid) { 
                alert('已提交！');
                this.isDialogEditVisible=false;
                this.unFinishTask.splice(this.inDex, 1, JSON.parse(JSON.stringify(this.editTaskForm)));
                this.inDex = 0;
                //var resmble = JSON.parse(JSON.stringify(this.editTaskForm))
                //this.unFinishTask.indexOf(resmble)
            } else {
                console.log('错误！');
                return false;
            }
            });
      },
      resetForm(formName) {
            this.$refs[formName].resetFields();
      },
      handleEdit(index, row) {
        console.log(index, row);
        this.inDex=index;
        this.isDialogEditVisible=true;
      },
      handleDelete_unFinishTask(index, row) {
        console.log(index, row);
        this.unFinishTask.splice(index, 1);
      },
      handleDelete_finishTask(index, row) {
        console.log(index, row);
        this.finishTask.splice(index, 1);
      },
      handleDelete_allTask(index, row) {
        console.log(index, row);
        this.allTask.splice(index, 1);
      },
      finishTaskF(index, row) {
        console.log(index, row);
        this.finishTask.push(JSON.parse(JSON.stringify(row)))
        //从未完成中删除
        this.unFinishTask.splice(index, 1)
      },
      unFinishTaskF(index, row) {
        console.log(index, row);
        this.unFinishTask.push(JSON.parse(JSON.stringify(row)))
        //从已完成中删除
        this.finishTask.splice(index, 1)
      },
    }
}
</script>

