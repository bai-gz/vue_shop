<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>权限列表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
      <el-table :data="rightList" stripe border>
        <el-table-column type="index" label="#"></el-table-column>
        <template v-for="(item,index) in rightTableColumns" >
          <el-table-column :key="index" :prop="item.prop" :label="item.label" v-if="index < rightTableColumns.length - 1">
            <template scope="scope">
                        <span>
                            {{scope.row[item.prop]}}
                        </span>
            </template>
          </el-table-column>
        </template>
        <el-table-column label="权限等级" prop="level">
          <template scope="scope">
            <el-tag v-if="scope.row.level === '0'">一级</el-tag>
            <el-tag type="success" v-else-if="scope.row.level === '1'">二级</el-tag>
            <el-tag type="warning" v-else>三级</el-tag>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
export default {
  name: 'Rights',
  data () {
    return {
      rightList: [],
      rightTableColumns: [
        { label: '权限名称', prop: 'authName' },
        { label: '路径', prop: 'path' },
        { label: '权限等级', prop: 'level' }
      ]
    }
  },
  created () {
    this.getRightList()
  },
  methods: {
    async getRightList () {
      const { data: res } = await this.$http.get('rights/list')
      if (res.meta.status !== 200) return this.$message.error('获取权限列表失败！')
      this.rightList = res.data
      // console.log(this.rightList)
      // console.log(this.rightTableColumns.length)
    }
  }
}
</script>

<style scoped>

</style>
