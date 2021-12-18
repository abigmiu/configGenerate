<template>
    <div style="margin-top: 20px">
        <el-button type="primary" size="mini" @click="handleAddList">
            新增list
        </el-button>

        <el-dialog v-model="dialog.list">
            <el-form
                ref="listFormRef"
                :model="listBaseForm"
                label-width="80px"
                label-position="left"
            >
                <el-form-item label="type" prop="type" required>
                    <el-radio-group v-model="listBaseForm.type" size="mini">
                        <el-radio-button
                            v-for="(item, index) in listTypes"
                            :key="index"
                            :label="item.value"
                        >
                            {{ item.label }}
                        </el-radio-button>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="name" prop="name" required>
                    <el-input v-model="listBaseForm.name"></el-input>
                </el-form-item>
                <el-form-item label="value" prop="value" required>
                    <el-input v-model="listBaseForm.value"></el-input>
                </el-form-item>
                <el-form-item label="width" prop="width">
                    <el-input-number
                        v-model.number="listBaseForm.width"
                    ></el-input-number>
                </el-form-item>
                <el-form-item
                    label="hash options"
                    v-if="listBaseForm.type === 'hash'"
                >
                    <el-button size="mini" @click="handleAddListOption"
                        >新增option</el-button
                    >
                    <div v-for="(option, index) in listHashOption" :key="index">
                        <el-input
                            v-model="option.key"
                            placeholder="key"
                        ></el-input>
                        <el-input
                            v-model="option.value"
                            placeholder="value"
                        ></el-input>
                        <el-divider></el-divider>
                    </div>
                </el-form-item>
                <el-form-item
                    label="array options"
                    v-if="listBaseForm.type === 'array'"
                >
                    <el-input
                        v-model="listArrayOption[0]"
                        placeholder="true"
                    ></el-input>
                    <el-input
                        v-model="listArrayOption[1]"
                        placeholder="false"
                    ></el-input>
                </el-form-item>

                <el-form-item
                    label="shape"
                    v-if="listBaseForm.type === 'avatar'"
                    prop="shape"
                >
                    <el-radio-group v-model="listBaseForm.shape">
                        <el-radio label="circle">circle</el-radio>
                        <el-radio label="square">square</el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item
                    label="fit"
                    v-if="listBaseForm.type === 'avatar'"
                    prop="fit"
                >
                    <el-radio-group v-model="listBaseForm.fit">
                        <el-radio label="fill">fill</el-radio>
                        <el-radio label="contain">contain</el-radio>
                        <el-radio label="cover">cover</el-radio>
                        <el-radio label="none">none</el-radio>
                        <el-radio label="scale-down">scale-down</el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item
                    label="size"
                    v-if="listBaseForm.type === 'avatar'"
                    prop="size"
                >
                    <el-select
                        v-model="listBaseForm.size"
                        filterable
                        allow-create
                        default-first-option
                    >
                        <el-option value="large" label="large"></el-option>
                        <el-option value="medium" label="medium"></el-option>
                        <el-option value="small" label="small"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item>
                    <el-button
                        type="primary"
                        size="mini"
                        @click="handleAddListConfigItem"
                    >
                        确认
                    </el-button>
                </el-form-item>
            </el-form>
        </el-dialog>
    </div>
</template>

<script setup>
import { reactive, ref, defineEmits } from 'vue'

const emit = defineEmits(['updateCode'])

const listFormRef = ref(null)

const dialog = reactive({
    list: false,
})

const listData = reactive([])

const listBaseConfig = reactive({
    type: '',
    name: '',
    value: '',
    options: [],
    width: '',
    align: '',
    shape: '',
    fit: '',
})
const listTypes = [
    {
        label: 'text 文本',
        value: 'text',
    },
    {
        label: 'avatar 头像',
        value: 'avatar',
    },
    {
        label: 'image 图像',
        value: 'image',
    },
    {
        label: 'hash',
        value: 'hash',
    },
    {
        label: 'array: 数组',
        value: 'array',
    },
    {
        label: 'datetime 日期时间',
        value: 'datetime',
    },
    {
        label: 'boolean 布尔',
        value: 'boolean',
    },
    {
        label: 'slot vip定制',
        value: 'slot',
    },
]

const listHashOption = reactive([])
const listArrayOption = reactive([])
let listBaseForm = listBaseConfig
/**
 * 点击添加listConfig
 */
const handleAddList = () => {
    dialog.list = true
}
/**
 * 点击添加hashOption
 */
const handleAddListOption = () => {
    listHashOption.push({
        key: '',
        value: '',
    })
}

/**
 * 提交列表configItem
 */
const handleAddListConfigItem = () => {
    listFormRef.value.validate((valid) => {
        if (valid) {
            const obj = {}
            for (const key in listBaseForm) {
                if (Object.hasOwnProperty.call(listBaseForm, key)) {
                    if (listBaseForm[key].length !== 0) {
                        obj[key] = listBaseForm[key]
                    }
                }
            }

            if (listBaseConfig.width === 0) {
                delete obj.width
            } else {
                obj.width = obj.width + 'px'
            }
            if (listBaseForm.type === 'hash') {
                const hashObj = {}
                listHashOption.forEach(option => {
                    const { key, value } = option
                    hashObj[key] = value
                })
                obj.options = hashObj
                listHashOption.lenght = 0
            }
            if (listBaseForm.type === 'array') {
                obj.options = [...listArrayOption]
                listArrayOption[0] = ''
                listArrayOption[1] = ''
            }

            listFormRef.value.resetFields()
            listData.push(obj)
            emit('updateCode', listData)
        } else {
            return false;
        }
    })
}
</script>

<style></style>
