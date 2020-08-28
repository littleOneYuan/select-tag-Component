<template>
  <div class="drawerContainer">
    <el-form
      ref="drawerForm"
      :model="drawerForm"
      :rules="rules"
    >
      <el-form-item prop="gameIds">
        <span>子游戏名：</span>
        <el-select
          v-model="drawerForm.gameIds"
          multiple // 可多选
          collapse-tags // 带标签
          filterable // 可搜索
          clearable // 可清空
          @clear="clearDo" // 清空事件
          @change="chooseGame" // 选项改变事件
          placeholder="请选择"
        >
          <el-option
            v-for="item in belongDeptOptions"
            :key="item.gameId"
            :label="item.gameName"
            :value="item.gameId"
          >
          </el-option>
        </el-select>
      </el-form-item>
    </el-form>
    <el-tag
      v-for="tag in tags"
      :key="tag.gameName"
      size="medium"
      effect="plain"
      closable
      :disable-transitions="false"
      @close="tagClose(tag)" // 标签关闭事件
    >
      {{tag.gameName}}
    </el-tag>
  </div>
</template>

<script>
export default {
  data () {
    return {
      drawerForm: {
        gameIds: []
      },
      tags: [],
      belongDeptOptions: [{ // 所属部门
        gameId: '...',
        gameName: '..'
      },...]
    }
  },
  methods: {
    // 选择器选中动态添加标签
    chooseGame () {
      if (this.drawerForm.gameIds.length !== 0) {
        // 临时对象保存选项
        let temp = {}
        temp.gameId = this.drawerForm.gameIds[this.drawerForm.gameIds.length - 1]
        this.belongDeptOptions.forEach(item => {
          if (item.gameId === temp.gameId) temp.gameName = item.gameName
        })
        // 选择相同标签等于删除该标签
        var tFlag = true // 标记是否在已有标签中存在新增标签，若存在，则不新增而是删除
        for (let i = 0; i < this.tags.length; i++) {
          if (this.tags[i].gameId === temp.gameId) {
            tFlag = false
            this.tags.splice(i === this.tags.length - 1 ? 0 : i + 1, 1) // 这里的i+1具体也不是很清楚...本意是取index但是实际取到的是index-1所以就+1了，然后index=0的时候实际是this.tags.length - 1
          }
        }
        // 未重复且非空标签才push
        if (tFlag && !temp.gamaId) this.tags.push(temp)
      } else { // 未选择任何一项时直接赋空值
        this.tags = []
      }
    },
    // 清空标签
    clearDo () {
      this.tags = []
    },
    // 标签关闭
    tagClose (tag) {
      this.tags.splice(this.tags.indexOf(tag), 1)
      this.drawerForm.gameIds.splice(this.drawerForm.gameIds.indexOf(tag.gameId), 1)
    }
  }
}
</script>

