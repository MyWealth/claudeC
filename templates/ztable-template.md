# 表格组件模板 (ZTable)

## 使用场景
适用于需要分页、搜索、排序的数据表格，推荐使用。

## 完整代码

```vue
<script setup lang="ts">
import { ref } from 'vue'
import ZTable from '@/components/z/table'
import { NSpace, NButton, NPopconfirm } from 'naive-ui'

const emit = defineEmits<{
  (e: 'row-click', row: Record<string, any>): void
  (e: 'edit', row: Record<string, any>): void
  (e: 'delete', row: Record<string, any>): void
}>()

interface Props {
  k?: string
  method?: 'get' | 'post'
  requestBody?: Record<string, any>
}
const props = withDefaults(defineProps<Props>(), {
  k: '',
  method: 'get',
  requestBody: () => ({}),
})

const tableRef = ref()

interface TableColumn {
  key: string
  title: string
  width?: number
  fixed?: 'left' | 'right'
  render?: (row: any) => any
}

// 表格列配置 - 根据字段动态生成
const tableColumns = ref<TableColumn[]>([
  // TABLE_COLUMNS: 根据字段列表生成列
  {
    key: '字段key',
    title: '字段标题',
    width: 150,
  },
  {
    title: '操作',
    key: 'actions',
    fixed: 'right',
    width: 150,
    render: (row: any) => {
      return (
        <NSpace>
          <NButton type="primary" text onClick={() => handleEdit(row)}>
            编辑
          </NButton>
          <NPopconfirm onPositiveClick={() => handleDelete(row)}>
            {{
              default: () => '确认删除？',
              trigger: () => (
                <NButton type="error" text>
                  删除
                </NButton>
              ),
            }}
          </NPopconfirm>
        </NSpace>
      )
    },
  },
])

// 搜索表单配置
const searchFormOptions = ref({
  layout: 'grid',
  cols: 4,
  options: [
    // SEARCH_FORM_OPTIONS: 前3个字段作为搜索条件
    {
      type: 'Input',
      key: '字段key',
      span: 1,
      label: '字段标签',
      showFeedback: false,
      itemStyle: {},
      attribute: {
        data: {
          placeholder: '请输入字段标签'
        }
      }
    },
  ],
})

const tableData = ref<any[]>([])

function handleEdit(row: any) {
  emit('edit', row)
}

function handleDelete(row: any) {
  emit('delete', row)
}

function handleRowClick(row: any) {
  emit('row-click', row)
}

function handleSearch(params: any) {
  console.log('搜索参数:', params)
  tableRef.value?.requestTableData(params)
}

function handleReset(params: any) {
  console.log('重置参数:', params)
  tableRef.value?.requestTableData(params)
}

function refresh() {
  tableRef.value?.requestTableData()
}

function getChecked() {
  return tableRef.value?.getChecked()
}

defineExpose({
  refresh,
  getChecked,
  tableRef,
})
</script>

<template>
  <ZTable
    ref="tableRef"
    :k="props.k"
    :method="props.method"
    :request-body="props.requestBody"
    :table-columns="tableColumns"
    :options="searchFormOptions"
    <!-- PAGINATION_CONFIG: 根据配置添加分页属性 -->
    :paging="true"
    :page-size="10"
    :show-size-picker="true"
    :show-quick-jumper="true"
    @search="handleSearch"
    @reset="handleReset"
  >
    <!-- LEFT_BUTTONS: 左侧操作按钮区 -->
    <template #left>
      <NSpace>
        <NButton type="primary">新增</NButton>
      </NSpace>
    </template>
  </ZTable>
</template>

<style lang="scss" scoped>
</style>
```

## 占位符说明

| 占位符 | 说明 |
|--------|------|
| `TABLE_COLUMNS` | 根据字段列表生成列配置 |
| `SEARCH_FORM_OPTIONS` | 前3个字段作为搜索条件 |
| `PAGINATION_CONFIG` | 分页相关属性 |
| `LEFT_BUTTONS` | 左侧操作按钮 |

## ZForm 类型映射

| 字段类型 | ZForm 类型 |
|---------|-----------|
| input | Input |
| inputNumber | InputNumber |
| textarea | Textarea |
| select | Select |
| radio | Radio |
| checkbox | Checkbox |
| switch | Switch |
| datePicker | DatePicker |
