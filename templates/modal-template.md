# 弹窗组件模板 (ZModal)

## 使用场景
适用于需要弹窗交互的表单组件，如新增、编辑、详情查看。

## 完整代码

```vue
<script setup lang="ts">

// Emits
const emit = defineEmits<{
  (e: 'ok', data: Record<string, any>): void
  (e: 'cancel'): void
  (e: 'close'): void
}>()

// Props
interface Props {
  title?: string
  size?: 'mini' | 'small' | 'medium' | 'large'
  okText?: string
  cancelText?: string
  okLoading?: boolean
}
const props = withDefaults(defineProps<Props>(), {
  title: 'COMPONENT_TITLE',
  size: 'medium',
  okText: '确定',
  cancelText: '取消',
  okLoading: false,
})

// 表单数据接口 - 根据字段动态生成
interface FormData {
  // FIELD_INTERFACES: 每个字段的类型定义
}

const formData = ref<FormData>({
  // FIELD_DEFAULTS: 每个字段的默认值
})

const modalRef = ref()

function show() {
  modalRef.value?.show()
}

function close() {
  modalRef.value?.close()
}

function handleOk() {
  emit('ok', formData.value)
}

function handleCancel() {
  emit('cancel')
  close()
}

function resetForm() {
  formData.value = {
    // FIELD_DEFAULTS
  }
}

function setFormData(data: Partial<FormData>) {
  Object.assign(formData.value, data)
}

defineExpose({
  show,
  close,
  resetForm,
  setFormData,
})
</script>

<template>
  <ZModal
    ref="modalRef"
    :title="props.title"
    :size="props.size"
    :ok-text="props.okText"
    :cancel-text="props.cancelText"
    :ok-loading="props.okLoading"
    @ok="handleOk"
    @cancel="handleCancel"
  >
    <NForm label-placement="left" label-width="auto">
      <!-- FORM_ITEMS: 根据字段列表循环生成表单项 -->
      <NFormItem label="字段标签" required>
        <组件类型 v-model:value="formData.字段key" placeholder="占位符" />
      </NFormItem>
    </NForm>

    <!-- FOOTER_BUTTONS: 可选的自定义底部按钮 -->
  </ZModal>
</template>

<style lang="scss" scoped>
</style>
```

## 占位符说明

| 占位符 | 说明 | 示例 |
|--------|------|------|
| `COMPONENT_TITLE` | 组件标题 | 用户信息 |
| `FIELD_INTERFACES` | 字段类型定义 | `name?: string` |
| `FIELD_DEFAULTS` | 字段默认值 | `name: undefined` |
| `FORM_ITEMS` | 表单项内容 | `<NFormItem>...</NFormItem>` |
| `组件类型` | NaiveUI 组件 | `NInput`, `NSelect` 等 |

## 字段组件映射

| 字段类型 | 组件 | 说明 |
|---------|------|------|
| input | NInput | 文本输入 |
| inputNumber | NInputNumber | 数字输入 |
| textarea | NInput | 多行文本 |
| select | NSelect | 下拉选择 |
| radio | NRadioGroup | 单选 |
| checkbox | NCheckbox | 复选 |
| switch | NSwitch | 开关 |
| datePicker | NDatePicker | 日期选择 |
