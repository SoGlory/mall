<template>
  <div class="tab-bar-item" @click="itemClick">
    <!-- 到时候要显示item未选中时，图片普通的状态和item被选中时，当前图片激活的状态 -->
    <!-- 往插槽传入内容时，应该把所有图片传进来，至于何时显示哪张图片，这个决定应该在组件内部处理，不应该由外部处理 -->
    <!-- 推荐：在<slot></slot>外包裹一层。因为slot这个东西最终是会被替换的，有些属性会被替换掉，从而不会起作用
    例如：图片的v-if、v-else虽然有效果，但是在设置文字颜色时却没有其效果，因为外界插入的文字那行标签，直接将slot整个给替换掉了，动态绑定属性压根不在上面 -->
    <div v-if="!isActivated">
      <slot v-if="!isActivated" name="item-icon"></slot>
    </div>
    <div v-else>
      <slot name="item-icon-activated"></slot>
    </div>
    <div :style="activeColorStyle">
      <slot name="item-text"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TabBarItem',
  props: {
    link: String,
    activeColor: {
      type: String,
      // 为了更好的实现封装，在别人使用我们封装好的组件时，本就不该为了某个需求而进去修改源码，组件里
      // 的东西都必须要是动态的
      default: '#eb98a6'
    }
  },
  data () {
    return {
    }
  },
  computed: {
    isActivated () {
      // $route:当前哪个路由处于活跃，这个路由对象就是哪个
      return this.$route.path.indexOf(this.link) !== -1
    },
    activeColorStyle () {
      return this.isActivated ? {color: this.activeColor} : {}
    }
  },
  methods: {
    itemClick () {
      this.$router.push(this.link)
    }
  }
}
</script>

<style>
.tab-bar-item {
  flex: 1;
  height: 49px;
  text-align: center;
  font-size: 14px;
}
.tab-bar-item img {
  margin-top: 3px;
  /* 解决图片底部多出的 3px */
  vertical-align: middle;
  width: 14px;
  height: 14px;
}
</style>
