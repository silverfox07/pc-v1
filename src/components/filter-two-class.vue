<template>
  <div class="meedu-filter-box">
    <div class="category2-box" v-if="categories">
      <div class="box">
        <div class="label">分类：</div>
        <div class="item-box">
          <div class="item" :class="{ active: id1 === 0 }" @click="setCid(0)">
            全部
          </div>
          <div
            class="item"
            :class="{ active: id1 === item.id }"
            @click="setCid(item.id)"
            v-for="item in categories"
            :key="item.id"
          >
            {{ item.name }}
          </div>
        </div>
      </div>
      <div
        class="box2"
        v-if="
          categories[cateIndex] && categories[cateIndex].children.length > 0
        "
      >
        <div class="label">细分：</div>
        <div class="item-box">
          <div class="item" :class="{ active: id2 === 0 }" @click="setCid2(0)">
            全部
          </div>
          <div
            class="item"
            :class="{ active: id2 === item2.id }"
            @click="setCid2(item2.id)"
            v-for="item2 in categories[cateIndex].children"
            :key="item2.id"
          >
            {{ item2.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["categories", "cid", "child"],
  data() {
    return {
      id1: 0,
      id2: 0,
    };
  },
  computed: {
    cateIndex() {
      let index = null;
      for (let i = 0; i < this.categories.length; i++) {
        if (this.categories[i].id === this.id1) {
          index = i;
        }
      }
      return index;
    },
  },
  mounted() {
    this.id1 = parseInt(this.cid);
    this.id2 = parseInt(this.child);
  },
  watch: {
    id1() {
      this.$emit("change", this.id1, this.id2);
    },
    id2() {
      this.$emit("change", this.id1, this.id2);
    },
  },
  methods: {
    setCid(id) {
      this.id1 = id;
      this.id2 = 0;
      if (this.id1 === 0) {
        this.$router.push({
          path: this.$route.path,
        });
        return;
      }
      this.$router.push({
        path: this.$route.path,
        query: {
          cid: this.id1,
          child: this.id2,
        },
      });
    },
    setCid2(id) {
      this.id2 = id;
      if (this.id1 === 0) {
        this.$router.push({
          path: this.$route.path,
        });
        return;
      }
      this.$router.push({
        path: this.$route.path,
        query: {
          cid: this.id1,
          child: this.id2,
        },
      });
    },
  },
};
</script>

<style lang="less" scoped>
.meedu-filter-box {
  width: 100%;
  background: #ffffff;
  .category2-box {
    width: 1200px;
    height: auto;
    margin: 0 auto;
    padding: 15px 30px 5px 0px;
    box-sizing: border-box;
    .box {
      width: 100%;
      height: auto;
      display: flex;
      flex-direction: row;
      .label {
        width: 42px;
        height: 14px;
        font-size: 14px;
        font-weight: 400;
        color: #666666;
        line-height: 14px;
        margin-right: 15px;
        margin-top: 10px;
      }
      .item-box {
        width: 1068px;
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        flex-direction: row;
        .item {
          display: flex;
          justify-content: center;
          align-items: center;
          margin-right: 20px;
          font-size: 14px;
          font-weight: 400;
          color: #333333;
          line-height: 14px;
          padding: 10px 15px;
          cursor: pointer;
          margin-bottom: 10px;
          &.active {
            background-color: rgba(160, 175, 187, 0.15);
            border-radius: 4px;
            color: #3ca7fa;
          }
        }
      }
    }
    .box2 {
      width: 100%;
      height: auto;
      display: flex;
      flex-direction: row;
      margin-top: 25px;
      .label {
        width: 42px;
        height: 24px;
        font-size: 14px;
        font-weight: 400;
        color: #666666;
        line-height: 24px;
        margin-right: 20px;
      }
      .item-box {
        width: 1053px;
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        flex-direction: row;
        margin-bottom: 5px;
        .item {
          padding: 4px 9px;
          display: flex;
          justify-content: center;
          align-items: center;
          margin-right: 30px;
          font-size: 14px;
          font-weight: 400;
          color: #333333;
          line-height: 14px;
          border: 1px solid #fff;
          cursor: pointer;
          margin-bottom: 10px;
          &.active {
            color: #3ca7fa;
            border-radius: 13px;
            border: 1px solid #3ca7fa;
          }
        }
      }
    }
  }
}
</style>
