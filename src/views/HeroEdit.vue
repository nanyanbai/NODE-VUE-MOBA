<template>
  <div class="classify">
    <h1 class="title">{{id ? '编辑' : '新建'}}英雄</h1>
    <el-form @submit.native.prevent="save" label-width="120px">
      <el-tabs type="border-card"  value="skills">
        <el-tab-pane label="基础信息">
          <el-form-item label="头像">
            <el-upload
              class="avatar-uploader uploader__"
              :action="$http.defaults.baseURL + '/upload'"
              :show-file-list="false"
              :on-success="afterUpload"
            >
              <img v-if="model.avatar" :src="model.avatar" class="avatar" />
              <i v-else class="el-icon-plus avatar-uploader-icon"></i>
            </el-upload>
          </el-form-item>
          <el-form-item label="名称">
            <el-input v-model="model.name"></el-input>
          </el-form-item>
          <el-form-item label="称号">
            <el-input v-model="model.title"></el-input>
          </el-form-item>
          <el-form-item label="类型">
            <el-select v-model="model.categories" multiple>
              <el-option
                v-for="item  of categories"
                :key="item._id"
                :label="item.name"
                :value="item._id"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="难度">
            <el-rate :max="10" show-score v-model="model.scores.difficult"></el-rate>
          </el-form-item>
          <el-form-item label="技能">
            <el-rate :max="10" show-score v-model="model.scores.skiills"></el-rate>
          </el-form-item>
          <el-form-item label="攻击">
            <el-rate :max="10" show-score v-model="model.scores.attack"></el-rate>
          </el-form-item>
          <el-form-item label="生存">
            <el-rate :max="10" show-score v-model="model.scores.survive"></el-rate>
          </el-form-item>
          <el-form-item label="顺风出装">
            <el-select v-model="model.items1" multiple>
              <el-option
                v-for="item  of items"
                :key="item._id"
                :label="item.name"
                :value="item._id"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="逆风出装">
            <el-select v-model="model.items2" multiple>
              <el-option
                v-for="item  of items"
                :key="item._id"
                :label="item.name"
                :value="item._id"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="使用技巧">
            <el-input type="textarea" v-model="model.usageTips"></el-input>
          </el-form-item>
          <el-form-item label="对抗技巧">
            <el-input type="textarea" v-model="model.battleTips"></el-input>
          </el-form-item>
          <el-form-item label="团战思想">
            <el-input type="textarea" v-model="model.teamTips"></el-input>
          </el-form-item>
        </el-tab-pane>

        <el-tab-pane  label="技能"  name="skills">
          <el-button size="small "  @click="model.skills.push({})"> <i class="el-icon-plus"></i> 添加技能</el-button>
          <el-row type="flex" style="flex-wrap: wrap;">
            <el-col :md="12"  v-for="(item ,index)  in  model.skills" :key="index">
              <el-form-item  label="名称">
                <el-input  v-model="item.name"></el-input>
              </el-form-item>
              <el-form-item  label="图标">
                <el-upload
                  class="avatar-uploader uploader__"
                  :action="$http.defaults.baseURL + '/upload'"
                  :show-file-list="false"
                  :on-success="res => $set(item , 'icon' , res.url)"
                >
                  <img v-if="item.icon" :src="item.icon" class="avatar" />
                  <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
              </el-form-item>
              <el-form-item  label="描述">
                <el-input type="textarea"  v-model="item.description"></el-input>
              </el-form-item>
              <el-form-item  label="小提示">
                <el-input type="textarea"  v-model="item.tips"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button  size="small"  type="danger"  @click="model.skills.splice(index,1)">
                  删除
                </el-button>
              </el-form-item>
            </el-col>
          </el-row>
        </el-tab-pane>
      </el-tabs>

      <el-form-item>
        <el-button type="primary" native-type="submit">保存</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  name: "HeroEdit",
  props: {
    id: {}
  },
  data() {
    return {
      categories: [],
      items: [],
      heroes:[],
      model: {
        name: "",
        avatar: "",
        skills: [],
        scores: {
          difficult: 0
        }
      }
    };
  },
  methods: {
    afterUpload(res) {
      // eslint-disable-line no-unused-vars
      // this.$set(this.model, 'avatar', res.url)
      this.model.avatar = res.url;
    },
    async save() {
      let res; // eslint-disable-line no-unused-vars
      if (this.id) {
        res = this.$http.put(`rest/heroes/${this.id}`, this.model);
      } else {
        res = this.$http.post("rest/heroes", this.model);
      }
      this.$router.push("/heroes/list");
      this.$message({
        type: "success",
        message: "保存成功"
      });
    },
    async fetch() {
      const res = await this.$http.get(`rest/heroes/${this.id}`); // eslint-disable-line no-unused-vars
      // this.model = res.data
      this.model = Object.assign({}, this.model, res.data);
    },
    async fetchCategories() {
      const res = await this.$http.get(`rest/categories`); // eslint-disable-line no-unused-vars
      this.categories = res.data;
    },
    async fetchItems() {
      const res = await this.$http.get(`rest/items`); // eslint-disable-line no-unused-vars
      this.items = res.data;
    }
  },
  created() {
    this.fetchCategories();
    this.fetchItems();
    this.id && this.fetch();
  }
};
</script>
<style scoped >
.el-rate {
  line-height: 3;
}
.classify .title {
  margin-bottom: 30px;
  padding: 10px 0;
  border-bottom: 1px solid #e5e5e5;
}

.el-upload {
  border: 1px solid red !important;
}

.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}

</style>



