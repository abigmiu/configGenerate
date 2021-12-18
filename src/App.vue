<template>
    <div>
        <list-generate @updateCode="updateCode"></list-generate>
        <filter-generate @updateCode="updateCode"></filter-generate>
        <form-generate @updateCode="updateCode"></form-generate>
        <div style="margin-top: 20px">
            <pre v-highlightjs="codeVal"><code class="javascript"></code></pre>

             {{ codeVal }}
        </div>
    </div>
</template>
<script setup>
import {  ref } from 'vue'
import ListGenerate from './components/ListGenerate.vue'
import FilterGenerate from './components/FilterGenerate.vue'
import FormGenerate from './components/FormGenerate.vue'
import { js_beautify } from 'js-beautify'


let codeVal = ref('const a = []')

const walk = (value) => {
    let str = ''
    if (Array.isArray(value)) {
        str += '['
        value.forEach((item) => {
            str+= `${walk(item)},`
        })
        str += ']'
    } else if (Object.prototype.toString.call(value) === '[object Object]') {
        str += '{'
        for (const [key, value] of Object.entries(value)) {
            str += `${key}:${walk(value)},`
        }
        str += '}'
    } else {
        str += `'${value}'`
    }
    return str
}


const updateCode = (val) => {
    const walkRes = walk(val);
    codeVal.value = js_beautify(walkRes)
}


</script>
