<template>
  <ul class="wrapper group">
    <li class="item checked">
      <div class="gutter">
        <div class="box tall">
          <!-- eslint-disable vue/no-v-html-->
          <span v-html="$t('検査実施<br />人数')" />
          <!-- eslint-enable vue/no-v-html-->
          <span>
            <b>{{ 検査実施人数 }}</b>
            <span class="unit">{{ $t('人') }}</span>
          </span>
        </div>
      </div>
    </li>
    <li class="item positive">
      <div class="gutter">
        <div class="box tall">
          <span>
            {{ $t('陽性患者数') }}
            <br />{{ $t('(累積)') }}
          </span>
          <span>
            <b>{{ 陽性患者数 }}</b>
            <span class="unit">{{ $t('人') }}</span>
          </span>
        </div>
      </div>
      <ul class="group">
        <li class="item in-hospital">
          <div class="gutter">
            <div class="box">
              <span>{{ $t('入院中') }}</span>
              <span>
                <b>{{ 入院中 }}</b>
                <span class="unit">{{ $t('人') }}</span>
              </span>
            </div>
          </div>
        </li>
        <li class="item recovered">
          <div class="gutter">
            <div class="box">
              <span>{{ $t('退院') }}</span>
              <span>
                <b>{{ 退院 }}</b>
                <span class="unit">{{ $t('人') }}</span>
              </span>
            </div>
          </div>
        </li>
        <li class="item deceased">
          <div class="gutter">
            <div class="box">
              <span>{{ $t('死亡') }}</span>
              <span>
                <b>{{ 死亡 }}</b>
                <span class="unit">{{ $t('人') }}</span>
              </span>
            </div>
          </div>
        </li>
      </ul>
    </li>
  </ul>
</template>

<script>
export default {
  /* eslint-disable vue/prop-name-casing */
  props: {
    検査実施人数: {
      type: Number,
      required: true
    },
    陽性患者数: {
      type: Number,
      required: true
    },
    入院中: {
      type: Number,
      required: true
    },
    退院: {
      type: Number,
      required: true
    },
    死亡: {
      type: Number,
      required: true
    }
  },
  methods: {
    /** 桁数に応じて位置の調整をする */
    getAdjustX(input) {
      const length = input.toString(10).length
      switch (length) {
        case 1: {
          return 3
        }
        case 2: {
          return 0
        }
        case 3: {
          return -3
        }
        case 4: {
          return -8
        }
        default: {
          return 0
        }
      }
    },
    /** グラフ内容がわかる支援技術用テキストの中身を取得する **/
    ariaLabel(inspected, positive, hospitalized, deceased, discharged) {
      const ariaLabel = `検査陽性者の状況: 検査実施人数は${inspected}人、うち累積の陽性者数は${positive}人です。入院中は${hospitalized}人です。さらに退院は${discharged}人、死亡は${deceased}人です。`
      return ariaLabel
    }
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  width: calc(100% + 3px);
  box-sizing: border-box;
  * {
    box-sizing: inherit;
  }
}
.group {
  display: flex;
  align-items: flex-end;
  padding-left: 0;
  flex: 0 0 auto;
}
.item {
  display: block;
  list-style-type: none;
  flex: 0 0 auto;
}
.gutter {
  width: 100%;
  padding-right: 3px;
}
.box {
  $box-height: 170px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  position: relative;
  padding-bottom: 26px;
  width: 100%;
  height: $box-height;
  border: 3px solid $green-1;
  color: $green-1;
  @include font-size(14);
  text-align: center;
  line-height: 1.2;
  &.tall {
    height: $box-height + 35px;
  }
  &.short {
    height: $box-height - 35px;
  }
  span:last-child {
    margin-top: 0.2em;
  }
  b {
    font-weight: normal;
    @include font-size(18);
  }
}
.unit {
  @include font-size(14);
}
// 検査
.item.checked {
  width: calc(100% / 5);
  > .gutter > .box {
    border-color: $gray-1;
    color: $gray-1;
  }
}
// 陽性
.item.positive {
  display: flex;
  justify-content: space-between;
  width: calc(100% / 5 * 4);
  > .group {
    width: calc(100% / 4 * 3);
  }
}
// 入院
.item.in-hospital {
  display: flex;
  justify-content: space-between;
  width: calc(100% / 3);
}
// 退院
.item.recovered {
  width: calc(100% / 3);
}
// 死亡
.item.deceased {
  width: calc(100% / 3);
}
.item.positive > .gutter > .box::before,
.item.deceased > .gutter > .box::before {
  content: '';
  display: block;
  border: 3px solid $green-1;
  background-color: $white;
  position: absolute;
  height: 32px;
}
.item.positive > .gutter > .box::before,
.item.in-hospital > .gutter > .box::before {
  border-right: none;
  top: -3px;
  right: calc(-100% - 3px - 3px);
  width: calc(100% + 3px + 3px);
  border-left: none;
  border-right: none;
}
.item.deceased > .gutter > .box::before {
  top: calc(-35px - 3px);
  right: -3px;
  border-left: none;
}
.item.deceased > .gutter > .box::before {
  width: 320%;
}
@function px2vw($px, $vw) {
  @return ceil($px / $vw * 100000vw) / 1000;
}
@mixin variation($vw, $bdw, $fz, $boxh, $boxdiff) {
  .gutter {
    padding-right: px2vw($bdw, $vw);
  }
  .box {
    padding-bottom: px2vw($boxh * 0.17, $vw);
    border-width: px2vw($bdw, $vw);
    height: px2vw($boxh, $vw);
    font-size: px2vw($fz, $vw);
    &.tall {
      height: px2vw($boxh + $boxdiff, $vw);
    }
    &.short {
      height: px2vw($boxh - $boxdiff, $vw);
    }
    b {
      font-size: px2vw($fz + 2, $vw);
    }
  }
  .unit {
    font-size: px2vw($fz, $vw);
  }
  .item.positive > .gutter > .box::before,
  .item.in-hospital > .gutter > .box::before,
  .item.deceased > .gutter > .box::before {
    border-width: px2vw($bdw, $vw);
    height: px2vw($boxdiff - $bdw, $vw);
  }
  .item.positive > .gutter > .box::before,
  .item.in-hospital > .gutter > .box::before {
    top: px2vw(-$bdw, $vw);
    right: calc(-100% - #{px2vw($bdw * 2, $vw)} + 0.3px);
    width: calc(100% + #{px2vw($bdw * 2, $vw)});
  }
  .item.deceased > .gutter > .box::before {
    top: px2vw(-$boxdiff - $bdw, $vw);
    right: px2vw(-$bdw, $vw);
  }
}
// variables.scss Breakpoints: huge
@include lessThan(1440) {
  @include variation(1440, 3, 14, 180, 35);
}
// Vuetify Breakpoints: Large
@include lessThan(1263) {
  @include variation(1263, 2, 12, 150, 24);
}
// Vuetify Breakpoints: Small
@include lessThan(959) {
  @include variation(960, 4, 16, 250, 40);
}
// Vuetify Breakpoints: Extra Small
@include lessThan(599) {
  @include variation(600, 3, 14, 200, 35);
}
</style>
