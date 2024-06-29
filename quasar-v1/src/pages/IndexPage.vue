<template>
  <q-page class="tw-py-4 tw-pr-4 tw-flex tw-flex-col ">
    <div class="tw-h-full tw-grow tw-w-full glass-body glass-border tw-rounded-2xl tw-overflow-hidden tw-flex tw-flex-col">
      <div class="tw-p-4 tw-shrink-0 tw-flex tw-items-center tw-justify-between">
        <div class="">
          <div class="tw-text-2xl tw-font-bold">
            JSON Formatter
          </div>
          <div class="tw-font-bold tw-text-gray-400">
            Format any json.
          </div>
        </div>
        <ActionButtons />
      </div>
      <div class="tw-grid tw-grid-cols-2 tw-gap-4 tw-px-4 tw-grow tw-pb-4">
        <div class=" tw-flex tw-flex-col tw-gap-4">
          <div class="tw-shrink-0 tw-flex tw-gap-2">
            <q-btn @click="add_sample" no-caps unelevated size="1.1em" class="tw-rounded-lg tw-bg-white/20 ">
              <div class="tw-flex tw-justify-between tw-gap-2 tw-items-center">
                <span>Sample JSON</span>
              </div>
              <q-tooltip>
                Add a sample URL
              </q-tooltip>
            </q-btn>
            <q-toggle
              v-model="dynamic_mode"
              label="Dynamic"
            />
          </div>
          <div class="theme-json-editor tw-grow tw-flex tw-flex-col tw-max-h-full">
            <!-- <q-input v-model="input_text" type="textarea" borderless class="tw-grow" placeholder="Input here . . .">

            </q-input> -->
            <JsonEditorVue v-model="input_text" mode="text" :navigationBar="false" class="tw-grow tw-rounded-lg tw-overflow-hidden" />
          </div>
        </div>
        <div class=" tw-flex tw-flex-col tw-gap-4">
          <div class="tw-shrink-0 tw-flex tw-justify-end tw-gap-2">
            <q-btn @click="input_text = '', output_text = ''" unelevated size="1.1em" class="tw-rounded-lg tw-bg-white/20 tw-text-red-400">
              <div class="tw-flex tw-justify-between tw-gap-2 tw-items-center">
                <span>Clear</span>
                <svg xmlns="http://www.w3.org/2000/svg" class="tw-h-6 tw-w-6" viewBox="0 0 24 24"><path fill="currentColor" d="M6.4 19L5 17.6l5.6-5.6L5 6.4L6.4 5l5.6 5.6L17.6 5L19 6.4L13.4 12l5.6 5.6l-1.4 1.4l-5.6-5.6L6.4 19Z"/></svg>
              </div>
              <q-tooltip>
                Clear all
              </q-tooltip>
            </q-btn>
            <q-btn @click="handle_action" unelevated size="1.1em" class="tw-rounded-lg tw-bg-green-700">
              <div class="tw-flex tw-justify-between tw-gap-2 tw-items-center">
                <span>Format</span>
                <svg xmlns="http://www.w3.org/2000/svg" class="tw-h-6 tw-w-6" viewBox="0 0 24 24"><path fill="currentColor" d="M5.825 16L7.7 17.875q.275.275.275.688t-.275.712q-.3.3-.713.3t-.712-.3L2.7 15.7q-.15-.15-.213-.325T2.426 15q0-.2.063-.375T2.7 14.3l3.6-3.6q.3-.3.7-.287t.7.312q.275.3.288.7t-.288.7L5.825 14H12q.425 0 .713.288T13 15q0 .425-.288.713T12 16H5.825Zm12.35-6H12q-.425 0-.713-.288T11 9q0-.425.288-.713T12 8h6.175L16.3 6.125q-.275-.275-.275-.688t.275-.712q.3-.3.713-.3t.712.3L21.3 8.3q.15.15.212.325t.063.375q0 .2-.063.375T21.3 9.7l-3.6 3.6q-.3.3-.7.288t-.7-.313q-.275-.3-.288-.7t.288-.7L18.175 10Z"/></svg>
              </div>
              <q-tooltip>
                Format JSON
              </q-tooltip>
            </q-btn>
          </div>
          <div class="tw-grow tw-flex tw-flex-col tw-ov">
            <q-input ref="output" v-model="output_text" readonly type="textarea" borderless class="tw-max-h-full tw-grow" placeholder="Output will be here . . .">
              <template v-slot:append>
                <q-btn @click="copy" dense flat class="tw-rounded-lg tw-p-2 tw-bg-black/20">
                  <q-icon name="content_copy"></q-icon>
                  <q-tooltip>
                    Copy
                  </q-tooltip>
                </q-btn>
              </template>
            </q-input>
          </div>
        </div>
      </div>
    </div>
  </q-page>
