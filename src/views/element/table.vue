<template>
  <pre>tableLayout使用详情见使用文档，这其实是另一个小得开源项目。(tip: 文档编写中)</pre>
  <tableLayout ref="layout" :columns="columns" :form-item="formItem" :select-item="selectItem" :rules="rules"
    :get="getData" :add="addData" :edit-data="editGetData" :edit="editData" :del="delData" :options="options"
    :replace-fields="{ id: 'user_id' }" :row-selection="{ selectedRowKeys, onChange: onSelectChange }"
    @editOpen="editOpen" @addOpen="addOpen" @editSuccess="editSuccess" @addSuccess="addSuccess">
    <template #button>
      <a-button style="margin-left: 20px" @click="toDetail">
        插槽按钮
      </a-button>
    </template>
    <template #operationMore="item">
      <a-divider type="vertical" />
      <a-dropdown>
        <a class="ant-dropdown-link" @click.prevent>更多</a>
        <template #overlay>
          <a-menu>
            <a-menu-item>
              <a @click="editPassword(item)">修改密码</a>
            </a-menu-item>
          </a-menu>
        </template>
      </a-dropdown>
    </template>

    <template #status="item">
      测试 {{ item.value.name }}
    </template>

    <template #custom="data">
      <a-input v-model:value="data.formData[data.key]" />
    </template>
  </tableLayout>
</template>

<script lang="ts" setup name="el_table">
import { useRouter } from 'vue-router'
import { addData, delData, editData, editGetData, getData, options, upload } from '@/api/table'
import tableLayout from '@/components/tableLayout/tableLayout.vue'

type Key = string | number

// 列表
const columns = [
  { title: '序号', dataIndex: 'id' },
  { title: '键名转换', dataIndex: 'user_id' },
  { title: '姓名', dataIndex: 'name' },
  { title: '年龄', dataIndex: 'age' },
  { title: '住址', dataIndex: 'addr' },
  { title: '手机号', dataIndex: 'phone' },
  { title: '行业', dataIndex: 'industry' },
  { title: '净资产(亿元)', dataIndex: 'wealth' },
  { title: '状态', key: 'status' },
]

// 表单
const formItem = [
  { title: '姓名', key: 'name', type: 'input' },
  { title: '年龄', key: 'age', type: 'number' },
  { title: '住址', key: 'addr', type: 'input' },
  { title: '手机号', key: 'phone', type: 'input' },
  { title: '行业', key: 'industry', type: 'input' },
  {
    title: '净资产',
    key: 'wealth',
    type: 'select',
    options: [
      { value: 10, label: '10亿' }, { value: 20, label: '20亿' },
      { value: 30, label: '30亿' }, { value: 40, label: '40亿' },
      { value: 999, label: '50亿+' },
    ],
  },
  { title: '头像', key: 'avatar', type: 'upload', upload },
  { title: '车型', key: 'cat', type: 'checkbox', options: [], optionKey: 'cat' },
  { title: '备注', key: 'content', type: 'textarea', itemWidth: 'calc(100% - 20px)', labelCol: 3 },
  { title: '表单插槽', key: 'formnet', type: 'slot', slotName: 'custom' },
]

// 筛选
const selectItem = ref([
  { title: '姓名', key: 'name', type: 'input', itemWidth: '290px' },
  { title: '年龄', key: 'age', type: 'number', itemWidth: '290px', defaultVal: 20 },
  { title: '住址', key: 'addr', type: 'input', itemWidth: '290px' },
  { title: '日期', key: 'time', type: 'rangePicker', itemWidth: '290px' },
  { title: '行业', key: 'industry', type: 'select', options: [], optionKey: 'industry', itemWidth: '290px' },
])

// 规则
const rules = {
  name: [{ required: true, message: '请输入姓名', trigger: 'change' }],
  age: [{ required: true, message: '请输入年龄', trigger: 'change', type: 'number' }],
  addr: [{ required: true, message: '请输入地址', trigger: 'change' }],
  phone: [{ required: true, message: '请输入手机号', trigger: 'change' }],
  industry: [{ required: true, message: '请输入行业', trigger: 'change', type: 'string' }],
  wealth: [{ required: true, message: '请输入净资产', trigger: 'change', type: 'number' }],
  formnet: [{ required: true, message: '请输入表单插槽', trigger: 'change' }],
}

// 改密
const editPassword = (item: any) => {
  console.log(item)
}

// 多选
const selectedRowKeys = ref<any[]>([])
const onSelectChange = (keys: Key[]) => {
  selectedRowKeys.value = keys
}

// 详情
const router = useRouter()
const toDetail = () => {
  router.push({ path: '/element/detail', query: { id: Math.floor(Math.random() * 100) } })
}

// 事件钩子
const editOpen = (data: any) => {
  console.log('编辑弹框打开，并但会编辑数据')
  console.log(data)
}

const addOpen = () => {
  console.log('添加弹框打开，无数据')
}

const editSuccess = () => {
  console.log('编辑提交成功，无数据')
}

const addSuccess = () => {
  console.log('添加提交成功，无数据')
}
</script>

<style lang="scss" scoped>
pre {
  background-color: #f6f6f6;
  padding: 10px;
  margin: 10px;
  display: block;
}
</style>
