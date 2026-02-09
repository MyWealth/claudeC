# 表格组件模板 (NDataTable)

## 使用场景
适用于需要灵活自定义的表格，使用 NaiveUI 原生组件。

## 完整代码

```vue
<script setup lang="ts">

const emit = defineEmits<{
  (e: 'row-click', row: Record<string, any>): void
  (e: 'edit', row: Record<string, any>): void
  (e: 'delete', row: Record<string, any>): void
}>()

interface Props {
  data?: any[]
  loading?: boolean
}
const props = withDefaults(defineProps<Props>(), {
  data: () => [],
  loading: false,
})

// 表格列配置 - 根据字段动态生成
const tableColumns = ref([
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

// 分页配置
const pagination = ref({
  page: 1,
  pageSize: 10,
  itemCount: 0,
  showSizePicker: true,
  pageSizes: [10, 20, 50, 100],
  showQuickJumper: true,
})

function handleEdit(row: any) {
  emit('edit', row)
}

function handleDelete(row: any) {
  emit('delete', row)
}

function handleRowClick(row: any) {
  emit('row-click', row)
}
</script>

<template>
  <div class="data-table-wrapper">
    <NDataTable
      :columns="tableColumns"
      :data="props.data"
      :loading="props.loading"
      :pagination="pagination"
      striped
      @update:page="pagination.page = $event"
      @update:page-size="pagination.pageSize = $event"
    />
  </div>
</template>

<style lang="scss" scoped>
.data-table-wrapper {
  padding: 20px;
}
</style>
```

## 占位符说明

| 占位符 | 说明 |
|--------|------|
| `TABLE_COLUMNS` | 根据字段列表生成列配置 |

## 与 ZTable 的区别

| 特性 | ZTable | NDataTable |
|------|--------|------------|
| 搜索表单 | 内置 | 需手动实现 |
| 分页 | 内置 | 需手动配置 |
| 数据请求 | 内置 | 需手动实现 |
| 灵活性 | 中等 | 高 |
| 代码量 | 少 | 多 |
