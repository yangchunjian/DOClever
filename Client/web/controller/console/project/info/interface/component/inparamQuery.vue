<template>
    <el-row class="row">
        <table width="100%">
            <template v-for="(item,index) in arr">
                <tr style="text-align: center;vertical-align: middle">
                    <td style="width: 20%;text-align: center;vertical-align: middle;height: 50px">
                        <el-input size="small" style="width: 90%" placeholder="请填写参数名称" v-model.trim="item.name" @input="index==arr.length-1?arr.push({name:'',type:0,must:0,remark:''}):''"></el-input>
                    </td>
                    <td style="width: 10%;" >
                        <el-checkbox v-model="item.must" :true-label="1" :false-label="0">必选</el-checkbox>
                    </td>
                    <td style="width: 55%">
                        <el-input size="small" style="width: 90%" placeholder="请填写备注" v-model="item.remark"></el-input>
                    </td>
                    <td style="width: 5%">
                        <el-button size="mini" type="text" style="font-size: 13px" @click="configValue(item)">{{(item.value && (item.value.data.length>0 || item.value.status))?"已填值":"未填值"}}</el-button>
                    </td>
                    <td style="width: 10%">
                        <el-button size="mini" type="text" style="color: red;font-size: 15px" @click="remove(index)" icon="el-icon-close" v-if="index!=arr.length-1"></el-button>
                    </td>
                </tr>
            </template>
        </table>
    </el-row>
</template>
<script>
    var valueList=require("./valueList.vue")
    module.exports={
        props:["index","item"],
        data:function () {
            return {

            }
        },
        computed: {
            arr:function () {
                return this.item.query
            }
        },
        components:{
            "valuelist":valueList
        },
        methods:{
            remove:function (index) {
                this.arr.splice(index,1)
            },
            configValue:function (item) {
                if(!item.value)
                {
                    Vue.set(item,"value", {
                        type: 0,
                        data: [],
                        status: ""
                    });
                }
                var child=$.showBox(this.$parent,require("./valueList.vue"),{
                    "source":item.value
                });
                child.$on("save",function (value) {
                    item.value=value;
                });
            }
        }
    }
</script>