</template>
<script>
import { defineComponent, ref } from 'vue'
import ActionButtons from 'src/components/ActionButtons.vue'
import JsonEditorVue from 'json-editor-vue'

export default defineComponent({
  name: 'JsonFormatter',
  components:{
    ActionButtons,
    JsonEditorVue,
  },
  setup () {
    return {
      convert_option:ref('Encode'),
      input_text:ref(''),
      output_text:ref(''),
      config : {
        type: 'space',
        size: 2
      },
      dynamic_mode: ref(false),
    }
  },
  methods:{
    add_sample(){
      this.input_text = `{"label": "Name", "value": "", "rules": ["((val)=> (val && val.length > 0) || 'Field is required')"]}`
    },
    handle_action(){
      this.output_text = this.convert_to_q_input(this.input_text)
      // JSON.stringify(JSON.parse(), undefined, '\t')
    },
    convert_to_q_input(input){
      let json = JSON.parse(input)
      if(!json.length){
        json = [json]
      }
      let output = ''
      json.forEach(element => {
        let curr = '<q-input'
        for (const [key, value] of Object.entries(element)) {
          curr += `\n\t ${this.key_handler(key)}=${this.value_handler(key, value)}`
        }
        curr += '\n></q-input>'
        output += curr + '\n'
      })
      return output
    },
    key_handler(key){
      let out = ''
      switch (key) {
        case 'value':
          out = 'v-model'
          break
        default:
          out = key
          break
      }
      if(this.dynamic_mode && !['v-model'].includes(out)){
        return ':'+out
      }else{
        return out
      }
    },
    value_handler(key, value, parent_var='my_var'){
      if(this.dynamic_mode){
        return `"${parent_var}.${key}"`
      }
      let out = ''
      switch (key) {
        case 'rules':
          value = value.map((rule) => {
            if(typeof rule === 'string'){
              rule = rule.replace(/^"(.*)"$/, (match, p1) => p1.replace(/^"|"$/g, ''))
              return '\n\t\t' + rule
            }
            return rule
          })
          out = `[${value}\n\t]`
          break
        default:
          out = `"${value}"`
          break
      }
      return out
    },
    copy(){
      this.$refs.output.select()
      navigator.clipboard.writeText(this.output_text)
      this.$q.notify({
        type:'positive',
        message:"Copied to clipboard"
      })
    },
  },
})
</script>
<style>
.theme-json-editor{
  --jse-background-color: rgba(255, 255, 255, 0.2);
  --jse-theme-color: rgba(255, 255, 255, 0.2);
  --jse-text-color: white;
  --jse-main-border: 0px solid;
  --jse-panel-background: transparent;
  --jse-panel-border: rgba(255, 255, 255, 0.2);
  --jse-delimiter-color: #fff;
  --indent-markers: rgba(255, 255, 255, 0.2);
  --indent-marker-active-bg-color: rgba(255, 255, 255, 0.2);
  --internal-key-color: #fff;
  --internal-value-color-string: #fff;
  --internal-value-color-number: #0ff;
  --jse-panel-background: rgba(255, 255, 255, 0.2);
}
.theme-json-editor .jse-menu{
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}
.theme-json-editor .jse-selected{
  color: black !important;
}
.theme-json-editor .cm-gutters{
  border-right: 1px solid rgba(255, 255, 255, 0.2) !important;
}
.theme-json-editor .ͼo{
  color: #fffb;
}
.theme-json-editor .ͼr{
  color: #0ff;
}
.theme-json-editor .ͼp{
  color: #0ff;
}
.theme-json-editor .jse-button[title="Switch to text mode (current mode: text)"],
.theme-json-editor .jse-button[title="Switch to tree mode (current mode: text)"],
.theme-json-editor .jse-button[title="Switch to table mode (current mode: text)"],
.theme-json-editor .jse-button[title="Switch to table mode (current mode: text)"]+.jse-separator,
.theme-json-editor .jse-button[title="Compact JSON: remove all white spacing and new lines (Ctrl+Shift+I)"]+.jse-separator,
.theme-json-editor .jse-button.jse-sort,
.theme-json-editor .jse-button.jse-transform,
.theme-json-editor .jse-button.jse-search{
  display: none !important;
}
.theme-json-editor .jse-status-bar{
  border-top: 1px solid rgba(255, 255, 255, 0.2) !important;
}

</style>
