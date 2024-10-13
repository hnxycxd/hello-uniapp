<template>
  <u-popup :show="show" @close="close">
    <view>
      <u-search
        placeholder="请输入关键字搜索"
        v-model="keyword"
        custom-style="margin: 20px 10px 5px"
        @custom="search"
        @search="search"
      />

      <scroll-view scroll-y scroll-top="scrollTop" style="max-height: 60vh">
        <view v-if="multiple">
          <u-checkbox-group
            v-model="selectValue"
            placement="column"
            borderBottom
            icon-placement="right"
          >
            <u-checkbox
              :customStyle="{ padding: '10px 8px' }"
              v-for="item in optionsFilter"
              :key="item.name"
              :label="item.label"
              :name="item.name"
            ></u-checkbox>
          </u-checkbox-group>
        </view>

        <view v-else>
          <u-radio-group
            v-model="selectValue"
            placement="column"
            borderBottom
            icon-placement="right"
            @change="radioChange"
          >
            <u-radio
              :customStyle="{ padding: '10px 8px' }"
              v-for="item in optionsFilter"
              :key="item.name"
              :label="item.label"
              :name="item.name"
            />
          </u-radio-group>
        </view>
      </scroll-view>

      <u-row customStyle="margin-bottom: 10px" gutter="10">
        <u-col span="6">
          <u-button @click="cancel">取消</u-button>
        </u-col>
        <u-col span="6">
          <u-button type="primary" @click="confirm">确定</u-button>
        </u-col>
      </u-row>
    </view>
  </u-popup>
</template>

<script>
export default {
  props: {
    show: {
      type: Boolean,
    },
    options: {
      type: Array,
    },
    value: {
      type: String | Number | Array,
    },
    multiple: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      selectValue: this.value,
      prevRadioValue: this.value,
      keyword: '',
      optionsFilter: this.options,
      scrollTop: 0,
      // showOverlay: false,
    }
  },
  watch: {
    value(val) {
      this.selectValue = val
    },
    options(val) {
      this.optionsFilter = val
    },
  },
  methods: {
    search(val) {
      console.log('search value', val)
      // this.showOverlay = true;
      this.$nextTick(() => {
        this.scrollTop = 0
        // this.showOverlay = false;
      })

      if (!val || val.trim() === '') {
        this.optionsFilter = this.options
        return
      }

      this.optionsFilter = this.options.filter(
        (e) => String(e.name).includes(val) || String(e.label).includes(val)
      )
    },
    // 让radio可取消
    radioChange(val) {
      if (val === this.prevRadioValue) {
        this.selectValue = ''
        this.prevRadioValue = ''
        return
      }
      this.prevRadioValue = val
    },
    close() {
      this.$emit('update:show', false)
    },
    cancel() {
      this.close()
    },
    confirm() {
      this.$emit('update:value', this.selectValue)
      this.close()
    },
  },
}
</script>

<style></style>
