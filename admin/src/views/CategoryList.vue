<template>
  <div>
    <h1>分类列表</h1>
    <el-table :data="items">
        <el-table-column prop="_id" label="ID" width="240">
        </el-table-column>
        <el-table-column prop="name" label="姓名">
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleEdit(scope.row)">编辑</el-button>
              <el-button
              size="mini"
              @click="handleDelete(scope.row)">删除</el-button>
          </template>
    </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: []
    }
  },
  methods: {
    async fetch(){
      const res = await this.$http.get('categories')
      this.items = res.data
    },
    handleEdit(row){
      this.$router.push(`/categories/edit/${row._id}`)
    },
    handleDelete(row){
      this.$confirm(`是否确认删除分类 "${row.name}"`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(async () => {
          await this.$http.delete(`/categories/${row._id}`)
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          this.fetch()
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
    }
  },
  created() {
    this.fetch()
  },
}
</script>