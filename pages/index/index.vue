<template>
  <view>
    <u--form labelPosition="left" :model="formValues" :rules="rules" ref="uForm">
      <u-form-item label="姓名" prop="name" borderBottom @click="showNameSelect = true">
        <u--input
          v-model="formValues.name"
          border="none"
          disabled
          disabledColor="#fff"
          inputAlign="right"
          class="pop-select"
        />
        <u-icon slot="right" name="arrow-right" />
      </u-form-item>

      <u-form-item label="年龄" prop="age" borderBottom>
        <u--input v-model="formValues.age" border="none" />
      </u-form-item>

      <u-form-item label="兴趣" prop="hobby" borderBottom @click="showHobbySelect = true">
        <u--input
          v-model="formValues.hobby"
          border="none"
          disabled
          disabledColor="#fff"
          inputAlign="right"
        />
        <u-icon slot="right" name="arrow-right" />
      </u-form-item>
    </u--form>

    <u-button type="primary" @click="onSubmit">提交</u-button>

    <popup-select
      :show.sync="showNameSelect"
      :options="nameOptions"
      :value.sync="formValues.name"
    />

    <popup-select
      multiple
      :show.sync="showHobbySelect"
      :options="hobbyOptions"
      :value.sync="formValues.hobby"
    />
  </view>
</template>

<script>
export default {
  data() {
    return {
      formValues: {
        name: '7',
        age: null,
        hobby: ['3', '5'],
      },
      rules: {
        age: {
          type: 'number',
          required: true,
          message: '请填写年龄',
          trigger: ['blur', 'change'],
        },
      },

      showNameSelect: false,
      nameOptions: [],

      showHobbySelect: false,
      hobbyOptions: [],
    }
  },
  onLoad() {},
  onReady() {
    this.$refs.uForm.setRules(this.rules)
    setTimeout(() => {
      const mockList = Array(300)
        .fill(null)
        .map((e, i) => ({
          label: String(i),
          name: String(i),
        }))

      this.nameOptions = mockList
      this.hobbyOptions = mockList.slice(0, 20)
    }, 1000)
  },
  methods: {
    onSubmit(values) {
      this.$refs.uForm.validate().then((res) => {
        console.log('this.formValue', this.formValues)
      })
    },
  },
}
</script>

<style lang="scss">
.pop-select {
  background-color: white;
}
</style>
