<template>
  <div class="grid--flex-center grid--justify-between">
  <div class="theme-pagination--pages ">
    <div v-for="page in list"
         class="theme-pagination--page"
         :class="{active: currentPage === page, dots: page === '...'}"
         @click="changePage(page)"
    >
      {{page}}
    </div>

  </div>
    <div class="color--gray-label font--normal font--14-20">
      Показано {{this.pagination.resumesLength}} резюме из {{this.pagination.totalResumes}}
    </div>
  </div>
</template>

<script>
export default {
  props: ['pagination'],
  data() {
    return {
      list: [],
    }
  },
  methods: {
    paginationFunc(c, m) {
      let current = c,
      last = m,
      delta = 2,
      left = current - delta,
      right = parseInt(current) + delta + 1,
      range = [],
      rangeWithDots = [],
      l;

    for (let i = 1; i <= last; i++) {
      if (i == 1 || i == last || (i >= left && i < right)) {
        range.push(i);
      }
    }

    for (let i of range) {
      if (l) {
        if (i - l === 2) {
          rangeWithDots.push(l + 1);
        } else if (i - l !== 1) {
          rangeWithDots.push('...');
        }
      }
      rangeWithDots.push(i);
      l = i;
    }
    this.list = rangeWithDots;
      console.log(this.list)
},

    changePage(pageNum) {
      if (pageNum === '...') {
        return
      }
      this.paginationFunc(pageNum, this.pagination.totalPages)
      this.$router.push({query: {
        page: pageNum
        }})
    }
  },
  mounted() {
    this.paginationFunc(this.currentPage, this.pagination.totalPages)
  },
  computed: {
    currentPage() {
      return parseInt(this.$route.query.page) || 1
    }
  },
  watch: {
    pagination: {
      deep: true,
      handler: function (info) {
        this.paginationFunc(this.currentPage, this.pagination.totalPages)
      }
    }
  }


}

</script>


<style lang="scss" scoped>
.theme-pagination--pages {
  display: flex;
  height: 48px;
}
.theme-pagination--page{
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 32px;
  height: 32px;
  margin-right: 12px;
  margin-top: 8px;
  margin-bottom: 8px;
  padding-left: 8px;
  padding-right: 8px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  line-height: 20px;
  font-weight: 600;
  text-align: center;
}

.theme-pagination--page.dots:hover {
  background: white!important;
  color: #1c1c1c!important;
  cursor: default;
}

.active {
  color: #ffffff;
  background: #42ab44;
}


</style>
