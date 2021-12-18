<script setup>
import { getCurrentInstance, reactive, ref, defineEmits } from 'vue'
const emit = defineEmits(['updateCode'])
const formRef = ref(null)
import cloneDeep from 'lodash-es/cloneDeep'
const instance = getCurrentInstance()
const dialog = reactive({
    form: false,
})

const handleAddBtn = () => {
    dialog.form = true
}
const formList = []
const formBaseConfig = reactive({
    type: '',
    name: '',
    value: '',
    require: '',
    placeholder: '',
    message: '',
    rows: '',
    autosize: '',
    readonly: '',
    disabled: '',
    maxlength: '',
    min: '',
    max: '',
    options: [],
    connect: '',
    multiple: '',
    accept: '',
    props: '',
    tag: '',
})

const formTypes = [
    {
        label: 'text 文本',
        value: 'text',
    },
    {
        label: 'textarea 文本域',
        value: 'textarea',
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
        label: 'date 日期范围',
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
        label: 'switch 切换',
        value: 'switch',
    },
    {
        label: 'checkbox 选择',
        value: 'checkbox',
    },
    {
        label: 'radio 单选',
        value: 'radio',
    },
    {
        label: 'cascader 级联选择',
        value: 'cascader',
    },
    {
        label: 'upload 上传',
        value: 'upload',
    },
    {
        label: '富文本',
        value: 'richText',
    },
    {
        label: '地区选择',
        value: 'districtsSelector',
    },
    {
        label: '级联地区',
        value: 'districtsCascader',
    },
    {
        label: '插槽',
        value: 'slot',
    },
]

const radioOptions = []

const addRadioOption = () => {
    radioOptions.push({
        name: '',
        value: '',
    })
}

const selectOptions = []
const addSelectOption = () => {
    selectOptions.push({
        name: '',
        value: '',
    })
}

const confirmAdd = () => {
    formRef.value.validate((valid) => {
        const numAry = ['rows', 'maxlength', 'min', 'max']
        if (valid) {
            const obj = Object.create(null)
            for (const key in formBaseConfig) {
                if (Object.prototype.hasOwnProperty.call(formBaseConfig, key)) {
                    if (formBaseConfig[key].length > 0) {
                        obj[key] = formBaseConfig[key]
                    }
                }
            }
            numAry.forEach((key) => {
                if (formBaseConfig[key] !== 0) {
                    obj[key] = formBaseConfig[key]
                }
            })

            if (obj.type === 'radio') {
                obj.options = cloneDeep(radioOptions)
                radioOptions.length = []
            }
            if (obj.type === 'select') {
                obj.options = cloneDeep(selectOptions)
                selectOptions.length = []
            }

            formList.push(cloneDeep(obj))
            emit('updateCode', formList)
            formRef.value.resetFields()
        } else {
            console.error('valid error')
        }
    })
}

