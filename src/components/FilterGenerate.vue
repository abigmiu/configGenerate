<script setup>
import { reactive, getCurrentInstance, ref, defineEmits } from 'vue'
import cloneDeep from 'lodash-es/cloneDeep'

const emit = defineEmits(['updateCode'])

const instance = getCurrentInstance()

const filterRef = ref(null)

const dialog = reactive({
    filter: false,
})

const filterTypes = [
    {
        label: 'text 文本',
        value: 'text',
    },
    {
        label: 'select 选择',
        value: 'select',
    },
    {
        label: 'date 日期',
        value: 'date',
    },
    {
        label: 'daterange 日期范围',
        value: 'daterange',
    },
    {
        label: 'datetime 日期时间',
        value: 'datetime',
    },
    {
        label: 'datetimerange 日期时间范围',
        value: 'datetimerange',
    },
    {
        label: '单层地区',
        value: 'districtsSelector',
    },
    {
        label: '多层地区',
        value: 'districtsCascader',
    },
]

const filterBaseConfig = reactive({
    type: '',
    name: '',
    value: '',
    placeholder: '',
    multiple: '',
    filterable: '',
    options: [],
    connect: '',
})
const filterList = []
const handleAdd = () => {
    dialog.filter = true
}
const handleDialogClose = (done) => {
    dialog.filter = false
    done()
}

const selectOptions = reactive([])
const handleTypeChange = (val) => {
    filterBaseConfig.type = val
}
const handleAddSelectOption = () => {
    selectOptions.push({
        name: '',
        value: '',
    })
}


const resetForm = () => {
    filterRef.value.resetFields()
}
const handleAddConfig = () => {
    filterRef.value.validate((valid) => {
        if (valid) {
            const obj = Object.create(null)
            for (const key in filterBaseConfig) {
                if (Object.prototype.hasOwnProperty.call(filterBaseConfig, key)) {
                    if (filterBaseConfig[key].length > 0) {
                        obj[key] = filterBaseConfig[key]
                    }
                }
            }
            if (obj.type === 'select') {
                obj.options = [...selectOptions]
                selectOptions.length = 0
            }
            filterList.push(cloneDeep(obj))
            emit('updateCode', filterList)
            resetForm()
        } else {
            console.error('valid error')
        }
    })

}

instance.render = () => {
    return (
        <div>
            <el-button type="primary" size="mini" onClick={handleAdd}>
                新增filter
            </el-button>

            <el-dialog
                model-value={dialog.filter}
                before-close={handleDialogClose}>
                <el-form model={filterBaseConfig} ref={filterRef}>
                    <el-form-item label="type" prop="type" required>
                        <el-radio-group
                            model-value={filterBaseConfig.type}
                            onChange={handleTypeChange}>
                            {filterTypes.map((type) => {
                                return (
                                    <el-radio-button label={type.value}>
                                        {type.label}
                                    </el-radio-button>
                                )
                            })}
                        </el-radio-group>
                    </el-form-item>

                    <el-form-item label="name" prop="name" required>
                        <el-input
                            modelValue={filterBaseConfig.name}
                            onInput={(val) =>
                                filterBaseConfig.name = val
                            }></el-input>
                    </el-form-item>

                    <el-form-item label="value" prop="value" required>
                        <el-input
                            modelValue={filterBaseConfig.value}
                            onInput={(val) =>
                                filterBaseConfig.value = val
                            }></el-input>
                    </el-form-item>

                    <el-form-item label="option">
                        <el-button size="mini" onClick={handleAddSelectOption}>
                            新增option
                        </el-button>
                        {selectOptions.map((option) => {
                            return (
                                <div>
                                    <el-input
                                        modelValue={option.name}
                                        onInput={(val) => option.name = val}
                                        placeholder="name"></el-input>
                                    <el-input
                                        modelValue={option.value}
                                        onInput={(val) =>
                                            option.value = val
                                        }></el-input>
                                    <el-divider></el-divider>
                                </div>
                            )
                        })}
                    </el-form-item>

                    <el-form-item label="multiple" v-show={filterBaseConfig.type === 'select'}>
                        <el-radio-group
                            modelValue={filterBaseConfig.multiple}
                            onChange={(val) => filterBaseConfig.multiple = val}
                        >
                            <el-radio label={true}></el-radio>
                            <el-radio label={false}></el-radio>
                        </el-radio-group>
                    </el-form-item>
                    <el-form-item label="filterable" v-show={filterBaseConfig.type === 'select'}>
                        <el-radio-group
                            modelValue={filterBaseConfig.filterable}
                            onChange={(val) => filterBaseConfig.filterable = val}
                        >
                            <el-radio label={true}></el-radio>
                            <el-radio label={false}></el-radio>
                        </el-radio-group>
                    </el-form-item>
                    <el-form-item label="connect" v-show={filterBaseConfig.type === 'daterange' || filterBaseConfig === 'datetimerange'}>
                        <el-input
                            modelValue={filterBaseConfig.connect}
                            onInput={(val) => filterBaseConfig.connect = val}
                        >
                        </el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button
                            type="primary"
                            size="mini"
                            onClick={handleAddConfig}>
                            确认
                        </el-button>
                    </el-form-item>
                </el-form>
            </el-dialog>
        </div>
    )
}
</script>
