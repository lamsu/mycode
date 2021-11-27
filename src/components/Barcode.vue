<template>
    <div style="margin-left:100px;" v-loading="loading" >
        <!-- element-loading-text="拼命加载中"
    element-loading-spinner="el-icon-loading"
    element-loading-background="rgba(0, 0, 0, 0.8)" -->
        <p style="font-size:30px">条形码生成系统</p>
        <el-input v-model="input" placeholder="请输入条码文本，使用英文逗号或者换行分隔"  type="textarea" :rows="5" style="width:50%;text-align:center">
        </el-input>
        <div style="display:inline-block;margin-left:20px;">
            <el-button @click="add2(input)" style="margin:10px">
            生成
            </el-button>
            <br>
            <el-button @click="clear" style="margin:10px">清除</el-button>

        </div>
        
        <br>
        <div v-for="(item,i) in items" >            
            <p style="">{{i+1}}</p>
            <img :id="'a'+item"/>    
        </div>

    </div>
</template>

<script>

    import { finished } from 'stream';
    export default{

        data(){
            return{
                input:"",
                items:[],
                str:[],
                loading:false,
            }
        },
        
        methods:{
            clear(){
                this.items=[];
                this.input="";
            },
            add2:function (input) {
                if(input){
                    this.loading=true;
                    this.items=[];
                    this.str = input.split((/,\r|,\n|;\r|;\n|;|\r|\n|,/));
                    for(var i = 0; i < this.str.length; i++ ){
                        this.items.push(this.str[i]);
                        console.log(this.str[i]);
                    };
                    setTimeout(() => {
                        this.add3();
                    }, 100);
                } else{
                    this.$message("文本不能为空")
                }
                
            },
            add3(){
                for(var i = 0; i < this.str.length; i++ ){
                    console.log(this.str[i]);
                    JsBarcode("#a"+this.str[i], this.str[i]);
                };
                this.loading=false;
                this.finished();
            },
            finished(){
                if(this.input){
                    this.$message({
                        showClose: true,
                        message: '恭喜你，条码生成成功！',
                        type: 'success',
                        duration:1500,
                    });
                }
                
            }
            
        },
       
    }
</script>