instance.render = () => {
    return (
        <div>
            <el-button type="primary" size="mini" onClick={handleAddBtn}>
                新增form
            </el-button>

            <el-dialog
                modelValue={dialog.form}
                beforeClose={() => (dialog.form = false)}>
                <el-form model={formBaseConfig} ref={formRef}>
                    <el-form-item label="type" prop="type" required>
                        <el-radio-group
                            modelValue={formBaseConfig.type}
                            onChange={(val) => (formBaseConfig.type = val)}>
                            {formTypes.map((type) => {
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
                            modelValue={formBaseConfig.name}
                            onInput={(val) =>
                                (formBaseConfig.name = val)
                            }></el-input>
                    </el-form-item>

                    <el-form-item label="value" prop="value" required>
                        <el-input
                            modelValue={formBaseConfig.value}
                            onInput={(val) =>
                                (formBaseConfig.value = val)
                            }></el-input>
                    </el-form-item>

                    <el-form-item label="placeholder">
                        <el-input
                            modelValue={formBaseConfig.placeholder}
                            onInput={(val) =>
                                (formBaseConfig.placeholder = val)
                            }></el-input>
                    </el-form-item>

                    <el-form-item
                        label="rows"
                        vShow={
                            formBaseConfig.type === 'text' ||
                            formBaseConfig.type === 'textarea'
                        }>
                        <el-input-number
                            modelValue={formBaseConfig.rows}
                            onInput={(val) =>
                                (formBaseConfig.rows = val)
                            }></el-input-number>
                    </el-form-item>

                    <el-form-item
                        label="autosize"
                        vShow={
                            formBaseConfig.type === 'text' ||
                            formBaseConfig.type === 'textarea'
                        }>
                        <el-switch
                            modelValue={formBaseConfig.autosize}
                            onChange={(val) =>
                                (formBaseConfig.autosize = val)
                            }></el-switch>
                    </el-form-item>

                    <el-form-item
                        label="maxlength"
                        vShow={
                            formBaseConfig.type === 'text' ||
                            formBaseConfig.type === 'textarea'
                        }>
                        <el-input-number
                            modelValue={formBaseConfig.maxlength}
                            onInput={(val) =>
                                (formBaseConfig.maxlength = val)
                            }></el-input-number>
                    </el-form-item>

                    <el-form-item
                        label="min"
                        vShow={
                            formBaseConfig.type === 'text' ||
                            formBaseConfig.type === 'textarea'
                        }>
                        <el-input-number
                            modelValue={formBaseConfig.min}
                            onInput={(val) =>
                                (formBaseConfig.min = val)
                            }></el-input-number>
                    </el-form-item>

                    <el-form-item
                        label="max"
                        vShow={
                            formBaseConfig.type === 'text' ||
                            formBaseConfig.type === 'textarea'
                        }>
                        <el-input-number
                            modelValue={formBaseConfig.max}
                            onInput={(val) =>
                                (formBaseConfig.max = val)
                            }></el-input-number>
                    </el-form-item>

                    <el-form-item
                        label="connect"
                        vShow={
                            formBaseConfig.type === 'datetange' ||
                            formBaseConfig.type === 'datetimerange'
                        }>
                        <el-input
                            modelValue={formBaseConfig.connect}
                            onInput={(val) =>
                                (formBaseConfig.connect = val)
                            }></el-input>
                    </el-form-item>

                    <el-form-item
                        label="options"
                        vShow={formBaseConfig.type === 'radio'}>
                        <el-button size="mini" onClick={addRadioOption}>
                            新增option
                        </el-button>
                        {radioOptions.map((option) => {
                            return (
                                <div>
                                    <el-input
                                        modelValue={option.name}
                                        onInput={(val) =>
                                            (option.name = val)
                                        }></el-input>
                                    <el-input
                                        modelValue={option.value}
                                        onInput={(val) =>
                                            (option.value = val)
                                        }></el-input>
                                    <el-divider></el-divider>
                                </div>
                            )
                        })}
                    </el-form-item>

                    <el-form-item
                        label="options"
                        vShow={formBaseConfig.type === 'select'}>
                        <el-button size="mini" onClick={addSelectOption}>
                            新增option
                        </el-button>
                        {selectOptions.map((option) => {
                            return (
                                <div>
                                    <el-input
                                        modelValue={option.name}
                                        onInput={(val) =>
                                            (option.name = val)
                                        }></el-input>
                                    <el-input
                                        modelValue={option.value}
                                        onInput={(val) =>
                                            (option.value = val)
                                        }></el-input>
                                    <el-divider></el-divider>
                                </div>
                            )
                        })}
                    </el-form-item>

                    <el-form-item
                        label="accept"
                        vShow={formBaseConfig.type === 'upload'}>
                        <el-input
                            modelValue={formBaseConfig.accept}
                            onInput={(val) =>
                                (formBaseConfig.accept = val)
                            }></el-input>
                    </el-form-item>

                    <el-form-item
                        label="tag"
                        vShow={formBaseConfig.type === 'upload'}>
                        <el-select
                            filterable
                            allow-create
                            default-first-option
                            modelValue={formBaseConfig.tag}
                            onChange={(val) => (formBaseConfig.tag = val)}>
                            <el-option label="video" value="video"></el-option>
                            <el-option label="audeo" value="audeo"></el-option>
                            <el-option
                                label="application"
                                value="application"></el-option>
                            <el-option label="image" value="image"></el-option>
                        </el-select>
                    </el-form-item>

                    <el-form-item>
                        <el-button
                            type="primary"
                            size="mini"
                            onClick={confirmAdd}>
                            确认
                        </el-button>
                    </el-form-item>
                </el-form>
            </el-dialog>
        </div>
    )
}
</script>
