<template>
  <div class="classify">
    <h1 class="title">{{id ? '编辑' : '新建'}}分类</h1>
    <el-row>
      <el-col :span="8">
        <el-form    @submit.native.prevent="save"  label-width="120px">
          <el-form-item  label="上级分类">
            <el-select  v-model="model.parent">
              <el-option v-for="item  in parents" :key="item._id"  
              :label="item.name"
              :value="item._id">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item  label="名称">
            <el-input v-model="model.name"></el-input>
          </el-form-item>
          <el-form-item  label="创建时间">
            <el-input  v-model="model.times"></el-input>
          </el-form-item>
          <el-form-item>
              <el-button  type="primary"  native-type="submit">保存</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: "CategoryEdit",
  props:{
    id: {}
  },
  data() {
    return {
       model: {},
       parents: []
    }
  },
  methods:{
      async save(){

        /**
         * eslint react xxx is assigned a value but never used 的解决办法 
         *  比较麻烦的解决办法--每一行报错的地方都加一行注释
         * 在声明变量的当前行加上一条注释 // eslint-disable-line no-unused-vars
         * 
         * 
         * 比较暴力的解决方法--直接禁用变量声明但未使用的提示,但是也比较省心，不用加一堆注释
         * 设置.eslint.json(或者是yaml等)的"no-unused-vars"规则为禁用(0为禁用)。
         * ```
         * {
              "rules":{
                "no-unused-vars":0
              }
            }
         * ```
         * 
         * 
         * */

        let  res  // eslint-disable-line no-unused-vars
        if(this.id){
           res = this.$http.put(`rest/categories/${this.id}`, this.model) 
        }else{
           res = this.$http.post('rest/categories', this.model) 
        }
        this.$router.push('/categories/list')
        this.$message({
          type: 'success',
          message: '保存成功'
        })
      },
      async  fetch(){
        const  res =  await  this.$http.get(`rest/categories/${this.id}`)
        this.model = res.data
      },
      //分类选项
      async  fetchParents(){
        const  res =  await  this.$http.get(`rest/categories`)
        this.parents = res.data
      }
  },
  created() {
    this.fetchParents();
    this.id && this.fetch()
  }
};
</script>

<style scoped >
.classify .title{
  margin-bottom: 30px;
  padding: 10px 0;
  border-bottom: 1px solid  #e5e5e5;
}
</style>
