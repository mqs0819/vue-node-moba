<template>
  <div>
    <h1>英雄列表</h1>
    <el-table style="width: 100%" :data="items">
      <el-table-column prop="_id" label="ID" width="220" />
      <!-- <el-table-column prop="" label="名称" width="180" /> -->
      <el-table-column prop="name" label="名称" width="180" />
      <el-table-column prop="title" label="称号" width="180" />
      <el-table-column prop="avatar" label="头像" width="180">
        <template slot-scope="scope">
          <img :src="scope.row.avatar" style="height: 3rem" alt="" />
        </template>
      </el-table-column>
      <el-table-column fixed="right" label="操作" width="100">
        <template slot-scope="scope">
          <el-button
            type="text"
            size="small"
            @click="$router.push(`/heroes/edit/${scope.row._id}`)"
            >编辑</el-button
          >
          <el-button type="text" size="small" @click="remove(scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
    };
  },
  methods: {
    async fetch() {
      const res = await this.$http.get("rest/heroes");
      this.items = res.data;
    },
    async remove(row) {
      this.$confirm(`此操作将永久删除 ( ${row.name} ) , 是否继续?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(async () => {
          const res = await this.$http.delete(`rest/heroes/${row._id}`);
          this.$message({
            type: "success",
            message: "删除成功!",
          });
          this.fetch();
        })
        .catch(() => {
          // this.$message({
          //   type: "info",
          //   message: "取消删除",
          // });
        });
    },
  },
  created() {
    this.fetch();
  },
};
</script>