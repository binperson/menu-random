<template>
  <div id="app">
    <div class="menu-wrapper">
      <div class="menu-parts">
        <h1 class="part-title menu-part-item">
          <i class="icon-menu"></i>
          <span>
          MENU
          </span>
        </h1>
        <ul>
          <li @click="addMenu(index)" v-for="(item, index) in menu" class="menu-item">
            <div class="menu-item-content" :class="{'active': index === menuIndex}">
              {{item}}
            </div>
          </li>
        </ul>
      </div>
      <div class="menu-parts">
        <div class="filter-part menu-part-item">
          <h1 class="part-title">
            <i class="icon-filter"></i>
            <span>
          FILTER BY
        </span>
          </h1>
          <ul>
            <li @click="addFilter(index)" v-for="(item, index) in filters" class="menu-item">
              <div class="menu-item-content" :class="{'active': index === filterIndex}">
                {{item}}
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <router-view :filterName="filterName" :menuTitle="menuTitle"></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
export default {
  name: 'app',
  data() {
    return {
      menu: ['Breakfast','lunch','Dinner','Deserts'],
      filters: ['Num of dishes','Total prices','disabled','Healthy','Halal','Beef'],
      filterIndex: 0,
      menuIndex: 0
    }
  },
  methods: {
    addFilter(index){
        this.filterIndex = index;
    },
    addMenu(index){
        this.menuIndex = index;
    }
  },
  computed: {
    menuTitle(){
        return this.menu[this.menuIndex]
    },
    filterName(){
      return this.filters[this.filterIndex]
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  #app
    display: flex
    width: 100%
    position: absolute
    top: 0
    bottom: 0
    .menu-wrapper
      box-sizing: border-box
      flex: 0 0 250px
      width: 250px
      background: #d7634e
      padding: 8px 16px
      .menu-parts
        &:first-child
          border-bottom: #bd503c 1px solid
        .part-title
          height: 60px
          line-height: 60px
          color: #ffffff
        .menu-item
          color: #ffffff
          line-height: 33px
          .menu-item-content
            padding: 0 16px
            &.active
              background: #ffffff
              color: #d7634e
              border-radius: 5px




</style>
