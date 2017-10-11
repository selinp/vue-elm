<template>
    <div class="cartcontrol">
        <transition name="fadeRotate">
            <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart($event)">
                <i class="inner icon-remove_circle_outline"></i>
            </div>
        </transition>
        <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
        <div class="cart-add" @click.stop.prevent="addCart($event)">
            <i class="icon-add_circle"></i>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue'

  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart(event) {
        if (!event._constructed) {
          return
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 0)
        }
        this.food.count++
//         eventHub 为main.js中 new Vue()
//         把event 传递给eventHub上,然后在其他组件中通过
//         this.$root.eventHub.$on('cart.add', this.drop) 获取,
//         this.drop 为shopcart.vew 中要执行的方法
        this.$root.eventHub.$emit('cart.add', event.target)
      },
      decreaseCart(event) {
        if (!event._constructed) {
          return
        }
        if (this.food.count) {
          this.food.count--
        }
      }
    }
  }
</script>

<style lang="less" type="text/less">
    .cartcontrol {
        font-size: 0;
        .cart-decrease {
            display: inline-block;
            padding: 6px;
            transition: all 0.4s linear;
            .inner {
                display: inline-block;
                line-height: 24px;
                font-size: 24px;
                color: rgb(0, 160, 220);
                transition: all 0.4s linear;
            }
            &.fadeRotate-enter-active, &.fadeRotate-leave-active {
                transform: translate3d(0, 0, 0);
                .inner {
                    transform: rotate(0);
                }
            }
            &.fadeRotate-enter, &.fadeRotate-leave-active {
                opacity: 0;
                transform: translate3d(24px, 0, 0);
                .inner {
                    transform: rotate(180deg);
                }
            }
        }
        .cart-count {
            display: inline-block;
            vertical-align: top;
            width: 12px;
            padding-top: 6px;
            line-height: 24px;
            text-align: center;
            font-size: 10px;
            color: rgb(147, 153, 159);
        }
        .cart-add {
            display: inline-block;
            padding: 6px;
            line-height: 24px;
            font-size: 24px;
            color: rgb(0, 160, 220);
        }
    }

</style>