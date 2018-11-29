<template>
  <div data-namespace="minesweeper-controller">
    <section data-name="header">
      <div class="header-wrapper">
        <div class="remaining-mines">{{remainingMines}}</div>
        <action-button @click="onRefreshClick">
          <div class="avatar" :style="{backgroundImage: `url(${avatar})`}"></div>
        </action-button>
        <div class="time"></div>
      </div>
    </section>
    <section data-name="minesweeper">
      <div class="minesweeper-wrapper">
        <minesweeper :debug="debug"
                     :width="width"
                     :height="height"
                     :action="action"
                     :mines-count="minesCount"
                     @mark="mark"
                     @mousedown="onMousedown"
                     @mouseup="onMouseup"
                     ref="minesweeper"
        ></minesweeper>
      </div>
    </section>
    <section data-name="tool">
      <div class="tool-wrapper">
        <action-button v-if="action === DIG"
                       @click="onActionClick"
        >
          <div class="action dig"></div>
        </action-button>
        <action-button v-else
                       @click="onActionClick"
        >
          <div class="action mark"></div>
        </action-button>
      </div>
    </section>
  </div>
</template>

<style lang="scss" scoped>
  @import "~butter-styles/@flex";
  @import "~butter-styles/@helpers";
  @import "~butter-styles/@colors";
  @import "common";

  $header_height: .8rem;
  $tool_height: .65rem;

  div[data-namespace=minesweeper-controller] {
    background: $main_color;
    @include helpers-full-size;
    @include flex-grow(1);

    section[data-name=header] {
      height: $header_height;
      padding: .05rem .1rem;
      border-bottom: .03rem solid $shadow_color;
      border-bottom-left-radius: 5px;
      border-bottom-right-radius: 5px;
      @include helpers-border-box;

      .header-wrapper {
        position: relative;
        border: .025rem solid #fff;
        border-left-color: $shadow_color;
        border-top-color: $shadow_color;
        @include helpers-full-size;
        @include flexbox;
        @include flex-align-items(center);
        @include flex-justify-content(space-between);
        @include helpers-border-box;
      }

      .remaining-mines, .time {
        width: 1rem;
        height: 100%;
        font-size: .3rem;
        @include flexbox;
        @include flex-justify-content(center);
        @include flex-align-items(center)
      }

      .avatar {
        width: $header_height - .35rem;
        height: $header_height - .35rem;
        @include helpers-border-box;
        @include helpers-background-config()
      }
    }

    section[data-name=minesweeper] {
      padding: 0 5px;
      height: calc(100% - #{$header_height} - #{$tool_height});
      @include helpers-border-box;

      .minesweeper-wrapper {
        border: 4px solid #fff;
        border-top: 0;
        border-left-color: $shadow_color;
        border-top-color: $shadow_color;
        @include helpers-y-scroll;
        @include helpers-full-size;
        @include helpers-border-box;
      }
    }

    section[data-name=tool] {
      height: $tool_height;
      @include helpers-border-box;

      .tool-wrapper {
        padding: .05rem 0;
        @include helpers-full-size;
        @include flexbox;
        @include flex-align-items(center);
        @include flex-justify-content(center);
        @include helpers-border-box;
      }

      .action {
        width: $tool_height - .2rem;
        height: $tool_height - .2rem;
        @include helpers-background-config()
      }

      .dig {
        background-image: url(~assets/shovel.jpg);
      }

      .mark {
        background-image: url(~assets/flag.jpg);
      }
    }
  }
</style>

<script>
  import Minesweeper, {DIG, MARK} from "./Minesweeper";
  import ActionButton from "./ActionButton";

  const avatars = {
    smile: require('assets/smile.jpg'),
    surprise: require('assets/surprise.jpg')
  };

  export default {
    name: "MinesweeperController",
    components: {
      Minesweeper,
      ActionButton
    },
    props: {
      width: {
        type: Number,
        default: 16
      },
      height: {
        type: Number,
        default: 30
      },
      minesCount: {
        type: Number,
        default: 99
      },
      debug: {
        type: Boolean,
        default: false
      },
    },
    data() {
      return {
        DIG,
        MARK,
        action: DIG,
        remainingMines: this.minesCount,
        avatar: avatars.smile
      }
    },
    methods: {
      mark(count) {
        this.remainingMines = this.minesCount - count
      },
      onActionClick() {
        this.action = this.action === DIG ? MARK : DIG;
      },
      onRefreshClick() {
        const {minesweeper} = this.$refs;
        minesweeper.refresh()
      },
      onMousedown() {
        this.avatar = avatars.surprise
      },
      onMouseup() {
        this.avatar = avatars.smile
      }
    }
  }
</script>