<template>
  <div class="pagination">
      <!-- 上 -->
    <button :disabled='pageNo == 1' @click="$emit('getPageNo',pageNo-1)">上一页</button>
    <button v-if="startNumAndEndNum.start > 1" @click="$emit('getPageNo',1)" :class="{active:pageNo == 1}" >1</button>
    <button v-if="startNumAndEndNum.start > 2" >···</button>

    <!-- 中间 -->
    <button 
    v-for="(page, index) in startNumAndEndNum.end" 
    :key="index" 
    v-if="page>=startNumAndEndNum.start" 
    @click="$emit('getPageNo',page)" 
    :class="{active:pageNo == page}" 
    >
    <!-- page 为中间的数字 -->
    {{ page }}
    </button>

    <!-- 下 -->
    <button v-if="startNumAndEndNum.end < totalPage-1 "  >···</button>
    <button v-if="startNumAndEndNum.end < totalPage "  
    @click="$emit('getPageNo',totalPage)" 
    :class="{active:pageNo == totalPage}" 
    >
    {{ totalPage }}
    </button>
    <button :disabled='pageNo == totalPage' @click="$emit('getPageNo',pageNo+1)"  >下一页</button>

    <button style="margin-left: 30px">共 {{ total }} 条</button>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  props: ["pageNo", "pageSize", "total", "continues"],
  computed: {
    //总共多少页
    totalPage() {
      return Math.ceil(this.total / this.pageSize);
    },
    startNumAndEndNum() {
      const { pageNo, totalPage, continues } = this;
      let start = 0,
          end = 0;
      //连续页码为5页 也就是说最少5页   不正常现象 -->不够5页
      //不正常 连续页码 大于 总页数
      if (continues > totalPage) {
        start = 1;
        end = totalPage;
      } else {
        //正常
        start = pageNo - parseInt(continues / 2); //起始
        end = pageNo + parseInt(continues / 2); //结束

        //还有start 为 0 负数
        if (start < 1) {
          start = 1;
          end = continues;
        }
        //end   大于总页码
        if (end > totalPage) {
          end = totalPage;
          start = totalPage - continues + 1;
        }
      }
      return { start, end };
    },
  },
};
</script>

<style lang="less" scoped>
.pagination {
  text-align: center;
  button {
    margin: 0 5px;
    background-color: #f4f4f5;
    color: #606266;
    outline: none;
    border-radius: 2px;
    padding: 0 4px;
    vertical-align: top;
    display: inline-block;
    font-size: 13px;
    min-width: 35.5px;
    height: 28px;
    line-height: 28px;
    cursor: pointer;
    box-sizing: border-box;
    text-align: center;
    border: 0;

    &[disabled] {
      color: #c0c4cc;
      cursor: not-allowed;
    }

    &.active {
      cursor: not-allowed;
      background-color: #409eff;
      color: #fff;
    }
  }
}
.active{
background: skyblue;
}
</style>
